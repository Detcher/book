# Chapter 6

1. P334 "In multithreaded applications, several threads—which are quite possibly sharing data—are running in parallel on different processing cores." threads可以在不同core上跑  
2. process之间不能交叉运行，否则会出现race condition，由此引出了"critical section problem"，交叉的反面是-顺序，也就是要让process之间同步(synchronize)起来以顺序执行，cooperatively share data；因此，需要一个protocol以让所有的process/threads/task遵循这个协议，比如一个process在critical section时，其他process不能执行critical section，通过这个protocol达到process synchronization的目的  
   > 解决**critical-section problem**的方法：
      > - **single-core**: prevent interrupts
      >     - **deficiency**: 在多核情况下不适用，因为会有delay，降低system efficiency；同时还有一个致命因素，"Also consider the effect on a system’s clock if the clock is kept updated by interrupts."，显然，对于通过interrupt更新clock的系统来说，disable interrupts是致命的  
      > - **kernel perspective**:
      >     - **nonpreemptive kernels**: free from race conditions  
      >     - **preemptive kernels**: stay unknown right now:(  
      > - **software**: Peterson's Solution
      >     - **deficiency**: up to the compiler and OS because of their efforts for optimization like reordering instructions, which is quite dangerous for multithreaded programs
      > - **hardware**:
      >     - memory barriers: memory_barrier() (can't continue until everything before memory_barrier() is done)
      >     - test_and_set()/compare_and_swap() (in x86 through locking the bus until something is done)
      >     - atomic variables/functions: 
3. 关于单核情况下禁止中断即可解决CSP的原因分析：要解决CSP，一个思路就是在执行一个task时，不发生context-switch，即不会发生交叉执行的情况，也即禁止掉context-switch即可；由Wiki得知，There are three potential triggers for a context switch: `A. multitasking`/`B. interrupt handling`/`C. user and kernel mode switching`；也就是说，分别禁止掉三种源即可禁止掉context-switch；分别来看，对于B是显然的，禁止中断即可；对于C，在调用system call时会发生mode switching，通过访管指令进行，而访管指令会引发软中断，所以感觉本质上也是禁止中断即可；对于A，Wiki里说"On a pre-emptive multitasking system, the scheduler may also switch out processes that are still runnable. To prevent other processes from being starved of CPU time, pre-emptive schedulers often configure a timer interrupt to fire when a process exceeds its time slice. This interrupt ensures that the scheduler will gain control to perform a context switch."，注意到了timer interrupt和scheduler的协同工作，所以感觉本质上也是禁止中断即可；综上，在单核情况下禁止中断即可解决CSP。btw, only interrupts will make a process switch context?  
   > <https://en.wikipedia.org/wiki/Context_switch>
4. 