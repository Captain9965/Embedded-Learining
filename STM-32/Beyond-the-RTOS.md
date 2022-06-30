# Beyond the RTOS:

Courtesy: [Beyond the RTOS](https://www.youtube.com/watch?v=o3eyz1gEqGU&list=PLPW8O6W-1chytjkg63-tM7MI0BvGxxPIP)

The main challenges of embedded system programming:
- Getting off the ground: custom software, custom hardware and a lot of low level code. This is what MCU vendors attempt to solve this by providing boards, cross-development tools bundled with low level software
- Concurrency: Difficult to isolate. Include deadlocks, missed real time deadlines and race conditions.Use software architecture that makes it hard for them to occur
- Spaghetti code: convoluted code that is convoluted. solved by using the right software design. 

## A solution to concurreny using the traditional superloop and the Traditional RTOS:

Superloop refers to main() + ISR's or forground/background. In this architecture, the code waits inline until the specified is met.This has a disadvantage in terms of lack of extensibility and composability (the ability to compose a system from smaller components without disturbing the smaller components)

Hence the purpose of RTOS which extends the basic superloop architecture, allowing one to run multiple superloops(threads/tasks) on one cpu. Remember that each task/thread requires its own stack space and hence takes up more memory than the superloop.

When a function is called eg. vTaskDelay in Free RTOS, it doesn't return to the point of pre-emption. It instead returns to another thread in a process called context switching, which is controlled by the scheduler. The state of the original thread is stored in its private stack. The thread then remains in a blockes state. After the specified time elapses, the context is switched back to the original thread which then unblocks.

The RTOS does this to achieve, first, an illusion that the threads are running simultanoeously. The thread must however block somewhere or else lower priority threads will not progress. 

The second goal is to support a sequential programming inside 

Advantages of RTOS:
- Enables composability of sequentially programmed components
- More efficient CPU use since even if all threads are blocked, the idle thread puts the cpu in a low power state, hence supporting low power designs
- Threads can be decoupled in time domain. Rate monotonic Analysis(describe a set of conditions that a set of threads must meet to prove that it can meet real time deadlines) requires pre-emptive priority based scheduling and is the most commonly provided scheduler in most RTOSes. 

The main challenges around RTOSes revolve around sharing of resources which leads to concurrency issues. 
In fact, RTOS just hijack the interrupt processing hardware available in the cpu. 
The race condiions can be solved if the RTOS provides a mechanism of mutual exclusions eg. mutexes