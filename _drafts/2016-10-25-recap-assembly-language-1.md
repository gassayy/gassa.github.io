---
layout: post
title: Recap Assembly Language 1
author: gassa
---

Recently, I have to quickly recap myself assembly language on x86 for my intern
project at Austrilia. I think it will be nice to summrize them in my blog for
my future references.

Firstly of all, I would like to quickly going throught the processor registers,
includeing eight general purpose registers, six segmented registers, a flag 
register and an instruction pointer register. In application development with 
advanced programming language, developers are not directly exposed to them, but 
they are very important to compiler implementation and system kernel development. 
These registers are like variables in the processor. Using registers instead of
memory to store temporary results makes computation faster and cleaner. While 
each register of the processor has specific meaning to the programm. I will 
emunerate each of them by categories with 16-bits naming convention.

### 8 general purpose registers(GPRs)
* Accumulator register (AX) is used arithmetic operations.
* Base register (BX) is used as a based pointer for memory/data access, combine with DS in segmented mode.
* Counter register (CX) is used as shift/rotate instructions and a loop counter.
* Data register (DX) is used in arithmetic operations, I/O port access and interrupt calls.
* Stack pointer register (SP) holds the top address of the stack.
* Stack base Pointer register (BP) holds the base address of the stack.
* Source index register (SI) is used for string and memory array copying
* Destination index register (DI) is used for string, memory array copying and setting.

With the development of processor, all registers have been enlarged from 8 bits
to 64 bits.  As the following table shows, the 16-bits naming convention is
identified by its two-letter abbreviation from the list above. This is also the
naming convention that nornally used in textbooks. The 32-bits registers are 
named with an "E" (extended) prefix. Similarly, the 64-bits version registers 
are renamed by replacing the letter "E" with the letter "R". 

|Register|Accumulator|Counter |  Data  |  Base  |Stack Pointer|Stack Base Pointer| Source  |Destination|
|--------|:---------:|:------:|:------:|:------:|:-----------:|:----------------:|:-------:|:---------:|
|64-bit  |RAX        |RCX     |RDX     |RBX     |RSP          |RBP               |RSI      |RDI        |
|32-bit  |EAX        |ECX     |EDX     |EBX     |ESP          |EBP               |ESI      |EDI        |
|16-bit  |AX         |CX      |DX      |BX      |SP           |BP                |SI       |DI         |
|8-bit   |      AH/AL|   CH/CL|   DH/DL|   BH/BL|             |                  |         |           |

Additionally, 64-bit x86 adds 8 more general-purpose registers, named from R8
to R15, and includes SSE2, so each 64-bit x86 CPU has at least 8 registers
(named XMM0~XMM7) that are 128 bits wide, but only accessible through SSE
instructions.  


### 6 segmented registers
Stack segment register (SS) holds the Stack segment your program uses
Code segment register (CS) holds the code segment in which a program runs.
Data segment register (DS) holds the data segment that a program accesses.
Extra segment registers (ES, FS, GS) are extra segment registers available.

Notice that the contemporary operatiing systems (Linux and Windows) disabled
these segment registers and replace them with OS memory model.


### EFLAGS register and instruction pointer register 

The EFLAGS is a 32-bit register used as a collection of bits representing
Boolean values to store the results of operations and the state of the
processor, more detials can be found on [wiki
page](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture).

The instruction pointer (IP) contains the address of the next instruction to 
be executed if no branching is done.


## Memory and Addressing Modes

### Declaring static data regions 

We can declare static data regions (similar to global variables) in x86
assembly using special assembler directives (`DB DW DD`) for this purpose.

```assembly
.DATA           
var DB 64      #Declare a byte, referred to as location var, containing the value 64.
var2    DB ?   #Declare an uninitialized byte, referred to as location var2.
DB 10          #Declare a byte with no label, containing the value 10. Its location is var2 + 1.
X   DW ?       #Declare a 2-byte uninitialized value, referred to as location X.
Y   DD 30000   #Declare a 4-byte value, referred to as location Y, initialized to 30000.
Z   DD 1, 2, 3         #Declare three 4-byte values, initialized to 1, 2, and 3. The value of location Z + 8 will be 3.
bytes   DB 10 DUP(?)   # Declare 10 uninitialized bytes starting at location bytes.
arr DD 100 DUP(0)      # Declare 100 4-byte words starting at location arr, all initialized to 0
str DB 'hello',0       # Declare 6 bytes starting at the address str, initialized to the ASCII character values for hello and the null (0) byte.
```

### Address Memory

```assembly
mov eax, [ebx]  # Move the 4 bytes in memory at the address contained in EBX into EAX
mov [var], ebx  # Move the contents of EBX into the 4 bytes at memory address var. (Note, var is a 32-bit constant).
mov eax, [esi-4]    # Move 4 bytes at memory address ESI + (-4) into EAX
mov [esi+eax], cl   # Move the contents of CL into the byte at address ESI+EAX
mov edx, [esi+4*ebx]        # Move the 4 bytes of data at address ESI+4*EBX into EDX
```

### Size Directives

In general, the intended size of the data item at a given memory address can be
inferred from the assembly code instruction in which it is referenced.  For
example, `mov eax, [ebx]`, the size of memory region is could be inferred from
the size of the register operand. When we were loading a 32-bit register, the
assembler could infer that the region of memory we were referring to was 4
bytes wide. When we were storing the value of a one byte register to memory,
the assembler could infer that we wanted the address to refer to a single byte
in memory.

However, in some cases, the size of a referred-to memory region is ambiguous.
Consider the instruction `mov [ebx], 2`. Should this instruction move the value 2
into the single byte at address EBX? Perhaps it should move the 32-bit integer
representation of 2 into the 4-bytes starting at address EBX. Since either is a
valid possible interpretation, the assembler must be explicitly directed as to
which is correct. The size directives `BYTE PTR, WORD PTR, and DWORD PTR` serve
this purpose, indicating sizes of 1, 2, and 4 bytes respectively.



## Subset of x86 instruction

Instruction Notations:


|-------|--------------------------------------------------------------------|
|\<reg32\>| Any 32-bit register (EAX, EBX, ECX, EDX, ESI, EDI, ESP, or EBP)    |
|\<reg16\>| Any 16-bit register (AX, BX, CX, or DX)                            |
|\<reg8\> | Any 8-bit register (AH, BH, CH, DH, AL, BL, CL, or DL)             |
|\<reg\>  | Any register                                                       |
|\<mem\>  | A memory address (e.g., [eax], [var + 4], or dword ptr [eax+ebx])  |
|\<con32\>| Any 32-bit constant                                                |
|\<con16\>| Any 16-bit constant                                                |
|\<con8\> | Any 8-bit constant                                                 |
|\<con\>  | Any 8-, 16-, or 32-bit constant                                    |

## Data Movement Instructions

### mov

```assembly
mov eax, ebx            #copy the value in ebx into eax
mov byte ptr [var], 5   #store the value 5 into the byte at location var
```
### push
```assembly 
push eax — push eax on the stack
push [var] — push the 4 bytes at address var onto the stack
```

### pop
```assembly
pop edi — pop the top element of the stack into EDI.
pop [ebx] — pop the top element of the stack into memory at the four bytes starting at location EBX.
```

### lea — Load effective address
```assembly
lea edi, [ebx+4*esi] — the quantity EBX+4*ESI is placed in EDI.
lea eax, [var] — the value in var is placed in EAX.
lea eax, [val] — the value val is placed in EAX.
```


## Arithmetic and Logic Instructions

* add, sub, inc/dec, imul, idiv
* and, or, xor

## Control Flow Instructions

* jmp
* je, jne, ...
* cmp
* call, ret



OK, this is really boring. The fun part is the next blog that introduces low-level
programming with assemly, and they are basic premitives for compiler and 
runtime implements.







