# Chapter 6

1. P334 "In multithreaded applications, several threads��which are quite possibly sharing data��are running in parallel on different processing cores." threads�����ڲ�ͬcore����  
2. process֮�䲻�ܽ������У���������race condition���ɴ�������"critical section problem"������ķ�����-˳��Ҳ����Ҫ��process֮��ͬ��(synchronize)������˳��ִ�У�cooperatively share data����ˣ���Ҫһ��protocol�������е�process/threads/task��ѭ���Э�飬����һ��process��critical sectionʱ������process����ִ��critical section��ͨ�����protocol�ﵽprocess synchronization��Ŀ��  
   > ���**critical-section problem**�ķ�����
      > - **single-core**: prevent interrupts
      >     - **deficiency**: �ڶ������²����ã���Ϊ����delay������system efficiency��ͬʱ����һ���������أ�"Also consider the effect on a system��s clock if the clock is kept updated by interrupts."����Ȼ������ͨ��interrupt����clock��ϵͳ��˵��disable interrupts��������  
      > - **kernel perspective**:
      >     - **nonpreemptive kernels**: free from race conditions  
      >     - **preemptive kernels**: stay unknown right now:(  
      > - **software**: Peterson's Solution
      >     - **deficiency**: up to the compiler and OS because of their efforts for optimization like reordering instructions, which is quite dangerous for multithreaded programs
      > - **hardware**:
      >     - memory barriers: memory_barrier() (can't continue until everything before memory_barrier() is done)
      >     - test_and_set()/compare_and_swap() (in x86 through locking the bus until something is done)
      >     - atomic variables/functions: 
3. ���ڵ�������½�ֹ�жϼ��ɽ��CSP��ԭ�������Ҫ���CSP��һ��˼·������ִ��һ��taskʱ��������context-switch�������ᷢ������ִ�е������Ҳ����ֹ��context-switch���ɣ���Wiki��֪��There are three potential triggers for a context switch: `A. multitasking`/`B. interrupt handling`/`C. user and kernel mode switching`��Ҳ����˵���ֱ��ֹ������Դ���ɽ�ֹ��context-switch���ֱ�����������B����Ȼ�ģ���ֹ�жϼ��ɣ�����C���ڵ���system callʱ�ᷢ��mode switching��ͨ���ù�ָ����У����ù�ָ����������жϣ����Ըо�������Ҳ�ǽ�ֹ�жϼ��ɣ�����A��Wiki��˵"On a pre-emptive multitasking system, the scheduler may also switch out processes that are still runnable. To prevent other processes from being starved of CPU time, pre-emptive schedulers often configure a timer interrupt to fire when a process exceeds its time slice. This interrupt ensures that the scheduler will gain control to perform a context switch."��ע�⵽��timer interrupt��scheduler��Эͬ���������Ըо�������Ҳ�ǽ�ֹ�жϼ��ɣ����ϣ��ڵ�������½�ֹ�жϼ��ɽ��CSP��btw, only interrupts will make a process switch context?  
   > <https://en.wikipedia.org/wiki/Context_switch>
4. 