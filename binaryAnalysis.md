
***
title: assembly
date: 2020-04-15 05:33:33+01:00
author: Gaxpert
***


# disassembly

It can be Intel syntax or AT&T syntax
Intel: Less verbose. **mnemonic destination, source**
Move 0x6 to edi
`mov edi, 0x6`
AT&T: More verbose, order of operands is reversed
`mov $0x6, %edi`
 
## Components of an Assembly Program

|  Type |    Example       |  Meaning   	|
| ------------- |-------------  | ------- |
|     Instruction   |    mov eax, 0          | Move 0 to eax       |
|    Directive    |    .section .text          | Place the following content into the .text section       |
|    Directive    |    .string "foobar"          | Define an ASCII string "foobar"       |
|    Directive    |    .long 0x12345678          | Define a doubleword with value 0x12345678       |
|    Label |    foo: .string "foobar"          | Define the "foobar" string with symbolic name foo       |
|    Comment    |    # This is a comment          | Human readable comment       |

### Directives
|  Directive      |   Size        | 
| ------------- |-------------  |
|    .string    |    ASCII string          | 
|    .byte    |    1 Byte          |
|    .word  |    2 Byte word         |
|    .long  |    4 Byte double word         |
|    .quad  |    8 Byte quad word         |

 
### Instructions
Three types of operands: register, memory and immediates

### Syscall
First we need to prepare the system by selecting its number and setting its operands as specified by the OS. For more info:
`man syscalls`

#### Arguments
In linux x86-64 Linux Systems the first six arguments to a function are passed in rdi, rsi, rdx, rcx, r8 and r9 respectively.

#### Red zone
On x86-64 systems there is a 128 byte "red zone", in which functions can use it as a scratch space and the OS won't touch it. 
The red zone is most useful for "leaf functions", that don't call any other functions


## Dissasembly
Default is AT&T, to use intel
`objdump -M intel -d a.out`
To see full dissasembly (including read-only data)
`objdump -M intel -D a.out`

## Compilation process
Preprocess --> Compilation --> Assembly --> Linking
Modern compilers merge some phases

#### Preprocess
Tell gcc to stop after preprocess (-E) nd to omit debugging information (-P)
`gcc -E -P file.c`
The preprocessor fully expands all macros

### Compilation
The source will be compiled to assembly. This is because all the existing languages, its easier to compile them all to assembly and have only one unified assembly compiler.

Compiler optimization will have a heavy impact in the resultant code. It can be specified in gcc with -00 to -03

Compile C to assembly into a "file.a"
`gcc -S -masm=intel file.c`

Note: All symbols (references to code and data) are still readable. On stripped binaries they won't...

### Assembly Phase
The input is a set of assembly files, and the output is a set of **object files**, sometimes refered as **modules**
`gcc -c file.c`
`file file.o`
> ch1_compilationExample.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
* Elf --> ELF specification for binary executable
* LSB --> Least Significant Byte first
* relocatable --> Means the file doesn't rely on being placed at a particular address to work. This means we are dealing with an object file and not an executable(there are position-independent executables, but these show up as shared objects).

### Linking Phase
Links together all the object files into a single binary executable. In modern systems the linking phase sometimes incorporates additional optimization called **link-time optimization(LTO)**

Object files are relocatable because they are compiled independently. Before the linking phase the addresses at which the referenced code and data will be placed are not known, so the object files contains only **relocation symbols**. In linking, references that rely on relocation symbols are called **symbolic references**.

Static libraries are merged into the binary executable, allowing references to be resolved.
Dynamic libraries (shared), are shared in memory across all programs on the system. Since the addresses of these libraries are not known, the linker leaves symbolic references, even in the final executable, and are only resolved when the binary is loaded into memory.

### Symbols
Read symbols
`readelf --syms a.out`


### Debugging symbols
For ELF --> DWARF format
For PE --> Microsoft Portable Debugging(PDB)

### Strip binary
Stripped binary --> No symbolic information
`strip --strip-all a.out`
The only symbols left are references for dynamic libraries, only loaded when the binary is loaded into memory

### Interpreter
When a binary is loaded into memory, the OS starts a new process for the program to un in, including a virtual address space (Virtual Memory Address). Then the OS maps an interpreter into the process virtual memory which takes over and runs the binary.
On Windows its --> ntdll.dll
On Linux its --> ld-linux.so
ELF binaries have a special section called **.interp** that specifies the path to the interpreter
`readelf -p .interp a.out`

The process of mapping the dynamic libraries to addresses its only done when they are invoked the first time, called **lazy binding**

## ELF
Exectuable and Linkable Format(ELF) is standard for linux. It consist only on 4 components
* Executable header
* Series of (optional) program headers
* Number of sections
* Series of (optional) section headers, one per section

### Executable header
Structured series of bytes telling its an ELF file, what kind of file it is, and where in the file to find all the other contents. The ELF specification is in "/usr/include/elf.h"
Check header information with:
`readelf -h a.out`

### Section headers
The code and data in an ELF format is divided into contiguos non overlapping chunks called sections. Sections don't have any predetermined structure, depending on the contents.
Every section is described by a **section header**. The section headers for all sections are contained in the **section header table**. This logical organization exist only during linking or static analysis and not during runtime.

### Section flags
Most important are:
* SHF_WRITE: Indicates if its writable at runtime. Makes it easy to distinguish between variables and static data
* SHF_ALLOC: Indicates the contents of the section to be loaded into memory when executing.
* SHF_EXECINSTR: Indicates that the section contains executable instructions.

List sections
`readelf --sections --wide a.out`
Notes 
* 'AX' in Flg field indicates sections is executable and no writable (usually executed sections are not writable)
* 'PROGBITS' in type means its used defined code

### Important sections
* .init: Executes code in .init before transfering control to binary (similar to a constructor)
* .fini: Executes code in .fini after binary is executed (similar to a destructor)
* .text: Where the main code of the program resides






## Linux analysis
### Ch5 ctf exercise
base64 decode
`base64 -d FILE`
Peak inside archive
`file -z FILE`
Find out which shared libs binary depends on
`ldd FILE`
Dump hex
`xxd FILE`
Extract bytes from file
`dd skip=52 count=64 if=... of=... bs=1`
Read executable header of elf
`readelf -h FILE`
Since we already have the start of the file correct, its missing the end of the file. We can calculate the bytes to extract it with:
`size = e_shoff + (e_shnum x e_shentsize)`
e_shoff => Start of section headers
e_shnum => Size of section headers
e_shentsize -> Number of section headers

C++ allows functions to be **overloaded**, which means that it can exist multiple functions with the same name, as long as they have different signatures. To eliminate duplicate functions, C++ emits **mangled** function names.
Mangled name => Combination of function name and parameters
Demangle function
`nm libXXX.so`
Ask nm to parse dynamic symbol table instead and demangle names
`nm -D --demangle libXXX.so`
Demangle function names, supports several formats and detects the correct one:
`c++filt MANGLED_NAME`

Tell the binary where to find a lib since its on a non standard directory
`export LD_LIBRARY_PATH=$(pwd) `

Get contents of raw data
`objdump -s --section .rodata FILE`

Dependencies for linking / libraries
`ldd FILE`






