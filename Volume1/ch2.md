## Exercises
1. When a user process is interrupted or causes a processor exception, the x86
hardware switches the stack pointer to a kernel stack, before saving the current
process state. Explain why?

- **The x86 hardware switches the stack pointer to a kernel stack before saving the current process state to ensure that the kernel has a clean and protected environment to handle the interrupt or exception.**

This is necessary because the user process may have modified the stack in an unexpected way, making it unsafe to use for kernel operations. By switching to a separate kernel stack, the kernel ensures that it has a known-good stack to work with, reducing the risk of a system crash.
