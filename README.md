Reverse Engineering & Binary Exploitation Challenges

This repository contains my hands-on analysis and write-ups for three Linux ELF binary challenges.
I put this together to document my reverse engineering process.
Heavily utilizing Ghidra for static analysis, digging into x86-64 assembly to figure out how these programs operate under the hood, deobfuscate logic, and ultimately exploit memory vulnerabilities.
You can read the full, step-by-step breakdown (with Ghidra screenshots and assembly analysis) in the PDF attached to this repo.

Tools & Skills
Demonstrated Static Analysis & Disassembly (Ghidra, x86-64 Assembly)
Deobfuscation (Analyzing stripped binaries, manual function mapping)
Memory Corruption / Vulnerability Research (Use-After-Free, Pointer Manipulation)
Linux Memory Management (LIFO Heap Allocation)

The Challenges
Challenge 1: Bypassing Pre Main Argument Logic
In this challenge, standard argument passing wasn't working. By analyzing the _start function before the code even hit main, I discovered a bitwise shift right (SHR AL, 0x1) operating on the argc value, which divided the argument count by two. Once I understood the underlying math, I bypassed the check by feeding the binary exactly 5 dummy arguments to satisfy the expected value in main.

Challenge 2: Reversing a Stripped Binary & Custom Algorithms
This binary was completely stripped. I started from __libc_start_main and manually mapped out the program's execution flow, renaming abstract functions in Ghidra to reconstruct the logic. I eventually reverse-engineered a custom password generation algorithm that used a Fibonacci sequence, applied a modulo 26 operation, and mapped the results to ASCII characters to dynamically generate the correct password (ghnuhbijra) based on user input.

Challenge 3: Use After Free (UAF) Exploitation
This was a custom login prompt with a fatal flaw. The program allocated memory for a username and password, but if the username wasn't exactly "root", it freed the memory without nullifying the pointers. Knowing that Linux manages freed memory chunks using a LIFO (Last-In, First-Out) structure, I triggered the free, and then strategically reallocated the memory by feeding a dummy password, followed by "root". This forced the dangling username pointer to point at the newly allocated "root" string, bypassing the restriction and granting admin access.
