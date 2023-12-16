---
tags:
  - uni
  - sem1
  - flashcards/CompSec/OSSecurity
---
# `UNIX`
What are the two execution modes a computer running `UNIX` has?
?
- **Kernel mode** - direct access to hardware resources
- **User mode** - can only make `syscalls` to the kernel to interface these resources
<!--SR:!2023-12-19,4,290-->

What do we use to access input/output (I/O) devices?::An *Application Programmer Interface* (API)
<!--SR:!2023-12-19,4,290-->

# Processes

What is a process in `UNIX`?
?
A process is an instance of a running program. This allows the same program to be ran with different permissions, at the same time.
<!--SR:!2023-12-19,4,290-->

How do we identify different processes of the same program?::Each process has it's **unique** *process ID* (`pid`)
<!--SR:!2023-12-19,4,290-->

What is the `uid`?::The *user ID* is the user *associated* with the running process. They can be grouped using a *group ID* (`gid`)
<!--SR:!2023-12-19,4,290-->

What is `uid` "`0`"?::`root`
<!--SR:!2023-12-19,4,290-->

How does the `euid` decide the `uid`?::Usually the *effective user ID* is set to the user who executed the process, but in some cases the application's owner - with higher privileges than the user who would run it - can set the `euid` to their `uid`.
<!--SR:!2023-12-19,4,290-->

What determines the *saved user ID* (`suid`)?::The `euid` before the last modification.
<!--SR:!2023-12-19,4,290-->

How can processes securely communicate to each other?::Using *sockets* or *pipes*, we can send these messages through RAM and have the kernel ensure security.
<!--SR:!2023-12-19,4,270-->

All requests for operations go through a ==reference monitor, which handles all specific access control policies==
<!--SR:!2023-12-17,2,250-->

The three types of `UNIX` permissions are:
?
- Read (`r`)
- Write (`w`)
- Execute (`x`)
<!--SR:!2023-12-19,4,290-->

# Security Principles

Least Privilege - ==Users and programs should only access the data and resources required to perform its function.==
<!--SR:!2023-12-18,4,250-->

Privilege Separation - ==Segment system into components we can limit access to.==
<!--SR:!2023-12-18,4,270-->

Open design - ==Security of a mechanism should not depend on its secrecy. The design will always get leaked!==
<!--SR:!2023-12-19,4,290-->

Fail-safe defaults - ==Default configuration should be conservative, e.g. new user should be granted least privileges by default.==
<!--SR:!2023-12-17,3,230-->

Complete mediation - ==Every access to a resource must be checked for compliance with security policy.==
<!--SR:!2023-12-19,4,270-->

Usable security - ==UIs and security mechanisms should be designed with the ordinary user in mind – the users should be supported in interacting in a secure way with the system – you can’t blame users!==
<!--SR:!2023-12-17,3,230-->

# `x86` memory

The `UNIX` stack and heap grow ==down and up== respectively
<!--SR:!2023-12-19,4,290-->

What does the `%eip` point to?::The next instruction to be executed
<!--SR:!2023-12-19,4,290-->

What does the `%esp` point to?::The top of the stack, at all times
<!--SR:!2023-12-19,4,290-->

What does the `%ebp` point to?::The base of the current stack [[OS Security#Stack Frames|frame]] of the current function call
<!--SR:!2023-12-18,4,270-->

When a function is called, the ==old frame pointer `%eip`== is pushed onto the stack
<!--SR:!2023-12-16,1,170-->

# Memory attacks

How can we exploit `C`'s many unsafe commands to overwrite variables?::By providing extremely long inputs that will be written past the boundaries set for the variable's memory grounds.
<!--SR:!2023-12-19,4,290-->

How does *control hijacking* allow attackers to exploit a system?::The data inserted is malicious code, alongside an address for this (perhaps in a `NOP` sled) for the `%eip` to point to.
<!--SR:!2023-12-19,4,290-->

How do *stack canaries* protect against buffer overrun attacks?::Stack canaries are *random* values, inserted at runtime, before the stack return pointer. They will likely be overwritten in primitive overflow attacks which detects their presence.
<!--SR:!2023-12-19,4,290-->

How does *Write `XOR` Execute* (W^X) protect against control hijacking?::By making regions in memory *either* executable or writeable. This protects data (malicious code) being executed, or code portions being rewritten.
<!--SR:!2023-12-19,4,290-->

How does the `return-to-libc` attack get around W^X protection?::Since most `C` programs will use `libc` to make `syscalls`, we can write code that simply points to the `exec()` function in this and spawn a *shell*.
<!--SR:!2023-12-19,4,290-->

How does *Address Space Layout Randomisation* (ALSR) defend against `return-to-libc` attacks?::By randomising the address space of certain functions (such as `libc`) we can avoid these attacks being repeatable (to the same payload)
<!--SR:!2023-12-19,4,290-->