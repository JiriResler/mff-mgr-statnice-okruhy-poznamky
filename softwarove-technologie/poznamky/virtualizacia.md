# KEY CPU VIRTUALIZATION TERMS (MECHANISMS)
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
