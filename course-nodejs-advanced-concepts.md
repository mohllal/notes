# Node JS: Advanced Concepts

By: Stephen Grider. [Purchase the course](https://www.udemy.com/course/advanced-node-for-developers)!

Date Started: Monday, November 8, 2019

Date Finished: TBD

## Section 1: The Internals of Node.js

- A ***process*** is an executing instance of an application. A ***thread*** is a path of execution within a process. Also, a process can contain multiple threads. Since a process can consist of multiple threads, a thread could be considered a *lightweight* process. Thus, the essential difference between a thread and a process is the work that each one is used to accomplish. Threads are used for small tasks, whereas processes are used for more *heavyweight* tasks – basically the execution of applications.

- Process ***control block*** controls the operation of any process. Process control block contains the information about processes for example: *Process priority*, *process id*, *process state*, *CPU*, *register* etc. A process can creates other processes which are known as ***Child Processes***. Process takes more time to terminate and it is isolated means it does not share memory with any other process.

- Threads within the same process ***share the same address space***, whereas different processes do not. But threads within the same process share that address space which allows threads to read from and write to the same data structures and variables, and also facilitates communication between threads. Communication between processes – also known as ***IPC***, or *inter-process communication* – is quite difficult and resource-intensive.

- A thread have 3 states: *running*, *ready*, and *blocked*.

- Comparison between Processes and Threads:
  
  | Processes                                         | Threads                                           |
  | ------------------------------------------------ | ------------------------------------------------- |
  | Process means any program is in execution.       | Thread means a path of execution within a process |
  | Process takes more time to create and terminate. | Thread takes less time to create and terminate.   |
  | Processes are isolated.                             | Threads share memory.                             |
  | Process takes more time for context switching.  | Thread takes less time for context switching.    |
  | Process consumes more resources.                  | Thread consumes less resources.                   |
- ***Multithreading*** requires careful programming since threads share data structures that should only be modified by one thread at a time; This is called ***Synchronization***.
- ***Multiprocessing*** is a system that has more than one or two processors that are executed simultaneously. In Multiprocessing, *CPUs are added for increasing computing speed* of the system.
- ***Multithreading*** is a system that has more than one or two threads, created of a single process, that are executed simultaneously. In Multithreading, *Threads are created for increasing computing speed* of the system.
- Multiprocessing can be classified as ***symmetric*** multiprocessing and ***asymmetric*** multiprocessing. In symmetric multiprocessing, all processors *are free to run any process* in a system. In Asymmetric multiprocessing, there is a *master-slave relationship among the processors*. The master processor is responsible for allotting the process to slave processors.
- Creating a thread is ***economical*** as it shares the code and data of the process to which they belong. So the system does not have to allocate resources separately for each thread.
- ***Blocking*** is when the execution of additional JavaScript in the Node.js process must wait until a non-JavaScript operation completes. ***Synchronous*** methods in the Node.js standard library that use `libuv` are the most commonly used blocking operations. Native modules may also have blocking methods.
- *Blocking* methods execute *synchronously* and *non-blocking* methods execute *asynchronously*.
- JavaScript execution in Node.js is ***single threaded***, so concurrency refers to the event loop's capacity to *execute JavaScript callback functions* after completing other work. Any code that is expected to run in a concurrent manner must allow the event loop to continue running as non-JavaScript operations, like I/O, are occurring.
- The ***event loop*** is what allows Node.js to perform *non-blocking I/O operations* — despite the fact that JavaScript is single-threaded — by *offloading operations* to the system kernel whenever possible. When one of these operations completes, the kernel tells Node.js so that the appropriate callback may be added to the ***poll queue*** to eventually be executed.
- The event loop's *order of operations* (phases):
  - timers: This phase executes `setTimeout()` and `setInterval()` callbacks.
  - pending callbacks: This phase executes **I/O** callbacks deferred to the next loop iteration.
  - idle, prepare: This phase only used internally.
  - poll: This phase retrieves new I/O events; ***execute I/O related callbacks*** (almost all with the exception of close callbacks, the ones scheduled by timers, and `setImmediate()`); Node.js will block here when appropriate.
  - check: This phase executes `setImmediate()` callbacks.
  - close: This phase executes `close` callbacks, e.g. `socket.on('close', ...)`.
- Each phase has a ***FIFO queue*** of callbacks to execute. While each phase is special in its own way, generally, when the event loop enters a given phase, it will perform any operations specific to that phase, then *execute callbacks* in that phase's queue until the ***queue has been exhausted*** or the ***maximum number of callbacks has executed*** and then the event loop will move to the next phase, and so on.
- Between each run of the event loop, Node.js checks if it is waiting for any asynchronous I/O or timers and shuts down cleanly if there are not any.
- A timer specifies the threshold after which a provided callback may be executed rather than the exact time a person wants it to be executed. Timers callbacks will run as early as they can be scheduled after the specified amount of time has passed;
- A timer specifies the *threshold* after which a provided callback may be executed rather than the exact time a person wants it to be executed. Timers callbacks will run ***as early as*** they can be scheduled after the specified amount of time has passed.
- To prevent the poll phase from *starving* the event loop, `libuv` (the C library that implements the Node.js event loop and all of the asynchronous behaviors of the platform) also has a ***hard maximum*** (system dependent) before it stops polling for more events.
- The pending callbacks phase executes callbacks for some system operations such as types of TCP errors.
- The poll phase has *two* main functions:
  1. Calculating how long it should block and poll for I/O
  2. Processing events in the poll queue.
- When the event loop enters the poll phase and there are no timers scheduled, ***one of two*** things will happen:
  - If the poll queue is ***not empty***, the event loop will iterate through its queue of callbacks executing them synchronously *until either the queue has been exhausted, or the system-dependent hard limit is reached*.
  - If the poll queue is ***empty***, *one of two* more things will happen:
    - If scripts have been scheduled by `setImmediate()`, the event loop will end the poll phase and continue to the ***check*** phase to execute those scheduled scripts.
    - If scripts have not been scheduled by `setImmediate()`, the event loop will wait for callbacks to be added to the queue, wait for an incoming connection, request, etc.,then execute them immediately.
- The ***check*** phase allows a person to execute callbacks ***immediately*** after the ***poll*** phase has completed. `setImmediate()` is actually a *special* timer that runs in a separate phase of the event loop. It uses a `libuv` API that schedules callbacks to execute after the poll phase has completed.
- If a socket or handle is ***closed abruptly*** (e.g. `socket.destroy()`), the `'close'` event will be emitted in the ***close callbacks*** phase.
- `setImmediate()` and `setTimeout()` are similar, but behave in different ways depending on the context in which they are called:
  - If both are called from within the ***main module***, not within an I/O cycle, then timing will be bound by the performance of the process (which can be impacted by other applications running on the machine).
  - If both are called from within an ***I/O cycle***, the `setImmediate()` callback is always executed *first* so it will be executed before any timers if scheduled within an I/O cycle, independently of how many timers are present.
- Instead, the `nextTickQueue`, which contains the callbacks registered with the `process.nextTick()`, function will be processed after the current operation is completed, *regardless of the current phase* of the event loop. By using `process.nextTick()` we guarantee that the event loop always runs its callback ***after*** the rest of the user's code and before the event loop is allowed to proceed to the next phase.
- So as a comparsion between `process.nextTick()` and `setImmediate()`:
  - `process.nextTick()` fires immediately on the same phase.
  - `setImmediate()` fires on the following iteration or *'tick'* of the event loop.
- There are *two* main reasons to use `process.nextTick()`:
  - Allow users to handle errors, cleanup any then unneeded resources, or perhaps try the request again before the event loop continues.
  - At times it's necessary to allow a callback to run after the call stack has unwound but before the event loop continues.
- The ***Thread Pool*** is a group of threads, *four* by default, that can be used for running computationally intensive tasks. The `UV_THREADPOOL_SIZE` environment variable can be used to edit the number of threads in the `libuv` Thread Pool.
- If the application is idle, which means that there are ***no pendings tasks*** (Timers, callbacks, etc), it would not make sense to run through the phases with full speed, so the event loop will adapt to that and ***block for a while in the polling phase*** to wait for new external events coming in.
