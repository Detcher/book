# Chapter 6

1. P334 "In multithreaded applications, several threads—which are quite possibly sharing data—are running in parallel on different processing cores." threads可以在不同core上跑  
2. process之间不能交叉运行，否则会出现race condition，由此引出了"critical section problem"，交叉的反面是-顺序，也就是要让process之间同步(synchronize)起来以顺序执行，cooperatively share data；因此，需要一个protocol以让所有的process/threads/task遵循这个协议，比如一个process在critical section时，其他process不能执行critical section，通过这个protocol达到process synchronization的目的  
   > 解决**critical-section problem**的方法：
      >
      > - **single-core**: prevent interrupts
      >     - **deficiency**: 在多核情况下不适用，因为会有delay，降低system efficiency；同时还有一个致命因素，"Also consider the effect on a system’s clock if the clock is kept updated by interrupts."，显然，对于通过interrupt更新clock的系统来说，disable interrupts是致命的  
      > - **kernel perspective**:
      >     - **nonpreemptive kernels**: free from race conditions  
      >     - **preemptive kernels**: stay unknown right now:(  
      > - **software-based**: Peterson's Solution
      >     - **deficiency**: not guaranteed to work on modern computer architectures, up to the compiler and OS because of their efforts for optimization like reordering instructions, which is quite dangerous for multithreaded programs
      > - **hardware**:
      >     - **memory barriers**: memory_barrier() (can't continue until everything before memory_barrier() is done)
      >     - **test_and_set()/compare_and_swap()** (in x86 through locking the bus until something is done)
      >     - **atomic variables/functions**: using CAS as building-blocks
      >        - **deficiency**: provide atomic updates, but can't apply to all circumstances with race conditions; limited use
      > - **software**:
      >     - **mutex locks**
      >        - **spinlock**
      >     - **semaphores**
3. 关于单核情况下禁止中断即可解决CSP的原因分析：要解决CSP，一个思路就是在执行一个task时，不发生context-switch，即不会发生交叉执行的情况，也即禁止掉context-switch即可；由Wiki得知，There are three potential triggers for a context switch: `A. multitasking`/`B. interrupt handling`/`C. user and kernel mode switching`；也就是说，分别禁止掉三种源即可禁止掉context-switch；分别来看，对于B是显然的，禁止中断即可；对于C，在调用system call时会发生mode switching，通过访管指令进行，而访管指令会引发软中断，所以感觉本质上也是禁止中断即可；对于A，Wiki里说"On a pre-emptive multitasking system, the scheduler may also switch out processes that are still runnable. To prevent other processes from being starved of CPU time, pre-emptive schedulers often configure a timer interrupt to fire when a process exceeds its time slice. This interrupt ensures that the scheduler will gain control to perform a context switch."，注意到了timer interrupt和scheduler的协同工作，所以感觉本质上也是禁止中断即可；综上，在单核情况下禁止中断即可解决CSP。btw, only interrupts will make a process switch context?  
   > <https://en.wikipedia.org/wiki/Context_switch>
4. 系统调用是一种软中断处理程序，从用户态转到内核态并执行相应操作，在大多数系统里，system calls只能被处于用户态的进程调用，当然也有一些特殊情况。"首先纠正一点，没有x8086，只有8086汇编（16位）或者x86汇编（32位）；一般来说，OS会把virtual memory分成用户空间和内核空间 (用户态和内核态)，而保护模式的出现引入了virtual memory的概念。8086里的int 21h，一般叫做DOS中断，因为int 21h工作在实模式下，实模式没有保护模式，也就没有内核态的概念。所以，DOS中断一般不叫做系统调用，我见过把它叫做调用系统服务的说法。只有在x86保护模式下，才有系统调用的说法。但保护模式下是没有int 21h的，Linux的系统调用用的是int 80h，使用`int 80h/21h`的同时将子功能号压入`AH/%eax`。"A system call does not generally require a context switch to another process; instead, it is processed in the context of whichever process invoked it."  
   > <https://stackoverflow.com/questions/33654579/in-an-operating-system-what-is-the-difference-between-a-system-call-and-an-inte#:~:text=System%20calls%20are%20not%20interrupts,but%20not%20in%20an%20interrupt.>  
   > <https://en.wikipedia.org/wiki/System_call#Processor_mode_and_context_switching>  
   > <https://www.zhihu.com/question/391175281/answer/1186574827?utm_campaign=shareopn&utm_medium=social&utm_oi=1391085814059732992&utm_psn=1624406933737361408&utm_source=wechat_session>
5. P347 "Spinlocks do have an advantage, however, in that no context switch is required when a process must wait on a lock." 关于互斥锁和信号量机制的选择：注意用spinlock的时机，在与context switch的代价与占用CPU的时间之间作出权衡。Spinlocks are often identified as the locking mechanism of choice on multiprocessor systems when the lock is to be held for a short duration (less than two context switches) 一点总结：对于自旋锁，怕的就是一直在busy waiting，特别是对于单核，占着CPU不放；有时间片的情况下，上下文切换几次发现都是在busy waiting，没有做任何有效工作；对于以上缺陷，不难看出自旋锁的适用情况：第一，多核；第二，持锁时间应该短些。而信号量机制修正了自旋锁忙等的现象，如果得不到锁，就sleep，随后再wakeup，显然至少有两次上下文切换，鉴于context switch的代价较高，因此有时候也会采用忙等/自旋的机制，如前所述。  
6. P341 关于原子性地执行一条指令：atomically—that is, as one uninterruptible unit.  
7. P348 In fact, on systems that do not provide mutex locks, binary semaphores can be used instead for providing mutual exclusion. 但是互斥锁和二元信号量仍有区别，比如信号量特有的wait-list？  
8. 赋予的含义不一样，语句的顺序可能也不一样。比如信号量可以有锁的含义，也可以有资源总数的含义。以wait()为例，当锁用时，应该先while测试是否available，再去S--；而当资源总数用时，应该先S--，再while测试是否小于0，如果小于零说明资源不够了，将该进程加入相应信号量的wait list。由此看来，同样也有busy waiting和sleep的区别。  
9.  P350 In general, however, the list can use any queuing strategy. Correct usage of semaphores does not depend on a particular queuing strategy for the semaphore lists.  
10. 对于多核，既然禁止中断不可取了，就要采用其他的技巧来让wait()/signal()操作performed atomically；原文中点名对于SMP来说，必须提供CAS指令/spinlock来实现原子性执行某些操作  
11. P350 "Rather, we have moved busy waiting from the entry section to the critical sections of application programs." 没看懂，怎么就转移了？也就是说semaphore机制仍存在busy-waiting  
   > <https://stackoverflow.com/questions/58032304/why-busy-waiting-moved-from-the-entry-section-to-the-critical-sections-of-applic> 只有一个问答，但是没看懂  
12. P358 "We say that a set of processes is in a deadlocked state when every process in the set is waiting for an event that can be caused only by another process in the set" 如果这个集合中的每个process都有求于人，就会造成死锁（造成死锁的events之一，更多见Chapter 8）  
13. 6.7 Monitor暂时跳过，一种high-level language constructs  
14. P360 关于将CAS-based approaches视作optimistic approach，将Mutual exclusion locking视作pessimistic strategy的说法挺有意思的，感觉有点mnemonic的意味在  
15. P361  a reader–writer lock？？  
16. P361 对于Monitor和condition variables：However, such tools may have significant overhead and, depending on their implementation, may be less likely to scale in highly contended situations. 关于"scale in highly contended situations"？？keywords：并发编程（concurrent programming/在高竞争状态下的可扩展性问题/scalable and efficient  
