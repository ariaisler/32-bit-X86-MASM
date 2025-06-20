# Table of Contents
- [Table of Contents](#table-of-contents)
- [Registers](#registers)
  - [General Purpose Registers](#general-purpose-registers)
  - [16-bit and 8-bit Register Subdivisions](#16-bit-and-8-bit-register-subdivisions)
  - [Segment Registers](#segment-registers)
  - [Flags Register (EFLAGS)](#flags-register-eflags)
- [Data Types and Sizes](#data-types-and-sizes)
  - [Integer Data Types](#integer-data-types)
  - [Floating-Point Data Types](#floating-point-data-types)
  - [Legacy Floating-Point Synonyms](#legacy-floating-point-synonyms)
  - [String and Character Data Types](#string-and-character-data-types)
  - [Pointer Data Types](#pointer-data-types)
  - [Array and Structure Data Types](#array-and-structure-data-types)
  - [Pointer Data Types](#pointer-data-types)
  - [Structure Example](#structure-example)
  - [Union Example](#union-example)
  - [Binary-Coded Decimal (BCD) Data Types](#binary-coded-decimal-bcd-data-types)
  - [Special Data Types](#special-data-types)
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
  - [Addressing Modes](#addressing-modes)
- [Directives](#directives)
  - [Basic Structure Directives](#basic-structure-directives)
  - [Data Definition and Memory Directives](#data-definition-and-memory-directives)
  - [Conditional Control Flow Directives (High-Level)](#conditional-control-flow-directives-high-level)


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

# Data Types and Sizes
## Integer Data Types
|Data Type  |	Size  |	MASM Directive  |	Signed Range  |  Unsigned Range  |	Example Declaration  |
| --------- | ----- | --------------- | ------------- | ---------------- | --------------------- |
|BYTE  |	8 bits  |	BYTE  |	-128 to 127  |	0 to 255  |	myByte BYTE 100  |
|SBYTE  |	8 bits  |	SBYTE  |	-128 to 127  |	N/A  |	mySignedByte SBYTE -50  |
|WORD  |	16 bits  |	WORD  |	-32,768 to 32,767  |	0 to 65,535  |	myWord WORD 1000  |
|SWORD  |	16 bits  |	SWORD  |	-32,768 to 32,767  |	N/A  |	mySignedWord SWORD -1000  |
|DWORD  |	32 bits  |	DWORD  |	-2,147,483,648 to 2,147,483,647  |	0 to 4,294,967,295  |	myDword DWORD 100000  |
|SDWORD  |	32 bits  |	SDWORD  |	-2,147,483,648 to 2,147,483,647  |	N/A  |	mySignedDword SDWORD -100000  |
|QWORD  |	64 bits  |	QWORD  |	-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807  |	0 to 18,446,744,073,709,551,615  |	myQword QWORD 123456789012345  |
|SQWORD  |	64 bits  |	SQWORD  |	-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807  |	N/A  | mySignedQword SQWORD -123456789  |

## Floating-Point Data Types
|Data Type  |	Size  |	MASM Directive  |	Precision  |	Range (Approximate)  |	Example Declaration  |
| --------- | ----- | --------------- | ---------- | --------------------- | --------------------- |
|REAL4  |	32 bits  |	REAL4  |	Single precision  |	±1.2 × 10^-38 to ±3.4 × 10^38  |	myFloat REAL4 3.14159  |
|REAL8  |	64 bits  |	REAL8  |	Double precision  |	±2.3 × 10^-308 to ±1.7 × 10^308  |	myDouble REAL8 2.718281828  |
|REAL10  |	80 bits  |	REAL10  |	Extended precision  |	±3.4 × 10^-4932 to ±1.2 × 10^4932  |	myExtended REAL10 1.23456789012345  |

## Legacy Floating-Point Synonyms
|Legacy Name  |	Modern Equivalent  |	Size  |	Description  |
| ----------- | ------------------ | ------ | ------------ |
|DD  |	DWORD  |	32 bits  |	Define Doubleword  |
|DQ  |	QWORD  |	64 bits  |	Define Quadword  |
|DT  |	REAL10  |	80 bits  |	Define Ten-byte  |

## String and Character Data Types
|Data Type  |	Size  |	MASM Directive  |	Description  |	Example Declaration  |
| --------- | ----- | --------------- | ------------ | --------------------- |
|String (null-terminated)  |	Variable  |	BYTE  |	C-style string with null terminator  |	myString BYTE "Hello", 0  |
|String (counted)  |	Variable  |	BYTE  |	Pascal-style with length prefix  |	myPString BYTE 5, "Hello"  |
|Character  |	8 bits  |	BYTE  |	Single ASCII character  |	myChar BYTE 'A'  |
|Unicode Character  |	16 bits  |	WORD  |	Single Unicode character  |	myUniChar WORD 041h  |

## Pointer Data Types
|Data Type  |	Size  |	MASM Directive  |	Description  |	Example Declaration  |
| --------- | ----- | --------------- | ------------ | --------------------- |
|NEAR PTR  |	32 bits  |	DWORD  |	Near pointer (32-bit offset)  |	myPtr DWORD OFFSET myVar  |
|FAR PTR  |	48 bits  |	6 bytes  |	Far pointer (segment:offset)  |	myFarPtr DF ?  |
|PROC PTR  |	32 bits  |	DWORD  |	Procedure pointer  |	myProcPtr PROC PTR ?  |

## Array and Structure Data Types
|Data Type  |  Size  |	MASM Directive  |	Description  |	Example Declaration  |
| --------- | ------ | ---------------- | ------------ | --------------------- |
|Array  |	Variable  |	Type DUP(?)  |	Array of specified type  |	myArray DWORD 10 DUP(?)  |
|Initialized Array  |	Variable  |	Type values	Array with initial values  |	myArray DWORD 1,2,3,4,5  |
|Structure  |	Variable  |	STRUCT/ENDS	User-defined structure  |	See structure example below  |
|Union  |	Variable  |	UNION/ENDS	Overlapping data fields  |	See union example below  |

## Structure Example
```
PERSON STRUCT
    firstName   BYTE 20 DUP(?)      ; 20-character first name
    lastName    BYTE 20 DUP(?)      ; 20-character last name
    age         WORD ?              ; Age in years
    salary      DWORD ?             ; Annual salary
PERSON ENDS

employee1   PERSON <>               ; Declare structure instance
employee2   PERSON {"John", "Doe", 25, 50000}  ; Initialize structure
```

## Union Example
```
NUMBER_UNION UNION
    wholePart   DWORD ?             ; 32-bit integer view
    parts       STRUCT              ; Byte-level view
        b0      BYTE ?              ; Lowest byte
        b1      BYTE ?              ; Second byte
        b2      BYTE ?              ; Third byte
        b3      BYTE ?              ; Highest byte
    parts       ENDS
NUMBER_UNION ENDS

myNumber    NUMBER_UNION <>         ; Declare union instance
```

## Binary-Coded Decimal (BCD) Data Types
|Data Type  |	Size  |	MASM Directive  |	Description  |	Example Declaration  |
| --------- | ----- | --------------- | ------------ | --------------------- |
|TBYTE  |	80 bits  |	TBYTE  |	Ten-byte BCD number  |	myBCD TBYTE 123456789012345678  |
|Packed BCD  |	Variable  |	BYTE  |	Two BCD digits per byte  |	myPackedBCD BYTE 12h, 34h  |
|Unpacked BCD  |	Variable  |	BYTE  |	One BCD digit per byte  |	myUnpackedBCD BYTE 1, 2, 3, 4  |

## Special Data Types
|Data Type  |	Size  |	MASM Directive  |	Description  |	Example Declaration  |
| --------- | ----- | --------------- | ------------ | --------------------- |
|FWORD  |	48 bits  |	FWORD  |	Far pointer (16:32)  |	myFarPtr FWORD ?  |
|PWORD  |	48 bits  |	PWORD  |	Pseudo-descriptor  |	myPseudo PWORD ?  |
|MMWORD  |	64 bits  |	MMWORD  |	MMX register data  |	myMMX MMWORD ?  |
|XMMWORD  |	128 bits  |	XMMWORD  |	XMM register data  |	myXMM XMMWORD ?  |
|YMMWORD  |	256 bits  |	YMMWORD  |	YMM register data  |	myYMM YMMWORD ?  |


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

## Addressing Modes
|Mode  |	Syntax  |	Description  |	Example  |
| ---- | -------- | ------------ | --------- |
|Immediate  |	mov eax, 100  |	Constant value  |	mov eax, 42  |
|Register  |	mov eax, ebx  |	Register to register  |	mov eax, ebx  |
|Direct  |	mov eax, [var]  |	Memory variable  |	mov eax, [myVar]  |
|Register Indirect  |	mov eax, [ebx]  |	Memory via register  |	mov eax, [ebx]  |
|Displacement  |	mov eax, [ebx+4]  |	Register + offset  |	mov eax, [ebx+8]  |
|Base + Index  |	mov eax, [ebx+ecx]  |	Base + index register  |	mov eax, [ebx+ecx*2]  |
|Base + Index + Displacement  |	mov eax, [ebx+ecx+4]  |	Complex addressing  |	mov eax, [ebx+ecx*4+8]  |

# Directives
## Basic Structure Directives
|Directive  |	Purpose  |	Example  |
| --------- | -------- | --------- |
|.data  |	Data section  |	.data  |
|.code  |	Code section  |	.code
|.const  |	Constant data section  |	.const
|.data?  |	Uninitialized data section  |	.data?  |
|.stack  |	Stack section  |	.stack 4096  |
|.model  |	Memory model  |	.model flat, stdcall  |
|PROC  |	Start procedure  |	MyProc PROC  |
|ENDP  |	End procedure  |	MyProc ENDP  |
|END  |	End of program  |	END main  |

## Data Definition and Memory Directives
|Directive  |	Purpose  |	Example  |
| --------- | -------- | --------- |
|OFFSET  |	Get address  |	mov eax, OFFSET myVar  |
|SIZEOF  |	Get size in bytes  |	mov ecx, SIZEOF myArray  |
|LENGTHOF  |	Get number of elements  |	mov ecx, LENGTHOF myArray  |
|TYPE  |	Get size of data type  |	mov eax, TYPE DWORD  |
|DUP  |	Duplicate values  |	myArray DWORD 5 DUP(0)  |
|EQU  |	Define constant  |	MAX_SIZE EQU 100  |
|=  |	Redefinable constant  |	counter = 0  |
|LABEL  |	Create label with type  |	myLabel LABEL DWORD  |
|ORG  |	Set location counter  |	ORG 100h  |
|ALIGN  |	Align data  |	ALIGN 4  |

## Conditional Control Flow Directives (High-Level)
|Directive  |	Purpose  |	Syntax  |	Example  |
| --------- | -------- | -------- | -------- |
|.IF  |	Conditional execution  |	.IF condition  |	.IF eax > 10  |
|.ELSEIF  |	Alternative condition  |	.ELSEIF condition  |	.ELSEIF eax == 5  |
|.ELSE  |	Default case  |	.ELSE  |	.ELSE  |
|.ENDIF  |	End conditional block  |	.ENDIF  |	.ENDIF  |
|.WHILE  |	While loop  |	.WHILE condition  |	.WHILE eax < 100  |
|.ENDW  |	End while loop  |	.ENDW  |	.ENDW  |
|.REPEAT  |	Repeat-until loop  |	.REPEAT  |	.REPEAT  |
|.UNTIL  |	End repeat loop  |	.UNTIL condition  |	.UNTIL eax == 0  |
|.UNTILCXZ  |	Repeat until CX is zero  |	.UNTILCXZ  |	.UNTILCXZ  |
|.FOR  |	For loop (MASM 6.0+)  |	.FOR reg:=start TO end  |	.FOR eax:=1 TO 10  |
|.ENDFOR  |	End for loop  |	.ENDFOR  |	.ENDFOR  |
|.BREAK  |	Break from loop  |	.BREAK .IF condition  |	.BREAK .IF eax > 50  |
|.CONTINUE  |	Continue loop  |	.CONTINUE .IF condition  |	.CONTINUE .IF eax == 0  |
