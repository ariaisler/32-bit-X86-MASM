# Table of Contents
  - [Table of Contents](#table-of-contents)
  - [Registers](#registers)
  -   [General Purpose Registers](#general-purpose-registers)





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
