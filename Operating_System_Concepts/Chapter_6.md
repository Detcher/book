# Chapter 6

1. P334 "In multithreaded applications, several threads��which are quite possibly sharing data��are running in parallel on different processing cores." threads�����ڲ�ͬcore����  
2. process֮�䲻�ܽ������У���������race condition���ɴ�������"critical section problem"������ķ�����-˳��Ҳ����Ҫ��process֮��ͬ��(synchronize)������˳��ִ�У�cooperatively share data����ˣ���Ҫһ��protocol�������е�process/threads/task��ѭ���Э�飬����һ��process��critical sectionʱ������process����ִ��critical section��ͨ�����protocol�ﵽprocess synchronization��Ŀ��  
   > ���**critical-section problem**�ķ�����
      >
      > - **single-core**: prevent interrupts
      >     - **deficiency**: �ڶ������²����ã���Ϊ����delay������system efficiency��ͬʱ����һ���������أ�"Also consider the effect on a system��s clock if the clock is kept updated by interrupts."����Ȼ������ͨ��interrupt����clock��ϵͳ��˵��disable interrupts��������  
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
3. ���ڵ�������½�ֹ�жϼ��ɽ��CSP��ԭ�������Ҫ���CSP��һ��˼·������ִ��һ��taskʱ��������context-switch�������ᷢ������ִ�е������Ҳ����ֹ��context-switch���ɣ���Wiki��֪��There are three potential triggers for a context switch: `A. multitasking`/`B. interrupt handling`/`C. user and kernel mode switching`��Ҳ����˵���ֱ��ֹ������Դ���ɽ�ֹ��context-switch���ֱ�����������B����Ȼ�ģ���ֹ�жϼ��ɣ�����C���ڵ���system callʱ�ᷢ��mode switching��ͨ���ù�ָ����У����ù�ָ����������жϣ����Ըо�������Ҳ�ǽ�ֹ�жϼ��ɣ�����A��Wiki��˵"On a pre-emptive multitasking system, the scheduler may also switch out processes that are still runnable. To prevent other processes from being starved of CPU time, pre-emptive schedulers often configure a timer interrupt to fire when a process exceeds its time slice. This interrupt ensures that the scheduler will gain control to perform a context switch."��ע�⵽��timer interrupt��scheduler��Эͬ���������Ըо�������Ҳ�ǽ�ֹ�жϼ��ɣ����ϣ��ڵ�������½�ֹ�жϼ��ɽ��CSP��btw, only interrupts will make a process switch context?  
   > <https://en.wikipedia.org/wiki/Context_switch>
4. ϵͳ������һ�����жϴ�����򣬴��û�̬ת���ں�̬��ִ����Ӧ�������ڴ����ϵͳ�system callsֻ�ܱ������û�̬�Ľ��̵��ã���ȻҲ��һЩ���������"���Ⱦ���һ�㣬û��x8086��ֻ��8086��ࣨ16λ������x86��ࣨ32λ����һ����˵��OS���virtual memory�ֳ��û��ռ���ں˿ռ� (�û�̬���ں�̬)��������ģʽ�ĳ���������virtual memory�ĸ��8086���int 21h��һ�����DOS�жϣ���Ϊint 21h������ʵģʽ�£�ʵģʽû�б���ģʽ��Ҳ��û���ں�̬�ĸ�����ԣ�DOS�ж�һ�㲻����ϵͳ���ã��Ҽ���������������ϵͳ�����˵����ֻ����x86����ģʽ�£�����ϵͳ���õ�˵����������ģʽ����û��int 21h�ģ�Linux��ϵͳ�����õ���int 80h��ʹ��`int 80h/21h`��ͬʱ���ӹ��ܺ�ѹ��`AH/%eax`��"A system call does not generally require a context switch to another process; instead, it is processed in the context of whichever process invoked it."  
   > <https://stackoverflow.com/questions/33654579/in-an-operating-system-what-is-the-difference-between-a-system-call-and-an-inte#:~:text=System%20calls%20are%20not%20interrupts,but%20not%20in%20an%20interrupt.>  
   > <https://en.wikipedia.org/wiki/System_call#Processor_mode_and_context_switching>  
   > <https://www.zhihu.com/question/391175281/answer/1186574827?utm_campaign=shareopn&utm_medium=social&utm_oi=1391085814059732992&utm_psn=1624406933737361408&utm_source=wechat_session>
5. P347 "Spinlocks do have an advantage, however, in that no context switch is required when a process must wait on a lock." ���ڻ��������ź������Ƶ�ѡ��ע����spinlock��ʱ��������context switch�Ĵ�����ռ��CPU��ʱ��֮������Ȩ�⡣Spinlocks are often identified as the locking mechanism of choice on multiprocessor systems when the lock is to be held for a short duration (less than two context switches) һ���ܽ᣺�������������µľ���һֱ��busy waiting���ر��Ƕ��ڵ��ˣ�ռ��CPU���ţ���ʱ��Ƭ������£��������л����η��ֶ�����busy waiting��û�����κ���Ч��������������ȱ�ݣ����ѿ����������������������һ����ˣ��ڶ�������ʱ��Ӧ�ö�Щ�����ź�������������������æ�ȵ���������ò���������sleep�������wakeup����Ȼ�����������������л�������context switch�Ĵ��۽ϸߣ������ʱ��Ҳ�����æ��/�����Ļ��ƣ���ǰ������  
6. P341 ����ԭ���Ե�ִ��һ��ָ�atomically��that is, as one uninterruptible unit.  
7. P348 In fact, on systems that do not provide mutex locks, binary semaphores can be used instead for providing mutual exclusion. ���ǻ������Ͷ�Ԫ�ź����������𣬱����ź������е�wait-list��  
8. ����ĺ��岻һ��������˳�����Ҳ��һ���������ź������������ĺ��壬Ҳ��������Դ�����ĺ��塣��wait()Ϊ����������ʱ��Ӧ����while�����Ƿ�available����ȥS--��������Դ������ʱ��Ӧ����S--����while�����Ƿ�С��0�����С����˵����Դ�����ˣ����ý��̼�����Ӧ�ź�����wait list���ɴ˿�����ͬ��Ҳ��busy waiting��sleep������  
9.  P350 In general, however, the list can use any queuing strategy. Correct usage of semaphores does not depend on a particular queuing strategy for the semaphore lists.  
10. ���ڶ�ˣ���Ȼ��ֹ�жϲ���ȡ�ˣ���Ҫ���������ļ�������wait()/signal()����performed atomically��ԭ���е�������SMP��˵�������ṩCASָ��/spinlock��ʵ��ԭ����ִ��ĳЩ����  
11. P350 "Rather, we have moved busy waiting from the entry section to the critical sections of application programs." û��������ô��ת���ˣ�Ҳ����˵semaphore�����Դ���busy-waiting  
   > <https://stackoverflow.com/questions/58032304/why-busy-waiting-moved-from-the-entry-section-to-the-critical-sections-of-applic> ֻ��һ���ʴ𣬵���û����  
12. P358 "We say that a set of processes is in a deadlocked state when every process in the set is waiting for an event that can be caused only by another process in the set" �����������е�ÿ��process���������ˣ��ͻ�������������������events֮һ�������Chapter 8��  
13. 6.7 Monitor��ʱ������һ��high-level language constructs  
14. P360 ���ڽ�CAS-based approaches����optimistic approach����Mutual exclusion locking����pessimistic strategy��˵��ͦ����˼�ģ��о��е�mnemonic����ζ��  
15. P361  a reader�Cwriter lock����  
16. P361 ����Monitor��condition variables��However, such tools may have significant overhead and, depending on their implementation, may be less likely to scale in highly contended situations. ����"scale in highly contended situations"����keywords��������̣�concurrent programming/�ڸ߾���״̬�µĿ���չ������/scalable and efficient  
