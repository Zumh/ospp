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

4. Suppose you are tasked with designing the security system for a new web browser that supports rendering web pages with embedded web page scripts. What checks would you need to implement to ensure that executing buggy or malicious scripts could not corrupt or crash the browser?
- **To ensure that executing buggy or malicious scripts does not corrupt or crash the browser, the following checks could be implemented:**
* **Validating input:** Before executing any script, the browser should validate the input to ensure that it is well-formed and does not contain any malicious code. This can be done by using a variety of techniques, such as regular expressions, whitelisting, or blacklisting.
* **Sandboxing:** The browser should run scripts in a sandboxed environment that prevents them from accessing or modifying the browser's memory or other resources. This can be done by using a variety of techniques, such as creating a separate process for each script or using a virtual machine.
* **Restricting access to system resources:** The browser should restrict scripts from accessing system resources, such as the file system or network, without the user's explicit permission. This can be done by using a variety of techniques, such as using a security policy or using a content security policy.
* **Monitoring script behavior:** The browser should monitor the behavior of scripts to detect any malicious activity. This can be done by using a variety of techniques, such as using a script debugger or using a intrusion detection system.
  
5. Define three types of user-mode to kernel-mode transfers.
- **Here are three types of user-mode to kernel-mode transfers:**

* **Interrupts:** These are triggered by hardware events, such as a timer expiring or a device completing an I/O operation.
* **Exceptions:** These are triggered by software events, such as a division by zero or an invalid memory access.
* **System calls:** These are triggered by user programs to request services from the operating system, such as reading a file or starting a new process.
6. 
