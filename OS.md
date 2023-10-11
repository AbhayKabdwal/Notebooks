## Definition

- It is a set of programs that controls a computer.
- Organized collection of software extensions of hardware, consisting of routines for operating the computer and for providing an environment for the execution of programs.

Other programs rely on facilities provided by the OS to gain access to computer system resources such as I/O.

Programs usually invoke services of OS by means of OS calls.

User interacts with OS by OS commands.

## Roles of OS

- OS makes the computing power of hardware conveniently available to users and manages the hardware carefully to achieve good performance.

### External View

- Interface between user and hardware.

### Internal View

- Takes care of I/O in the computer system, manages users, processes, memory, networks, etc.

## Design Goals

- User Interface: Should be easy to learn or for the convenience of an experienced user.
- Efficient system resource management: Complete resource management requires more overhead.
- Security: A more secure system may be less efficient.
- Portability
- Backward compatibility or Emulation: It is important that software that ran under previous OS versions or different OS be supported.
- Flexibility: It involves specifying the attached hardware so that only necessary drivers will be loaded. Some OS can load or unload drivers automatically at runtime.

## Evolution of OS

It evolved through the path of serial processing, batch processing, multiprogramming, etc.

### Serial Processing

- Directly hardware.
- Low productivity.
- Manually translating a sequence of instructions into binary code using control switches.
- Execution by loading the address of the first instruction into the Program Counter.
- Results are obtained by checking the relevant registers and memory.
- The next stage was with the advent of I/O devices.
- Programs can be coded in high-level programming languages and translated into executable form by a compiler or interpreter.
- The loader loads the executable program into memory, and execution commences.
- The whole process is cumbersome due to serial execution and involves manual operation.

### Batch Processing

- Jobs are converted to batches based on similar needs by an operator.
- Jobs of one batch are executed together.

#### Advantages and Disadvantages of Batch Processing

- Much manual work in serial processing is shifted from the operator to the computer.
- Increased performance as it will be possible to start a job after the previous job is finished.

    **Cons:**
    - Turnaround time can be larger from the user's point of view.
    - More difficult to debug.
    - One job can affect other pending jobs.

### Spooling

- Sophisticated form of I/O buffering.
- Uses disk to temporarily store input and output of jobs.
- Card to disk for the subsequent program and disk to printer for previous programs are performed by spooling monitor concurrently with the execution of the current program.

### Multiprogramming

- Most programs during their execution oscillate between computation-intensive and I/O-intensive phases.
- As a result, significant performance gain is achieved by interleaved execution of programs, referred to as multiprogramming.
- With a single processor, parallel execution is not possible, and at most one program can be in control of the CPU. The objective is to have some process running at all times.
- To increase resource utilization, actual multiprogramming systems usually allow more than two programs to compete for system resources.
- The number of programs actively competing for the resources of the system is called the degree of multiprogramming.
- In principle, a higher degree of multiprogramming should result in higher resource utilization.

### Process

- A process is an operating system abstraction for running a program.
- A process is a program in execution.
- A process is associated with an address space.
- A thread is a running program without its own address space.

(Kernel space is where the kernel (the core of the OS) executes and provides its services.)

## Program vs. Process

- A program by itself is not a process.
- A program is a passive entity such as a file containing a list of instructions (executable file).
- A process is an active entity with a program counter specifying the next instruction to execute and a set of associated resources.

- A program becomes a process when its executable file is loaded into memory.

## States of Process

- New: When a program present in secondary memory is initiated for execution.
- Ready: A process moves from the new state to the ready state after it is loaded into the main memory and is ready for execution. In multiprogramming, many processes may be present in the ready state.
- Running: A process moves from the ready to the run state after it is assigned the CPU for execution.
- Waiting: The process is waiting for some event to occur (such as I/O completion or reception of a signal).
- Terminated: The process has finished execution. The context (PCB) of the process is deleted by the OS.
  [ PCB = Process Control Block ]
- Suspend Ready: If a process with a higher priority has to be executed but the main memory is full, then it goes from ready to suspend ready.
- Suspend Wait: If a process with a higher priority has to be executed but the main memory is full, the waiting process goes from waiting to suspend wait.

## Process Control Block

- The OS groups all information that it needs about a particular process into a data structure called Process Descriptor or Process Control Block.
  - These pieces of information are called the context of the process.
  - Each process is defined by its PCB.
  - Whenever a process is created, the OS creates its corresponding PCB.
  - A process becomes known to the OS and thus eligible to compete for system resources only when it has an active PCB.

### Information Stored in PCB

- Process ID: It is a unique ID that identifies each process of the system uniquely and is assigned to each process during its creation.
- Program counter: Address of the next instruction to be executed.
- Process State: Current state of the process.
- Priority: It specifies how urgent it is to execute the process. (Higher priority -> first allocated to CPU)
- General-purpose registers: Used to hold data of process generated during its execution.
- File management information: Each process requires some files which must be present in the main memory during its execution. PCB maintains a list of all these files with access rights.
- I/O status: Allocated device and pending requests.

## Process Scheduling

The objective of multiprogramming is to have some process running at all times to maximize CPU utilization. The objective of time-sharing is to switch the CPU among processes so frequently that users can interact with each program when it is running. For a single-processor system, there will never be more than one running process. If there are more processes, the rest will have to wait until the CPU is free and can be rescheduled.

### Scheduling

Scheduling refers to the set of policies and mechanisms built into the operating system that govern the order in which work to be done by a computer system will be completed.

### Scheduler

It is an OS module that selects the next job to be admitted to the system and the next process to run.

Objective: To optimize system performance in accordance with the criteria deemed to be most important by the OS designer.

Types:
- Long Term
- Medium Term
- Short Term

### Scheduling Queues

- Job Queue: It consists of all processes in the system.
- Ready Queue: The processes that are residing in the main memory and are ready and waiting to execute are kept on a list called the ready queue.
  - Linked list.
  - Pointers to the first and final PCBs in the list.
  - Each PCB contains a pointer field that points to the next PCB in the ready queue.
- Device Queue: A list of processes waiting for a particular I/O device is called a device queue.
  (Each device has its device queue.)

### Long Term Scheduler

- Job scheduler.
- In a batch system, more processes are submitted than can be executed immediately. These processes are spooled to a mass storage device, where they are kept for later execution.
- Batch jobs contain all necessary data and commands for their execution and also contain a programmer's or system-assigned estimate of resources required.
- The objective of the long-term scheduler is to select a well-balanced mix of I/O-bound and CPU-bound processes from the secondary memory.

[SPOOL = Simultaneous Peripheral Operation OnLine]

- The long-term scheduler executes much less frequently and controls the degree of multiprogramming.

### Medium Term Scheduler

- A running process may become suspended by making an I/O request or by issuing a system call or by creating a new subprocess and waiting for its completion. The suspended process cannot make progress toward completion until the suspension condition is removed.
- The key idea behind a MTS is that sometimes it can be advantageous to remove them from memory to make room for other processes. Saving the image of a suspended process in secondary memory is called swapping. The process is said to be swapped out or rolled out.
- It is in charge of handling the swapped-out process. It has to do little while a process remains suspended. However, once the suspension condition is removed, the medium-term scheduler attempts to allocate the required amount of main memory and make it ready.

### Short Term Scheduler

- Also known as CPU scheduler.
- It decides which process to execute next from the ready queue.
- After the short-term scheduler decides the process, the dispatcher assigns the decided process to the CPU for execution.
- The short-term scheduler must select a process from the ready queue frequently. A process may execute only for a few milliseconds before waiting for an I/O request.

### Scheduling Criteria

- CPU Utilization
  - Keep the CPU and the other resources as busy as possible (must increase).
- Throughput
  - The number of processes that complete executing per unit of time (must increase).
- Turnaround Time
  - Amount of time to execute a particular process from its submission time to completion time (must decrease).
- Waiting Time
  - Amount of time a process has been waiting in the ready queue (must decrease).
- Response Time (in a time-sharing environment)
  - Amount of time it takes from when a request was submitted until the first response is produced (not output).

#### To Optimize Overall System Performance

- Maximize CPU utilization.
- Maximize Throughput.

#### To Optimize Individual Process Performance

- Minimize Turnaround Time.
- Minimize Waiting Time.
- Minimize Response Time.

## First Come First Serve (FCFS)

- In case of a tie, the process with a lower PID is executed first.
- Non-preemptive in nature.
  - Once the CPU is allocated to a process, it keeps the CPU until it releases it.
- Gantt chart is used to visualize schedules.

**Performance Metric:**
Evaluated on the parameters of Average Waiting Time in the queue and the Average Turnaround Time.

### Implementation

- Using FIFO is easy.
- When a process enters the ready queue, it is added to the tail, and its PCB is linked to the rear end of the queue.
- When the CPU becomes free, it is allocated to the process at the front, and the running process is removed from the ready queue.

**Turnaround Time = Exit time - Arrival time**
**Waiting Time = Turnaround Time - Burst Time**

### Performance in Terms of CPU Utilization

- When a CPU-bound process gets the CPU and holds it, the I/O-bound processes will finish their I/O and wait for the CPU in the ready queue, making I/O devices idle.
- When the CPU-bound process waits for I/O, the I/O-bound processes with the shortest CPU burst execute quickly and move to I/O device queues, while the CPU sits idle.
- When one big process holds the CPU, the other processes have to wait in the ready queue until the CPU becomes free, resulting in the convoy effect.

- This results in lower CPU and I/O device utilization.

#### Advantages

- Simple and easy to understand.
- Easy to implement using a queue data structure.
- Does not lead to starvation.

#### Disadvantages

- It does not consider the priority or burst time of the processes.
- It suffers from the convoy effect (short processes behind a long process).
- FCFS scheduling is not suitable for time-sharing systems where each process needs to get a share of the CPU at regular intervals.

## Shortest Job First Scheduling

Associates with each process the length of its next CPU burst time. When the CPU becomes available, it will be assigned the process that has the shortest next CPU burst time. When more than one process has the same length, FCFS is used to break the tie. SJF scheduling algorithm can be either preemptive or non-preemptive, and the choice arises when a new process arrives at the ready queue while a current process is executing.

- Non-preemptive SJF
  Once CPU is given to the process, it cannot be preempted until it completes its current CPU burst.

- Preemptive SJF
  If a new CPU process arrives with a CPU burst less than the remaining CPU burst time of the current running process, then preempt the current running process and allocate the new process with the shortest CPU burst to the CPU.
  [ SRTF = Shortest Remaining Time First ]

#### Advantages and Disadvantages of SRTF

- SRTF guarantees the minimum average time for a given set of processes and is optimal. The minimum average wait time is achieved by decreasing the waiting time for short processes and increasing the waiting time for long processes.
- It provides a standard for other algorithms as no other algorithm performs better than it.

**Cons:**
- It cannot be implemented practically since the burst time of the processes cannot be known in advance. We can estimate the burst time.
- Halting problem: cannot (in general) determine if a program will run forever on some input or complete in finite time.
- It leads to starvation of longer processes.
- Priorities cannot be set.
- Processes with larger burst time have poor response time.
