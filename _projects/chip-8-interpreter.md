---
layout: project
title: "Chip-8 Interpreter and Fuzzing"
slug: chip-8
status: "Active - v1.0"
tags: [C, Fuzzing, Cybersecurity, Emulators]
brief: "A short study into emulator development, why static analysis tools are insuficcient and fuzzing basics"
---

> **Status:** Active - v1.0

This project is an attempt to write an interpreter for the Chip-8 programming language based on Cowgod's technical reference.

As per [Wikipedia](https://en.wikipedia.org/wiki/CHIP-8) :
> "CHIP-8 is an interpreted programming language, developed by Joseph Weisbecker on his 1802 microprocessor. It was initially used on the COSMAC VIP and Telmac 1800, which were 8-bit microcomputers made in the mid-1970s.
> CHIP-8 was designed to use less memory than other programming languages like BASIC, while still being easier to program than machine code. The language looks like machine code as it uses hexadecimal codes for instructions, but as an interpreter, like BASIC, it does not need to be assembled or linked before running.
> CHIP-8 interpreters have since been made for many devices, such as home computers, microcomputers, graphing calculators, mobile phones, and video game consoles.""

As per [Cowgod's technical reference](https://github.com/trapexit/chip-8_documentation/blob/master/Misc/Cowgod%27s%20CHIP-8%20Technical%20Reference.pdf)
> "Chip-8 is a simple, interpreted, programming language which was first used on some do it-yourself computer systems in the late 1970s and early 1980s. The COSMAC VIP, DREAM 6800, and ETI 660 computers are a few examples. These computers typically were designed to use a television as a display, had between 1 and 4K of RAM, and used a 16 key hexadecimal keypad for input. The interpreter took up only 512 bytes of memory, and programs, which were entered into the computer in hexadecimal, were even smaller.
> In the early 1990s, the Chip-8 language was revived by a man named Andreas Gustafsson. He created a Chip-8 interpreter for the HP48 graphing calculator, called Chip-48. The HP48 was lacking a way to easily make fast games at the time, and Chip-8 was the answer. Chip-48 later begat Super Chip-48, a modification of Chip-48 which allowed higher resolution graphics, as well as other graphical enhancements."

## A brief technical note

The programming language is similar to an assembly language, even though it is not assembled to machine language. It defines a certain virtual machine with a couple of interesting properties:
    - It can address up to 4kib of memory.
    - It is register based, having:
        - 16 general purpose 8-bit registers
        - one general purpose 16-bit register.
        - Two special purpose registers, one for the delay timer and one for the sound timer. These are decremented automatically at 60hz if non-zero.
        - Two pseudo registers, being the program counter which points to the current instruction being executed and the stack pointer, which points to the topmost level of the stack.
    - The stack, which is a 16-bit array of 16 vales, used to store the address that the interpreter should return to when finished with a subroutine. This means that Chip-8 allows for up to 16 levels of nested subroutines

To make it I basically used stock-standard C plus some libsdl2 code for graphics.

I wanted (and still want) to code an emulator, and I had heard that building a Chip-8 interpreter was emulator development's "hello, world" project. Even though technically Chip-8 is a programming language and not some machine to be emulated, because of it's design it is very close to building an emulator that can read a ROM and execute instructions. It has all of the main parts of an emulator: a RAM stack, a ROM "cartridge", some display logic, hardware input, etc.

I also wanted to add my little cybersecurity spin on it (as one does), and decided that his project was a good way to learn more about fuzzing and debugging. So I deliberately wrote the lazyest possible code that (a) worked and (b) made the compiler, linter and Opengrep stop screaming at me. The result was v0, nicknamed Dangerous Donkey because it is unsafe and dumb (although donkeys are very smart animals, but y'all ain't ready for this discussion).

In the end this turned out to be an exercise in why static analysis tools are not enough, and why good security practices and dynamic security analysis is pretty much always needed. With a touch of learning about more tools like GDB and libFuzzer (a Clang/LLVM builtin).


The interpreter basically operates the fetch-decode-execute loop of a CPU. It reads the two bytes pointed by the PC, interpret them as instructions, decodes them into one of the standard Chip-8 set and finally executes the instruction according to the arguments passed, advancing the PC by two bytes.

The instructions are decoded in a large switch statement, which is far from the most elegant or scalable solution, but it worked fine and it meant that I didn't have to mess with function pointers.

Instructions are executed by basically just describe their behavior in C. They mostly involve performing operations on the various registers, modifying or reading from memory and interacting with "hardware" like drawing graphics or getting keyboard input.

## The fuzzing part

As I was coding I just kind of decided that I was going to try and fuzz this thing. It gave me a convenient excuse to write the most lazy and dangerous C code that I could get away with without either the compiler, linter, SAST or my own conscience screaming at me.

But it also was a great opportunity, as I think a project like this has many similarities to "normal" production code (at least hen it comes to the application security considerations), but in a more simplified and digestible manner, which makes for especially good didactic material.

My little program, a lot like something like an API webserver, is a program that owns some large region or memory, interprets arbitrary user-defined data as commands, performs operations based on these commands, may or may not modify the memory it owns, may or may not interact with some external function like hardware or another service and terminates.

Of course there are differences: the API webserver will use dynamic memory allocation and multithreading, will receive it's commands through the network stack rather than through reading from a file, and will have to return a response to the requestee, but those differences end up being helpful simplifications that don't change the underlying threat model as much as you would think.

![chip8 intepreter threat model vs API webserver threat model](/assets/blog/chip8/chip8-threat-models-resize.png)

The Chip-8 interpreter is also very easily fuzzed, as it has one very big input file that may contain arbitrary user defined instructions for our machine to run.

And because we will be focusing on the security of the interpreter and not of the programs it runs, we basically don't need to think about what the ROM does inside of it's own trust boundary, but rather when it can (a) crash our program or most dangerously (b) somehow convice our interpreter to make unwanted changes outside of it's own trust boundary, such as running a command in the OS.

As I've said, a very good exercise for Appsec!

I also want to try to write some sort of exploit to exploit a vulnerability in my unsafe code, and prove that by fixing them we can avoid exploits.

## Roadmap
- [x] Create the baseline unsafe program
- [x] Document it's flaws by reading code, performing static analysis and fuzzing
- [ ] Write exploits for the flaws
- [ ] Create a fixed version
- [ ] Perform the same analysis techniques to validate the correction of the flaws

