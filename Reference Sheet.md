# Table of Contents
- [Table of Contents](#table-of-contents)
- [Registers](#registers)
  - [General Purpose Registers](#general-purpose-registers)
  - [16-bit and 8-bit Register Subdivisions](#16-bit-and-8-bit-register-subdivisions)
  - [Segment Registers](#segment-registers)
  - [Flags Register (EFLAGS)](#flags-register-eflags)


# Registers
## General Purpose Registers
| Register | Size | Description | Common Uses | Example |
| -------- | ------- | ------- | ------- | ------- |
| EAX  |  32-bit  |  Accumulator Register  |  Return values, arithmetic operations  |  mov eax, 100  |
| EBX  |  32-bit  |  Base Register  |  Base pointer for memory access  |  mov ebx, OFFSET array  |
| ECX  |  32-bit  |  Counter Register  |  Loop counters, string operations  |  loop loopLabel  |
| EDX  |  32-bit  |  Data Register  |  I/O operations, multiplication/division  |  mul ebx ; result in EDX:EAX  |
| ESI  |  32-bit  |  Source Index  |  	String operations source pointer  |  lodsb ; load from [ESI]  |
| EDI  |  32-bit  |  Destination Index  |  String operations destination pointer  |  stosb ; store to [EDI]  |
| ESP  |  32-bit  |  Stack Pointer  |  Points to top of stack  |  push eax ; ESP decrements  |
| EBP  |  32-bit  |  Base Pointer  |  Stack frame base pointer  |  mov ebp, esp  |

## 16-bit and 8-bit Register Subdivisions
| 32-bit  |	16-bit  |	8-bit High  |	8-bit Low	Example Usage  |  Example |
| ------- | -------  | ---------- | ------------------------ | -------- |
|  EAX  |	AX  |	AH  |	AL  |	mov al, 'A' ; load character  |  mov al, 'A' ; load character  |
|  EBX  |	BX  |	BH  |	BL  |	mov bl, 255 ; load byte value  |  mov bl, 255 ; load byte value  |
|  ECX  |	CX  |	CH  |	CL  |	mov cl, 8 ; shift count  |  	mov cl, 8 ; shift count  |
|  EDX  |	DX  |	DH  |	DL  |	mov dl, 10 ; digit value  |  mov dl, 10 ; digit value  |
|  ESI  |  SI  (Not a register but lower 16-bits can be accessed through it)  |
|  EDI  |  DI  (Not a register but lower 16-bits can be accessed through it)  |
|  ESP  |  SP  (Not a register but lower 16-bits can be accessed through it)  |
|  EBP  |  BP  (Not a register but lower 16-bits can be accessed through it)  |

## Segment Registers
|  Register  |	Description  |	Purpose  |	Example  |
| ---------- | ------------- | --------- | --------- |
|  CS  |	Code Segment  |	Points to code segment  |	Automatically managed  |
|  DS  |	Data Segment  |	Points to data segment  |	mov ax, ds  |
|  SS  |	Stack Segment  |	Points to stack segment  |	Automatically managed  |
|  ES  |	Extra Segment  |	Additional data segment  |	Used with string instructions  |
|  FS  |	Extra Segment  |	Additional data segment  |	mov eax, fs:[ebx]  |
|  GS  |	Extra Segment  |	Additional data segment  |	mov eax, gs:[ebx]  |

## Flags Register (EFLAGS)

|Flag  |	Bit  |	Name  |	Description  |	Set When  |	Example  |
| ---- | ----- | ------ | ------------ | ---------- | -------- |
|CF  |	0  |	Carry Flag  |	Carry/borrow occurred  |	Unsigned overflow  |	add al, 255 ; sets CF if AL > 0  |
|ZF  |	6  |	Zero Flag  |	Result is zero  |	Operation result = 0  |	cmp eax, ebx ; ZF=1 if equal  |
|SF  |	7  |	Sign Flag  |	Result is negative  |	MSB of result = 1  |	sub eax, ebx ; SF=1 if negative  |
|OF  |	11  |	Overflow Flag  |	Signed overflow   |	Signed arithmetic overflow  |	add al, 127 ; may set OF  |
|PF  |	2  |	Parity Flag  |	Even number of 1 bits  |	Even parity in low byte  |	Rarely used in high-level code  |
|AF  |	4  |	Auxiliary Flag  |	BCD carry/borrow  |	BCD arithmetic  |	Used for BCD operations  |
|IF  |	9  |	Interrupt Flag  |	Interrupts enabled  |	Hardware interrupts allowed  |	sti ; sets IF, cli ; clears IF  |
|DF  |	10  |	Direction Flag  |	String direction  |	String ops decrement  |	std ; sets DF, cld ; clears DF  |
