# Table of Contents
- [Table of Contents](#table-of-contents)
- [Registers](#registers)
  - [General Purpose Registers](#general-purpose-registers)
  - [16-bit and 8-bit Register Subdivisions](#16-bit-and-8-bit-register-subdivisions)
  - [Segment Registers](#segment-registers)
  - [Flags Register (EFLAGS)](#flags-register-eflags)
- [Instructions](#instructions)
  - [Data Movement Instructions](#data-movement-instructions)
  - [Arithmetic Instructions](#arithmetic-instructions)
  - [Logical Instructions](#logical-instructions)
  - [Shift and Rotate Instructions](#shift-and-rotate-instructions)
  - [Comparison Instructions](#comparison-instructions)
  - [Conditional Jump Instructions](#conditional-jump-instructions)
  - [Unconditional Jump and Call Instructions](#unconditional-jump-and-call-instructions)
  - [String Instructions](#string-instructions)
  - [Prefix Instructions for String Operations](#prefix-instructions-for-string-operations)
  - [Stack Instructions](#stack-instructions)
  - [Flag Control Instructions](#flag-control-instructions)

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

# Instructions
## Data Movement Instructions
|Instruction  |	Syntax  |	Description  |	Flags Affected  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|MOV  |	mov dest, src  |	Move data from source to destination  |	None  |	mov eax, 100  |
|MOVSX  |	movsx dest, src  |	Move with sign extension  |	None  |	movsx eax, bl ; extend signed byte to dword  |
|MOVZX  |	movzx dest, src  |	Move with zero extension  |	None  |	movzx eax, bl ; extend unsigned byte to dword  |
|MOVSB  |	movsb  |	Move string byte (ESI→EDI)  |	None  |	rep movsb ; copy string  |
|MOVSW  |	movsw  |	Move string word (ESI→EDI)  |	None  |	rep movsw ; copy word array  |
|MOVSD  |	movsd  |	Move string doubleword (ESI→EDI)  |	None  |	rep movsd ; copy dword array  |
|CMOV  |	cmovcc dest, src  |	Conditional move (Pentium Pro+)  |	None  |	cmove eax, ebx ; move if equal  |
|XCHG  |	xchg op1, op2  |	Exchange values of two operands  |	None  |	xchg eax, ebx  |
|LEA  |	lea reg, mem  |	Load effective address  |	None  |	lea eax, [ebx+4]  |
|LAHF  |	lahf  |	Load AH from flags (low byte)  | None  |	lahf ; AH = low 8 bits of EFLAGS  |
|SAHF  |	sahf  |	Store AH into flags (low byte)  |	SF, ZF, AF, PF, CF  |	sahf ; restore flags from AH  |
|PUSHF  |	pushf  |	Push 16-bit flags onto stack  |	None  |	pushf  |
|PUSHFD  |	pushfd  |	Push 32-bit flags onto stack  |	None  |	pushfd  |
|POPF  |	popf  |	Pop 16-bit flags from stack  |	All flags  |	popf  |
|POPFD  |	popfd  |	Pop 32-bit flags from stack  |	All flags  |	popfd  |
|PUSH  |	push src  |	Push value onto stack  |	None  |	push eax  |
|POP  |	pop dest  |	Pop value from stack  |	None  |	pop eax  |
|PUSHA  |	pusha  |	Push all 16-bit general registers  |	None  |	pusha  |
|PUSHAD  |	pushad  |	Push all 32-bit general registers  |	None  |	pushad  |
|POPA  |	popa  |	Pop all 16-bit general registers  |	None  |	popa  |
|POPAD  |	popad  |	Pop all 32-bit general registers  |	None  |	popad  |

## Arithmetic Instructions
|Instruction  |	Syntax  |	Description  |	Flags Affected  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|ADD  |	add dest, src  |	Add source to destination  |	CF, ZF, SF, OF, PF, AF  |	add eax, 10  |
|SUB  |	sub dest, src  |	Subtract source from destination  |	CF, ZF, SF, OF, PF, AF  |	sub eax, 5  |
|MUL  |	mul src  |	Unsigned multiply EAX by source  |	CF, OF (ZF, SF, PF undefined)  |	mul ebx ; result in EDX:EAX  |
|IMUL  |	imul src  |	Signed multiply  |	CF, OF (ZF, SF, PF undefined)  |	imul ebx  |
|DIV  |	div src  |	Unsigned divide EDX:EAX by source  |	All flags undefined  |	div ebx ; quotient in EAX, remainder in EDX  |
|IDIV  |	idiv src  |	Signed divide EDX:EAX by source  |	All flags undefined  |  idiv ebx  |
|INC  |	inc dest  |	Increment destination by 1  |	ZF, SF, OF, PF, AF (CF unchanged)  |	inc eax  |
|DEC  |	dec dest  |	Decrement destination by 1  |	ZF, SF, OF, PF, AF (CF unchanged)  |	dec ecx  |
|NEG  |	neg dest  |	Two's complement negation  |	CF, ZF, SF, OF, PF, AF  |	neg eax  |

## Logical Instructions
|Instruction  |	Syntax  |	Description  |	Flags Affected  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|AND  |	and dest, src  |	Bitwise AND  |	CF=0, OF=0, ZF, SF, PF  |	and eax, 0FFh  |
|OR  |	or dest, src  |	Bitwise OR  |	CF=0, OF=0, ZF, SF, PF  |	or eax, 1  |
|XOR  |	xor dest, src  |	Bitwise XOR  |	CF=0, OF=0, ZF, SF, PF  |	xor eax, eax ; clear register  |
|NOT  |	not dest  |	Bitwise NOT (complement)  |	None  |	not eax  |
|TEST  |	test op1, op2  |	Bitwise AND without storing result  |	CF=0, OF=0, ZF, SF, PF  |	test eax, eax ; check if zero  |

## Shift and Rotate Instructions
|Instruction  |	Syntax  |	Description  |	Flags Affected  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|SHL/SAL  |	shl dest, count  |	Shift left (logical/arithmetic)  |	CF, ZF, SF, PF, OF  |	shl eax, 1 ; multiply by 2  |
|SHR  |	shr dest, count  |	Shift right logical  |	CF, ZF, SF, PF, OF  |	shr eax, 2 ; divide by 4  |
|SAR  |	sar dest, count  |	Shift right arithmetic  |	CF, ZF, SF, PF, OF  |	sar eax, 1 ; signed divide by 2  |
|ROL  |	rol dest, count  |	Rotate left  |	CF, OF  |	rol eax, 4  |
|ROR  |	ror dest, count  |	Rotate right  |	CF, OF  |	ror eax, 8  |
|RCL  |	rcl dest, count  |	Rotate left through carry  |	CF, OF  |	rcl eax, 1  |
|RCR  |	rcr dest, count  |	Rotate right through carry  |	CF, OF  |	rcr eax, 1  |

## Comparison Instructions
|Instruction  |	Syntax  |	Description  |	Flags Affected  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|CMP  |	cmp op1, op2  |	Compare (subtract without storing)  |	CF, ZF, SF, OF, PF, AF  |	cmp eax, 100  |
|CMPS  |	cmps  |	Compare string elements  |	CF, ZF, SF, OF, PF, AF  |	cmpsb ; compare bytes  |

## Conditional Jump Instructions
|Instruction  |	Condition  |	Flags Checked  |	Description  |	Example  |
| ----------- | ---------- | --------------- | ------------- | --------- |
|JE/JZ  |	Equal/Zero  |	ZF = 1  |	Jump if equal or zero  |	je equal_label  |
|JNE/JNZ  |	Not Equal/Not Zero  |	ZF = 0  |	Jump if not equal or not zero  |	jne not_equal_label  |
|JG/JNLE  |	Greater (signed)  |	ZF = 0 AND SF = OF  |	Jump if greater  |	jg greater_label  |
|JL/JNGE  |	Less (signed)  |	SF ≠ OF  |	Jump if less  |	jl less_label  |
|JGE/JNL  |	Greater or Equal (signed)  |	SF = OF  |	Jump if greater or equal  |	jge ge_label  |
|JLE/JNG  |	Less or Equal (signed)  |	ZF = 1 OR SF ≠ OF  |	Jump if less or equal  |	jle le_label  |
|JA/JNBE  |	Above (unsigned)  |	CF = 0 AND ZF = 0  |	Jump if above  |	ja above_label  |
|JB/JNAE  |	Below (unsigned)  |	CF = 1  |	Jump if below  |	jb below_label  |
|JAE/JNB  |	Above or Equal (unsigned)  |	CF = 0  |	Jump if above or equal  |	jae ae_label  |
|JBE/JNA  |	Below or Equal (unsigned)  |	CF = 1 OR ZF = 1  |	Jump if below or equal  |	jbe be_label  |
|JP  |  Parity  |  PF = 1  |  Jump if parity (even) | jp label  |
|JNP  | No Parity  | PF = 0  | Jump if no parity (odd)  | jnp label  |
|JC  |	Carry  |	CF = 1  |	Jump if carry flag set  |	jc carry_label  |
|JNC  |	No Carry  |	CF = 0  |	Jump if carry flag clear  |	jnc no_carry_label  |
|JO  |	Overflow  |	OF = 1  |	Jump if overflow flag set  |	jo overflow_label  |
|JNO  |	No Overflow  |	OF = 0  |	Jump if overflow flag clear  |	jno no_overflow_label  |
|JS  |	Sign  |	SF = 1  |	Jump if sign flag set  |	js negative_label  |
|JNS  |	No Sign  |	SF = 0  |	Jump if sign flag clear  |	jns positive_label  |
|JCXZ  |  |  CX = 0  |  Jump if CX = 0  |
|JECXZ  |  |  ECX = 0  |  Jump if ECX = 0  |
|JRCXZ  |  |  RCX = 0   |  Jump if RCX = 0 (64-bit mode)  |

## Unconditional Jump and Call Instructions
|Instruction  |	Syntax  |	Description  |	Example  |
| ----------- | ------- | ------------ | --------- |
|JMP  |	jmp label  |	Unconditional jump  |	jmp loop_start  |
|CALL  |	call procedure  |	Call procedure (push return address)  |	call MyProc  |
|RET  |	ret [value]  |	Return from procedure  |	ret or ret 8  |
|LOOP  |	loop label  |	Decrement ECX and jump if not zero  |	loop loop_label  |
|LOOPE/LOOPZ  |	loope label  |	Loop while equal/zero  |	loope check_loop  |
|LOOPNE/LOOPNZ  |	loopne label  |	Loop while not equal/not zero  |	loopne scan_loop  |

## String Instructions
|Instruction  |	Syntax  |	Description  |	Registers Used  |	Example  |
| ----------- | ------- | ------------ | ---------------- | -------- |
|MOVS  |	movsb/movsw/movsd  |	Move string  |	ESI (source), EDI (dest)  |	rep movsb  |
|STOS  |	stosb/stosw/stosd  |	Store string  |	EAX (value), EDI (dest)  |	rep stosb  |
|LODS  |	lodsb/lodsw/lodsd  |	Load string  |	ESI (source), EAX (loaded)  |	lodsb  |
|SCAS  |	scasb/scasw/scasd  |	Scan string  |	EAX (value), EDI (scan)  |	repne scasb  |
|CMPS  |	cmpsb/cmpsw/cmpsd  |	Compare strings  |	ESI (src1), EDI (src2)  |	repe cmpsb  |

## Prefix Instructions for String Operations
|Prefix  |	Description  |	Usage  |	Example  |
| ------ | ------------- | ------- | --------- |
|REP  |	Repeat while ECX ≠ 0  |	With MOVS, STOS, LODS  |	rep movsb  |
|REPE/REPZ  |	Repeat while equal/zero  |	With CMPS, SCAS  |	repe cmpsb  |
|REPNE/REPNZ  |	Repeat while not equal/not zero  |	With CMPS, SCAS  |	repne scasb  |

## Stack Instructions
|Instruction  |	Syntax  |	Description  |	Stack Effect  |	Example  |
| ----------- | ------- | ------------ | -------------- | -------- |
|PUSH  |	push src  |	Push onto stack  |	ESP -= 4  |	push eax  |
|POP  |	pop dest  |	Pop from stack  |	ESP += 4  |	pop ebx  |
|PUSHAD  |	pushad  |	Push all general registers  |	ESP -= 32  |	pushad  |
|POPAD  |	popad  |	Pop all general registers  |	ESP += 32  |	popad  |
|PUSHFD  |	pushfd  |	Push flags register  |	ESP -= 4  |	pushfd  |
|POPFD  |	popfd  |	Pop flags register  |	ESP += 4  |	popfd  |

## Flag Control Instructions
|Instruction  |	Description  |	Flag Effect  |	Example  |
| ----------- | ------------ | ------------- | --------- |
|CLC  |	Clear carry flag  |	CF = 0  |	clc  |
|STC  |	Set carry flag  |	CF = 1  |	stc  |
|CLD  |	Clear direction flag  |	DF = 0  |	cld  |
|STD  |	Set direction flag  |	DF = 1  |	std  |
|CLI  |	Clear interrupt flag  |	IF = 0  |	cli  |
|STI  |	Set interrupt flag  |	IF = 1  |	sti  |
