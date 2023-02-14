# Virtualizácia CPU
See also: [procesy](sprava-procesov.md)
## Key CPU virtualization terms (mechanisms)
- The CPU should support at least two modes of execution: a restricted **user mode** and a privileged (non-restricted) **kernel mode**.
- Typical user applications run in user mode, and use a **system call**
to **trap** into the kernel to request operating system services.
- The trap instruction saves register state carefully, changes the hardware status to kernel mode, and jumps into the OS to a pre-specified
destination: the **trap table**.
- When the OS finishes servicing a system call, it returns to the user
program via another special **return-from-trap** instruction, which reduces privilege and returns control to the instruction after the trap
that jumped into the OS.
- The trap tables must be set up by the OS at boot time, and make
sure that they cannot be readily modified by user programs. All
of this is part of the **limited direct execution** protocol which runs
programs efficiently but without loss of OS control.
- Once a program is running, the OS must use hardware mechanisms
to ensure the user program does not run forever, namely the timer
interrupt. This approach is a **non-cooperative** approach to CPU
scheduling.
- Sometimes the OS, during a timer interrupt or system call, might
wish to switch from running the current process to a different one,
a low-level technique known as a **context switch**.

## Scheduling procesov - jednoduché algoritmy
- First Come, First Served
- Shortest Job First 
- Shortest Time-to-Completion First
- Round Robin

## Preemptive schedulers
In the old days of batch computing, a number of **non-preemptive** schedulers were developed; such systems would run each job to completion
before considering whether to run a new job. Virtually all modern schedulers are **preemptive**, and quite willing to stop one process from running in order to run another. This implies that the scheduler employs the mechanisms we learned about previously; in particular, the scheduler can
perform a **context switch**, stopping one running process temporarily and
resuming (or starting) another

## Multi-level feedback queue algoritmus
It has _multiple levels_ of queues, and uses _feedback_ to determine the priority of a given job, thus its name. History is its guide: pay attention to how jobs
behave over time and treat them accordingly.

**MLFQ rules:**

Let A and B be processes, then:
- **Rule 1:** If Priority(A) > Priority(B), A runs (B doesn’t).
- **Rule 2:** If Priority(A) = Priority(B), A & B run in round-robin fashion using the time slice (quantum length) of the given queue.
- **Rule 3:** When a job enters the system, it is placed at the highest priority (the topmost queue).
- **Rule 4:** Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced (i.e., it moves down one queue).
- **Rule 5:** After some time period S, move all the jobs in the system to the topmost queue.

MLFQ is interesting for the following reason: instead of demanding _a priori_ knowledge of the nature of a job, it observes the execution of a job and prioritizes it accordingly. In this way, it manages to achieve the best of both worlds: it can deliver excellent overall performance (similar to SJF/STCF) for short-running interactive jobs, and is fair and makes progress for long-running CPU-intensive workloads. For this reason, many systems use a form of MLFQ as their base scheduler.

## Proportional-share scheduler 
Proportional-share is based around a simple concept: instead of optimizing for turnaround or response time, a scheduler might instead try to guarantee that each job obtains a certain percentage of CPU time. Approaches: lottery scheduling, stride scheduling, and the Completely Fair Scheduler (CFS) of Linux. Lottery uses randomness in a clever way (via tickets) to achieve proportional share; stride does so deterministically. CFS is a bit like weighted round-robin with dynamic time slices, but built to scale and perform well under load; it is the most widely used fair-share scheduler in existence today

