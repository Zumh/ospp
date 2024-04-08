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
  
6. Define four types of kernel-mode to user-mode transfers.
- **Here are four types of kernel-mode to user-mode transfers:**

* **New process:** The kernel creates a new process by copying the program into memory, setting the program counter to the first instruction of the process, setting the stack pointer to the base of the user stack, and switching to user mode.
* **Resume after an interrupt, processor exception, or system call:** When the kernel finishes handling a request, it resumes execution of the interrupted process by restoring its program counter (in the case of a system call, the instruction after the trap), restoring its registers, and changing the  mode back to user level.
* **Switch to a different process:** In some cases, such as on a timer interrupt, the kernel switches to a different process than the one that had been running before the interrupt. Since the kernel will eventually resume the old process, the kernel needs to save the process state — its program counter, registers, and so forth — in the process’s control block. The kernel can then resume a different process by loading its state — its program counter, registers, and so forth — from the process’s control block into the processor and then switching to user mode.
* **User-level upcall:** Many operating systems provide user programs with the ability to receive asynchronous notification of events. The mechanism, which we describe in Section 2.8, is similar to kernel interrupt handling, except at user level.

7. Most hardware architectures provide an instruction to return from an interrupt, such as iret. This instruction switches the mode of operation from kernel-mode to user-mode.
  a. Explain where in the operating system this instruction would be used.
  b. Explain what happens if an application program executes this instruction.
  - **a. Where in the operating system this instruction would be used:**
   
  The iret instruction is used in the operating system to return from an interrupt, processor exception, or system call back to user mode. It is typically used at the end of an interrupt handler or system call handler to restore the user-level state and resume execution of the interrupted process.
  
  - **b. What happens if an application program executes this instruction:**
  
  If an application program executes the iret instruction, it will likely cause a processor exception. This is because the iret instruction can only be executed in kernel mode, and application programs are normally not allowed to execute kernel-mode instructions. Attempting to execute iret from user mode will result in an illegal instruction exception, and the operating system will likely terminate the application.

8. A hardware designer argues that there is now enough on-chip transistors to provide 1024 integer registers and 512 floating point registers. As a result, the compiler should almost never need to store anything on the stack. As an operating system guru, give your opinion of this design.
  - a. What is the effect on the operating system of having a large number of registers?
  - b. What hardware features would you recommend adding to the design?
  - c. What happens if the hardware designer also wants to add a 16-stage pipeline into the CPU, with precise exceptions. How would that affect the user-kernel switching overhead?
  - **A large number of registers would have the following effects on the operating system:**
  
  * **Reduced stack usage:** With a large number of registers, the compiler would be able to keep more variables and temporary values in registers, reducing the need to store them on the stack. This would lead to faster execution times, as accessing registers is faster than accessing memory.
  * **Reduced context switching overhead:** When switching between user and kernel mode, the operating system must save and restore the contents of the registers. A large number of registers would increase the time required for context switching, potentially impacting the overall performance of the system.
  * **Increased complexity:** A large number of registers would require more complex hardware and software to manage them, potentially making the system more difficult to design and implement.
  
  - **Hardware features that could be added to the design to mitigate these effects include:**
  
  * **Register renaming:** Register renaming allows the hardware to map logical registers to physical registers, enabling the compiler to use more registers than are physically available. This would reduce the impact of having a large number of registers on context switching overhead.
  * **Speculative execution:** Speculative execution allows the hardware to execute instructions based on predicted branch outcomes, potentially hiding the latency of accessing memory. This could reduce the impact of increased stack usage on execution times.
  
  - **Adding a 16-stage pipeline into the CPU with precise exceptions would affect the user-kernel switching overhead in the following ways:**
  
  * **Increased latency:** A 16-stage pipeline would increase the number of cycles required to execute an instruction, potentially increasing the latency of user-kernel switches.
  * **Precise exceptions:** Precise exceptions allow the hardware to identify the exact instruction that caused an exception, enabling the operating system to handle exceptions more efficiently. This could reduce the overhead of handling exceptions, potentially offsetting the increased latency of the pipeline.
  
  Overall, while a large number of registers can improve performance, it also introduces challenges for the operating system. Careful design and implementation of hardware and software features are necessary to mitigate these challenges and ensure that the benefits of a large number of registers are realized.

9. With virtual machines, the host kernel runs in privileged mode to create a virtual machine that runs in user mode. The virtual machine provides the illusion that the guest kernel runs on its own machine in privileged mode, even though it is actually running in user mode.

Early versions of the x86 architecture (pre-2006) were not completely virtualizable — these systems could not guarantee to run unmodified guest operating systems properly. One problem was the popf “pop flags” instruction that restores the processor status word. When popf was run in privileged mode, it changed both the ALU flags (e.g., the condition codes) and the systems flags (e.g., the interrupt mask). When popf was run in unprivileged mode, it changed just the ALU flags.

   - **a. Why do instructions like popf prevent transparent virtualization of the (old) x86 architecture?**
  
  Instructions like popf prevent transparent virtualization of the old x86 architecture because they can be used to modify the processor status word (PSW) in a way that is not consistent with the expected behavior of a virtual machine. The PSW contains flags that control the processor's behavior, such as the interrupt mask and the condition codes. When popf is run in privileged mode, it modifies both the ALU flags and the system flags in the PSW. However, when popf is run in unprivileged mode, it only modifies the ALU flags. This discrepancy can cause problems for virtual machines because they rely on the PSW to maintain the illusion that the guest kernel is running on its own machine in privileged mode. If the PSW is modified in an unexpected way, the virtual machine may not be able to function properly.
  
  - **b. How would you change the (old) x86 hardware to fix this problem?**
  
  One way to fix this problem would be to modify the x86 hardware so that popf always modifies the same flags in the PSW, regardless of whether it is run in privileged mode or unprivileged mode. This would ensure that the PSW is always consistent with the expected behavior of a virtual machine. Another way to fix this problem would be to provide a separate instruction that is used to modify the system flags in the PSW. This instruction would only be available in privileged mode, which would prevent user-level programs from modifying the system flags in an unexpected way.
  
10. Which of the following components is responsible for loading the initial value in the program counter for an application program before it starts running: the compiler, the linker, the kernel, or the boot ROM?

- **The boot ROM is responsible for loading the initial value in the program counter for an application program before it starts running.**

When a computer boots, it sets the machine’s program counter to start executing at a pre-determined position in memory. Since the computer is not yet running, the initial machine instructions must be fetched and executed immediately after the power is turned on before the system has had a chance to initialize its DRAM. Instead, systems typically use a special read-only hardware memory (Boot ROM) to store their boot instructions. On most x86 personal computers, the boot program is called the BIOS, for “Basic Input/Output System”.

11. We described how the operating system kernel mediates access to I/O devices for safety. Some newer I/O devices are virtualizable — they permit safe access from user-level programs, such as a guest operating system running in a virtual machine. Explain how you might design the hardware and software to get this to work. (Hint: The device needs much of the same hardware support as the operating system kernel.)

- **To design hardware and software for virtualizable I/O devices, we can leverage the hardware support provided for the operating system kernel to ensure safe access from user-level programs.**

**Hardware Support:**

* **Privilege Levels:** Establish two privilege levels – kernel-mode and user-mode – to restrict access to sensitive operations and system resources.
* **Protected Instructions:** Designate certain instructions as privileged and only executable by the kernel.
* **Memory Protection:** Implement memory management units or page tables to restrict user-level programs from accessing kernel memory, device registers, and other protected areas.
* **Timer Interrupts:** Provide a hardware timer that can periodically interrupt the processor to regain control from the current process.

**Software Design:**

* **Virtual Device Drivers:** Create device drivers that implement the device's functionality in user mode, providing a safe and isolated execution environment.
* **System Call Interface:** Define a system call interface that allows user-level programs to access virtual devices through the kernel. The kernel will validate and translate these system calls to appropriate instructions for the device driver.
* **Guest Operating System Interaction:** For virtual machines, the guest operating system must be aware of the virtual device drivers and interact with them through the system call interface. The host operating system will provide necessary hardware virtualization support to make the guest operating system's access to virtual devices appear as if they were real hardware devices.

**Additional Considerations:**

* **Hardware Virtualization Extensions:** Utilize hardware virtualization extensions, such as Intel VT-x or AMD-V, to provide additional isolation and security measures for virtualized I/O devices.
* **Resource Allocation:** Implement fair resource allocation mechanisms to ensure that user-level programs do not monopolize access to virtualized I/O devices.
* **Error Handling:** Design error handling mechanisms to gracefully manage device failures and protect the system from potential security breaches.

12. stem calls vs. procedure calls: How much more expensive is a system call than a procedure call? Write a simple test program to compare the cost of a simple procedure call to a simple system call (getpid() is a good candidate on UNIX; see the man page). To prevent the optimizing compiler from “optimizing out" your procedure calls, do not compile with optimization on. You should use a system call such as the UNIX gettimeofday() for time measurements. Design your code so the measurement overhead is negligible. Also, be aware that timer values in some systems have limited resolution (e.g., millisecond resolution).
Explain the difference (if any) between the time required by your simple procedure call and simple system call by discussing what work each call must do.
  - **System calls are more expensive than procedure calls because they involve a switch from user mode to kernel mode, which requires additional hardware and software overhead.**
  
  To measure the cost difference, you can write a simple test program like this:
  
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #include <sys/time.h>
  
  int main() {
    struct timeval start, end;
  
    // Measure the cost of a procedure call.
    gettimeofday(&start, NULL);
    int x = 5;
    gettimeofday(&end, NULL);
  
    // Measure the cost of a system call.
    gettimeofday(&start, NULL);
    int y = getpid();
    gettimeofday(&end, NULL);
  
    // Calculate the time difference.
    double proc_time = (end.tv_sec - start.tv_sec) * 1000000 + (end.tv_usec - start.tv_usec);
    double sys_time = (end.tv_sec - start.tv_sec) * 1000000 + (end.tv_usec - start.tv_usec);
  
    // Print the results.
    printf("Procedure call time: %f microseconds\n", proc_time);
    printf("System call time: %f microseconds\n", sys_time);
  
    return 0;
  }
  ```
  
  **When you run this program, you should see that the system call is significantly more expensive than the procedure call. The exact difference will vary depending on the system you are using.**

13. Suppose you have to implement an operating system on hardware that supports interrupts and exceptions but does not have a trap instruction. Can you devise a satisfactory substitute for traps using interrupts and/or exceptions? If so, explain how. If not, explain why.
  - **It is not possible to create a satisfactory substitute for traps using only interrupts and exceptions on hardware that does not support a trap instruction.**
  
  Traps are a special type of interrupt that is used to transfer control from user mode to kernel mode. They are typically invoked by executing a special instruction, such as the SVC instruction on ARM processors. When a trap occurs, the hardware saves the current processor state and jumps to a specific location in the kernel.
  
  Interrupts and exceptions are both mechanisms for transferring control from user mode to kernel mode, but they are not as flexible as traps. Interrupts are typically triggered by external events, such as the arrival of a network packet or the completion of a disk I/O operation. Exceptions are typically triggered by internal events, such as a division by zero or an attempt to access an invalid memory address.
  
  Neither interrupts nor exceptions can be used to transfer control to an arbitrary location in the kernel. This makes it difficult to implement a general-purpose operating system on hardware that does not support traps.
  
  For example, consider the following scenario:
  
  * A user-mode program is executing and encounters a page fault.
  * The hardware generates a page fault exception.
  * The operating system kernel needs to handle the page fault by loading the missing page into memory.
  * The operating system kernel needs to return to the user-mode program after handling the page fault.
  
  If the hardware does not support traps, the operating system kernel would have to use a combination of interrupts and exceptions to implement this scenario. This would be a complex and error-prone process.
  
  In contrast, if the hardware supports traps, the operating system kernel could simply execute a trap instruction to transfer control to the page fault handler. This would be a much simpler and more reliable solution.
  
  **Therefore, it is not possible to create a satisfactory substitute for traps using only interrupts and exceptions on hardware that does not support a trap instruction.**
14. Suppose you have to implement an operating system on hardware that supports exceptions and traps but does not have interrupts. Can you devise a satisfactory substitute for interrupts using exceptions and/or traps? If so, explain how. If not, explain why.
  - **It is possible to create a satisfactory substitute for interrupts using a combination of exceptions and traps on hardware that does not support interrupts.**
  
  Exceptions are typically triggered by internal events, such as a division by zero or an attempt to access an invalid memory address. Traps are a special type of exception that is used to transfer control from user mode to kernel mode. They are typically invoked by executing a special instruction.
  
  On hardware that does not support interrupts, we can use a combination of exceptions and traps to implement a satisfactory substitute for interrupts. We can do this by defining a special exception that is triggered when a specific event occurs. For example, we could define an exception that is triggered when a timer expires.
  
  When the special exception occurs, the hardware would save the current processor state and jump to a specific location in the kernel. The kernel would then handle the event and return to the user-mode program.
  
  This approach would be less efficient than using interrupts, but it would be a viable alternative on hardware that does not support interrupts.
  
  Here is an example of how we could use a combination of exceptions and traps to implement a timer interrupt:
  
  1. Define a special exception that is triggered when a timer expires.
  2. In the kernel, define a handler for the special exception.
  3. In the user-mode program, set a timer to expire at a specific time.
  4. When the timer expires, the hardware will trigger the special exception.
  5. The kernel will handle the special exception by calling the handler for the special exception.
  6. The kernel will return to the user-mode program.
  
  This approach would allow us to implement a timer interrupt on hardware that does not support interrupts.

