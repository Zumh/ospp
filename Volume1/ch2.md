## Exercises
1. When a user process is interrupted or causes a processor exception, the x86
hardware switches the stack pointer to a kernel stack, before saving the current
process state. Explain why?

- **The x86 hardware switches the stack pointer to a kernel stack before saving the current process state to ensure that the kernel has a clean and protected environment to handle the interrupt or exception.**
  This is necessary because the user process may have modified the stack in an unexpected way, making it unsafe to use for kernel operations. By switching to a separate kernel stack, the kernel ensures that it has a known-good stack to work with, reducing the risk of a system crash.

2. For the “Hello world” program, we mentioned that the kernel must copy the string from the user program to screen memory. Why must the screen’s buffer memory be protected? Explain what might happen if a malicious application could alter any pixel on the screen, not just those within its own window.
- **The screen’s buffer memory must be protected because if a malicious application could alter any pixel on the screen, not just those within its own window, it could potentially take control of the machine.** For example, a virus could present a login prompt that looked identical to the system login, potentially inducing users to disclose their passwords to the attacker.

3. For each of the three mechanisms that supports dual-mode operation — privileged instructions, memory protection, and timer interrupts — explain what might go wrong without that mechanism, assuming the system still had the other two.
- **Without privileged instructions**, applications would not be prevented from executing privileged instructions and could potentially gain control of the system.
* **Without memory protection**, applications could overwrite kernel data structures or other applications' memory, leading to system crashes or security vulnerabilities.
* **Without timer interrupts**, the operating system would not be able to regain control of the processor from a rogue process and could potentially hang.
4. 
