# Chapter 3

1. ����thread && process:
   > - A thread and a process are both units of execution that are used to manage the execution of a program on a computer. 
   > - Process can be thought of as a container that holds all the resources that are required for the program to run. 
   > - A thread, on the other hand, is a lightweight, independent unit of execution within a process. 
   > - A process is an instance of a program, and a thread is a separate flow of execution within a process. A process can contain multiple threads, each of which can execute independently of the others.  
2. �����ָ���׼���Լ�ִ�г����һ��ָ��غ���ˮ�ߣ���ôǰ����ϵ�ṹ��ѧϰ���к���Ҫ��һ����(ָ�����)���ڽ����������ˮ���ϲ��ø��ּ�������Ч��ִ����Щָ�������OS�����������ᵽ"The objective of multiprogramming is to have some process running at all times so as to maximize CPU utilization."�����ѿ���multiprogramming��Ŀ���ǰ���ˮ��ǰ���ָ�������������ó���ʼ�ձ���������״̬����ά�ֶ����ṩ������ָ��**�ٶȶԵ�**��Ч��������������¿���maximize CPU utilization��  
3. ����exec() return���������� GUESSED: exec() system call����ɹ�ִ�еĻ��϶�����return anything����Ϊû�����壬ԭ����program�Ѿ���replace��return�Ķ�����û�����ˣ�����Ӧ����û��"return xxx;"������䣬����"if(xxx) {ERRNO = xxx; return -1;}"���� 
   > - https://en.wikipedia.org/wiki/Exec_(system_call)#Return_value
   > - A successful exec replaces the current process image, so it cannot return anything to the program that made the call. Processes do have an exit status, but that value is collected by the parent process.
   > - ����ChatGPT�����ֽ׶λ������ðɣ��������ִ𰸺�ֱ�ӵ�������Ӧ��û���⣬����һЩ�����ֿڵľͿ��ܻ��������Ĵ𰸣�����exec()��pid���䣬��˵��䣻�ֽ׶λ���һ�����Žӿڸ�������ѵ��ģ�͵Ľ׶Σ������û���reactions��reinforceģ�ͣ����Ľ�����Ĵ𰸣�����Ҳ�û�ж�"exec()�����һ��new pid"���ִ������κ�reaction��������һ�����ʵ���ͬ����ʱ�������������𰸣����������˻�ָ�����ģ���ChatGPT�ܾ�׼�ش���󲿷�����ʱ��Ӧ�þ�������ʼ�շѵĽ׶���:)
4. The wait() system call then retrieves the exit status of the child process from the child's PCB and stores it in the memory location passed as an argument to the wait() system call. In this way, the parent process can access the exit status of the child process and determine if the child process terminated successfully or if an error occurred. // the general idea is that the exit status of the child process is stored in a separate memory location before the PCB is deallocated. exit()��wait()�Ľ����������ˣ���������ӽ��̵�returned value����exit status����������ֵ������һ����ĳ����ƽ�����ӽ��̣������ķ���ֵ���ܾ���ĳһ������ƽ����û��exit status�ĺ����ˣ���ʱ��Ҫ����exit()�𣿣������ܸ���һ����IPC�йأ��������Ƕ�֪��returned value��Ҫ��exit()�ģ�Ȼ��Ž��ӽ��̵�PCB������̵�֪�ӽ���Ҫ�����������ӽ��̵�PCB retrieve exit status��һ��retrieve��ϣ�exit()����ִ�У������ӽ���(������PCB)����ô��˵the process table contains the process��s exit status :confused:  
   > - When a child process calls the exit() function, it passes an exit status as a parameter to the function. This exit status is then stored by the operating system in a specific memory location, typically known as the child's process control block (PCB). The PCB is a data structure that the operating system uses to store information about a process, such as its state, priority, and resources. 
   > - When the parent process calls the wait() system call, it suspends its execution and waits for one of its child processes to terminate. Once a child process terminates and calls the exit() function, the operating system notifies the parent process that one of its child processes has terminated.
   > - The wait() system call then retrieves the exit status of the child process from the child's PCB and stores it in the memory location passed as an argument to the wait() system call. In this way, the parent process can access the exit status of the child process and determine if the child process terminated successfully or if an error occurred.
   > - In summary, when a child process calls the exit() function, it passes an exit status as a parameter, the operating system stores the exit status in a specific memory location, typically known as the child's process control block (PCB), the parent process calls the wait() system call, it suspends its execution and waits for one of its child processes to terminate, once a child process terminates, the wait() system call retrieves the exit status of the child process from the child's PCB and stores it in the memory location passed as an argument to the wait() system call. This way, the parent process can access the exit status of the child process and determine if the child process terminated successfully or if an error occurred.
   > ---
   > - You are correct that the process control block (PCB) of a child process is typically deallocated by the operating system once the child process terminates. However, the exit status of the child process is typically stored in a separate memory location by the operating system before the PCB is deallocated.
   > - When a child process calls the exit() function, it passes an exit status as a parameter. The operating system stores the exit status in a specific memory location and marks the child process as terminated. The parent process can then call the wait() system call, which retrieves the exit status of the child process from the memory location where the operating system stored it and stores it in the memory location passed as an argument to the wait() system call. Once the parent process has retrieved the exit status of the child process, the operating system can safely deallocate the child's PCB.
   > - It's worth mentioning that different operating systems may have different implementation of exit() and wait() system call and the process control block (PCB) management, but the general idea is that the exit status of the child process is stored in a separate memory location before the PCB is deallocated.
   > - In summary, once the child process terminates and calls the exit() function, the operating system stores the exit status in a specific memory location, and marks the child process as terminated. The parent process can then call the wait() system call, which retrieves the exit status of the child process from the memory location where the operating system stored it and stores it in the memory location passed as an argument to the wait() system call, once the parent process has retrieved the exit status of the child process, the operating system can safely deallocate the child's PCB.  
5. under normal termination, exit() will be called indirectly, as the `C run-time library` (which is added to UNIX executable files) will include a call to exit() by default�����û��return statement��Ĭ�Ϸ���0��Ĭ�ϳɹ�ִ�У���Ȼ��Ҫ������ʵ�֣���ЩҲ��Ĭ�Ϸ���1����-1  
   > A common way for the ***main function*** to return is by using the "return xxx;" statement, where xxx is an integer value that represents the exit status of the program. When the ***main function*** returns, the C run-time library's exit() function will be called, and the exit status of the program will be passed as an argument to the exit() function.  
6. �ڶ������⣺in "the return code for the fork() is zero for the new (child) process, whereas the (nonzero) process identifier of the child is returned to the parent.", how does fork() implement it, creating a new memory space for child process, then returning zero to child process and nonzero(pid of child process) to parent process? ChatGPT�Ļش𣺴��ɣ�����
   > The operating system then sets the return value of fork() to zero in the child process's stack and the PID of the child process in the parent process's stack. When the child process starts executing, it retrieves the value of the fork() return from its stack, which is zero, and assigns it to a variable. Similarly, when the parent process resumes execution after the fork() call, it retrieves the value of the fork() return from its stack, which is the PID of the child process, and assigns it to a variable. This allows the parent process to communicate and synchronize with the child process if necessary.  
   > They(Parent/Child process) can then inspect the call's return value to determine their status, child or parent, and act accordingly.
7. P162 "a web application in any tab crashes, the entire process��including all other tabs displaying additional websites crashes as well." �������ڵ���ƣ�Ϊʲô  
8. P163 "Message passing is useful for exchanging smaller amounts of data, because no conflicts need be avoided. Message passing is also easier to implement in a distributed system than shared memory.(Although there are systems that provide distributed shared memory, we do not consider them in this text.)" Ϊʲô  
9. P168 "Nonblocking receive: The receiver retrieves either a valid message or a null." means:  
   > When defining a non-blocking receive operation as "The receiver retrieves either a valid message or a null", it means that the receiver will attempt to retrieve a message, but if there is no message available, it will not block and wait for a message to arrive, instead it will retrieve a "null" or "empty" value. A null value is a special value that represents the absence of a value or an "empty" value. In this context, it means that there is no message available to be received. This allows the receiver to check if a message has been received, and if not, to take appropriate action or continue executing other tasks. For example, the receiver can check if the received message is null, and if so, it can continue with other tasks, otherwise, it can process the received message. This is different from a blocking receive operation, where the receiver will wait until a message is available to be received and will not proceed with other tasks. It's worth noting that this type of non-blocking receive operation is also called "polling" where a process polls a communication channel repeatedly to check if a message is available, and it can be useful in situations where the process can't afford to be blocked and wait for a message to arrive.  
10. shared-memory��message-passing�ﶼ��buffer�G��������һ������֮��ģ��ܸо�message-passing model����һ�ָ�shared-memoryһ�����������֣���Ȼ����A�����ﻹ��ҪB�ĸо�  
11. P170 "the mmap() function establishes a memory-mapped file containing the shared-memory object. It also returns a pointer to the memory-mapped file that is used for accessing the shared-memory object." ����POSIX shared memory��ʵ�֣�������shm_open()��һ��shared memory object�����Ӧ������disk�Ȼ����ftruncate()ָ����С�������mmap()��objectӳ�䵽calling process��virtual address space��  
12. P173 "The Mach kernel supports the creation and destruction of multiple tasks, which are similar to processes but have multiple threads of control and fewer associated resources.", why "fewer associated resources"  
   > In traditional operating systems, a process typically has a single thread of control and its own set of associated resources. These resources are typically allocated to the process when it is created, and are not shared with other processes. So, compared to Mach tasks, traditional processes in other operating systems don't have threads of control by default, but they can have multiple threads of control. 
13. P173 "Each port may have multiple senders, but only one receiver."(The only receiver is the port's owner who created it no matter whhich sender is) "A port��s owner may also manipulate the capabilities for a port. This is most commonly done in establishing a reply port." "It also identifies the rights for the port. Each port right represents a name for that port, and a port can only be accessed via a right."?  
   > Each port right is a token that represents the name of the port and grants the holder of the right access to the port, allowing them to send or receive messages to and from it.  
14. P173 "Mach uses ports to represent resources such as tasks, threads, memory, and processors, while message passing provides an object-oriented approach for interacting with these system resources and services."  
15. P174 mach_port_allocate()���һ�������о�������  
16. P174 "Each task also has access to a bootstrap port, which allows a task to register a port it has created with a system-wide bootstrap server."?  
17. P176 "Therefore, the message itself is never actually copied, as both the sender and receiver access the same memory. This message-management technique provides a large performance boost but works only for intrasystem messages." ��POSIX shared-memory�Ǹ��������õ��ļ���һ��(Chapter 10 virtual-memory-management techniques)�����ǰ�һ��spaceӳ�䵽virtual memory space�ǰ��Ҫӳ���������disk�����ô�ͳ��I/O�϶�Ҫ����ʱ�䣬��������Ҫ��Ϊ�˼���message copy��ʱ�䣻" Message passing may occur between any two ports on the same host or on separate hosts on a distributed system." ͬʱ�ü���Ҳ��һ��������ֻ��intrasystem����Ϊֻ���ڵ���һ��ϵͳ�ڲ��źù���address space��ǣ������ͬsystemӦ�û��������������Կ�ϵͳ/�ֲ�ʽϵͳ�ڲ���message passing���ƻ�Ҫcopy��**���⻰��������취�˷���һ���𣬻��о���message pssing��shared-memory����IPC model�о��кö����Ƶĵط�����ǰ���10.������˵�����õ��Ķ���ͨ�õļ��ɣ���ô���ߵĲ�֮ͬ��/��ȱ����ʲô�أ�����˼��һ��**  
18. ����modularity��  
   > - The phrase "the limited modularity of the resulting process definitions" refers to the degree to which the process definitions can be broken down into smaller, independent, and reusable components.
   > - Modularity in software development refers to the ability to divide a large, complex system into smaller, more manageable, and independent parts, that can be developed and tested separately. When it comes to process definitions, modularity implies that the process definitions can be defined in a way that they can be easily reused, extended, and composed with other process definitions to create more complex systems.
   > - When the modularity of process definitions is limited, it means that the process definitions are tightly coupled and dependent on each other, making it difficult to reuse or extend them without affecting other parts of the system. This can make the system harder to maintain, understand, and evolve over time.
   > - Therefore, limited modularity of the resulting process definitions implies that the process definitions are not easily reusable, extensible, and composable and may hinder the maintainability and scalability of the system.  
19. P177 "Additionally, communication channels support a callback mechanism that allows the client and server to accept requests when they would normally be expecting a reply." callback mechanism?  
20. Unix���pipe��һ��������ļ���������������Ԫ�ص��������飬Ԫ����file descriptor���Ҹо�file descriptor��port/mailbox����ͬ���Ƕ�������һЩ�������е����潲������������󷽷�ѧ�ĸо�����ͬ����file descriptorֻ�д����ĺ��壬����port/mailbox�ڲ������ж�����message queue֮��ġ�pipe��unidirectional���Ҿ��ò��ܿ������̺��ӽ���֮���˫������Ҫ��pipe�������ǴӶ��˵�д�ˣ�����ν˭��˭д�����Ի���unidirectional��pipe��������ν������ļ��Ķ�д�������ļ����������ˣ�Ϊʲô������һ���ļ��أ��ǲ�����shared-memory�ˣ�������һ��ǰ��Mach���"reply port"��Ҳ��һ��port����receive����һ��port����response���кô�  
21. Linuxϵͳֱ�Ӱѹܵ�ʵ�ֳ���һ���ļ�ϵͳ������VFS��Ӧ�ó����ṩ�����ӿڡ���Ȼʵ����̬�����ļ������ǹܵ���������ռ�ô��̻��������ⲿ�洢�Ŀռ䡣��Linux��ʵ���ϣ���ռ�õ����ڴ�ռ䡣���ԣ�Linux�ϵĹܵ�����һ��������ʽΪ�ļ����ڴ滺������  
22. P186 "Socket client = sock.accept();" GUESSED: socket.accept()�����������һ��socket����Ȼ��Ӧ���Ƿ���request��clientһ�˵�socket��ע�⣬client�˵�socket��ʹ����ˣ�ֻ����server�˲�֪����ֱ��������һ������server��֪�����ҹ�����һ��client�˵�socket MODIFIED: "Socket sock = new Socket("127.0.0.1",6013);" client�˿���ͨ��������ʽ�����Լ���socket����������������IP��port number��������server��"When a client connects to the server, the accept() method returns a new socket that is bound to the client's IP address and port. This socket can be used by the server to communicate with the client and send/receive data."  
   > A network socket is a software structure within a network node of a computer network that serves as an endpoint for sending and receiving data across the network. Sockets are created only during the lifetime of a process of an application running in the node.
23. ����port��֪����������д��ͦ�ã�����������������port����һ����ѧ���ʱ������port����Ӳ���˿ڣ����ǼĴ�����CPU����ֱ��Ѱַ��CPU�����蹵ͨ��ý�飻�ڶ���ʱѧIPCʱ������mailbox/port��"The queue associated with each port is finite in size and is initially empty. As messages are sent to the port, the messages are copied into the queue."��Ҳ���Լ���port number��Ӧ�þ���buffer������������ѧRPCʱ��"A port in this context is simply a number included at the start of a message packet. Whereas a system normally has one network address, it can have many ports within that address to differentiate the many network services it supports. If a remote process needs a service, it addresses a message to the proper port."��ǣ����network�ϵĲ�ͬmachineʱ����ǣ�����˸���application protocol�������port�������˼�ǵ��������֣�����client&&server�˵Ķ˿ںţ���Ȼ�������֮��Ҳ��buffer������һЩmessage  
24. P188 ����RPCs "RPCs can fail, or be duplicated and executed more than once, as a result of common network errors."��  
   > RPCs can be duplicated and executed more than once if the network experiences communication issues, such as network congestion, network partition, or network delay, that result in the failure of the original RPC to reach the server, causing the client to retry the same request. Additionally, if the server crashes after executing the original request but before sending the response, the client might not know whether the request was processed or not, leading it to resend the request, causing the RPC to be executed twice. To mitigate this issue, most RPC systems implement techniques such as idempotency, sequence numbers, and retransmission to detect and avoid duplicated RPCs.
25. ����timestamp����η�ֹrepeated messages�����е��ɻ�  
26. P171/172 POSIX shared-memory ���δ���(producer process && consumer process)�о��д���  
   > - [APPRORIATE use for POSIX shared-memory API][1]��Ӧ�ð�`mmap(0, SIZE, PROT READ | PROT WRITE, MAP SHARED, fd, 0);`�ֱ�ĳ�`PROT_READ`��`PROT_WRITE`
   > - <https://support.xilinx.com/s/question/0D52E00006hpU0FSAU/cant-compile-shmopen?language=en_US>���������޷�����ͨ�����򣬱���"undefined reference to `shm_open`"��������gcc�����`-lrt`����Ϊ����`shm_open()`/`shm_unlink()`/`sem_timedwait()`/`mq_timedsend()`/`sched_rr_get_interval()`��Щ"are part of the real-time extension of the POSIX standard, which provides support for real-time programming and enables processes to meet deadlines in response to external events."���ڳ����ɿ�ִ���ļ��Ĺ����п�����Ҫrun-time library�İ�������Ϊheader-files��ֻ��declaration��������implementation��run-time library���ô������ʹ��gcc����ʱ������Ҫ��ʽ�ؼ���`-lrt`(������ʾ)���������������Ĭ�Ͼͻ���������ʱ�⣬Ϊʲô��"It depends on what functions you are using from the rt library. If you are using functions from the ***real-time extension of the POSIX standard***, then you may need to link with the rt library by adding `-lrt` when you compile your program. However, if you are not using any functions from the real-time extension, then you do not need to link with the rt library."
   > - <https://stackoverflow.com/questions/9923495/undefined-reference-shm-open-already-add-lrt-flag-here>������ʹ��gcc�������ɿ�ִ���ļ�ʱ���������ӿ������˳����Ҫ��(��֪���°汾����û��Ҫ��)
27. ʹ�� System V message queue ʵ�ֽ��̼�ͨ��  
   > <https://feichashao.com/ipc-mq/>  
28. ����process states && queue:  
   > <https://www.tutorialspoint.com/what-are-the-different-types-of-process-states-and-queues>  
   > <https://courses.cs.washington.edu/courses/cse410/99au/lectures/Lecture-11-01/tsld009.htm>
29. ���������ڴ棺With enough privileges, processes can request the kernel to map part of another process's memory space to its own, as is the case for debuggers. ��debugger��ʲô��ϵ  
   > <https://en.wikipedia.org/wiki/User_space_and_kernel_space> 


[1]: https://www.geeksforgeeks.org/posix-shared-memory-api/