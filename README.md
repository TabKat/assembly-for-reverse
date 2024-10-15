# Assembly 64 for reverse

<table>
<thead>
<tr>
<th>64-bit register</th>
<th>Lower 32 bits</th>
<th>Lower 16 bits</th>
<th>Lower 8 bits</th>
</tr>
</thead>
<tbody>
<tr>
<td>rax</td>
<td>eax</td>
<td>ax</td>
<td>al</td>
</tr>
<tr>
<td>rbx</td>
<td>ebx</td>
<td>bx</td>
<td>bl</td>
</tr>
<tr>
<td>rcx</td>
<td>ecx</td>
<td>cx</td>
<td>cl</td>
</tr>
<tr>
<td>rdx</td>
<td>edx</td>
<td>dx</td>
<td>dl</td>
</tr>
<tr>
<td>rsi</td>
<td>esi</td>
<td>si</td>
<td>sil</td>
</tr>
<tr>
<td>rdi</td>
<td>edi</td>
<td>di</td>
<td>dil</td>
</tr>
<tr>
<td>rbp</td>
<td>ebp</td>
<td>bp</td>
<td>bpl</td>
</tr>
<tr>
<td>rsp</td>
<td>esp</td>
<td>sp</td>
<td>spl</td>
</tr>
<tr>
<td><strong>r8</strong></td>
<td>r8d</td>
<td>r8w</td>
<td>r8b (r8l)</td>
</tr>
<tr>
<td><strong>r9</strong></td>
<td>r9d</td>
<td>r9w</td>
<td>r9b (r9l)</td>
</tr>
<tr>
<td><strong>r10</strong></td>
<td>r10d</td>
<td>r10w</td>
<td>r10b (r10l)</td>
</tr>
<tr>
<td><strong>r11</strong></td>
<td>r11d</td>
<td>r11w</td>
<td>r11b (r11l)</td>
</tr>
<tr>
<td><strong>r12</strong></td>
<td>r12d</td>
<td>r12w</td>
<td>r12b (r12l)</td>
</tr>
<tr>
<td><strong>r13</strong></td>
<td>r13d</td>
<td>r13w</td>
<td>r13b (r13l)</td>
</tr>
<tr>
<td><strong>r14</strong></td>
<td>r14d</td>
<td>r14w</td>
<td>r14b (r14l)</td>
</tr>
<tr>
<td><strong>r15</strong></td>
<td>r15d</td>
<td>r15w</td>
<td>r15b (r15l)</td>
</tr>
<tr>
<td><em>r16 (with APX)</em></td>
<td>r16d</td>
<td>r16w</td>
<td>r16b (r16l)</td>
</tr>
<tr>
<td><em>r17 (with APX)</em></td>
<td>r17d</td>
<td>r17w</td>
<td>r17b (r17l)</td>
</tr>
<tr>
<td>...</td>
<td>...</td>
<td>...</td>
<td>...</td>
</tr>
<tr>
<td><em>r31 (with APX)</em></td>
<td>r31d</td>
<td>r31w</td>
<td>r31b (r31l)</td>
</tr>
</tbody>
</table>


## Accessing memory
**.data** - initialized data segment \
**.bss** - uninitialized data segment (all data in **.bss** after program reload will be erased) 

Record data in the **.data** segment

```asm
mov rcx, qword ptr ds:[0x00000000004030E0]
mov qword ptr ds:[0x00000000004030F0], rcx
```

## ADD instruction

To write zero in register

```asm
xor rax, rax
```

```asm
add rbx, 0x
```

To overcome big number limitation write in register and use it's address.

## Partial **MOV** instruction

To write data in Lower 8 bits in register add **b**

```asm
mov r8b, 0x1
```

Add **-1** (0xFFFFFFFF) to **r8**
```asm
add rax, -1
```

## PUSH and POP

Take data from top fo the stack and write in a register

```asm
pop rbx
```
If the top of a stack is 0x00000001 then rbx will be 0x0000001

## MOV instruction

Fore types of **move**

1) mov register, value
2) mov memory, value
3) mov register, memory
4) mov memory, register

## XCHG instruction

Swap registers value
Let's say that **rax** is **3** and **rbx** is **5**

```asm
xchg rbx, rax
```

After this command **rax** will **5** and **rbx** **3**

Same with memory address (xchg memory address, register)

Illegal to change addresses directly, for example:

```asm
xchg [0x0000000000403120], [0x0000000000403130]
```
This operation is not permitted. Correct way is to save first memory
address in a register and cope value from there

```asm
mov rax, mem1
xchg rax, rax
mov mem1, rax
```

## INC, DEC, NEG, ADD, SUB
**INC** **DEC** **ADD** and **SUB** can work with registers as well as memory

**NEG**. Let's say **rax** is FFFFFFFFFFFFFFFF
```asm
neg rax
```
Result will be 0000000000000001 \
NEG for 0 is 0 \
NEG works with memory too

## Registers flags

Carry Flag (CF), Overflow Flag (OF), Sign Flag (SF), and Zero Flag (ZF) in x64 assembly programming:

Carry Flag (CF): This flag is primarily used in arithmetic operations. It is set when there is an overflow out of the most significant bit for unsigned operations, indicating that the result was too large to fit in the register. For example, in an addition operation, if the sum of two numbers exceeds the maximum value that can be stored in the destination operand, the CF is set. It's also used in multi-precision arithmetic operations.

Overflow Flag (OF): The OF is set when a signed arithmetic operation results in a value too large (or too small) to be represented in the destination operand. This occurs when there's an overflow or underflow out of the range of representable values for signed numbers. For instance, adding two large positive signed integers might result in a negative value if an overflow occurs, in which case the OF would be set.

Sign Flag (SF): This flag indicates the sign of the result of an arithmetic or logical operation. It is set if the result is negative (i.e., the most significant bit of the result is 1 in two's complement representation). The SF is often used in conjunction with the OF to determine overflow in subtraction and comparison operations.

Zero Flag (ZF): The ZF is set if the result of an arithmetic or logical operation is zero. This flag is often used for equality checks in conditional branch instructions. For example, after a subtract operation, if the ZF is set, it indicates that the two operands were equal.

These flags are part of the processor's status register and are automatically set or cleared by the CPU based on the results of various operations. They play a crucial role in decision-making and controlling the flow of a program.


The **carry flag (CF)** is set when the result of **an unsigned** arithmetic operation is **too large** to fit into the destination. The **overflow flag (OF)** is set when the result of **a signed** arithmetic operation is **too large** or **too small** to fit into the destination.

## Bitwise operations

To write binary in registers use **0b** for example 0xb0001

### Bitwise AND &


<table>
<tbody><tr>
<th>bit a</th>
<th>bit b</th>
<th><code>a &amp; b</code> (a AND b)
</th></tr>
<tr>
<td>0</td>
<td>0</td>
<td>0
</td></tr>
<tr>
<td>0</td>
<td>1</td>
<td>0
</td></tr>
<tr>
<td>1</td>
<td>0</td>
<td>0
</td></tr>
<tr>
<td>1</td>
<td>1</td>
<td>1
</td></tr></tbody>
<table>


### Bitwise NOT !

<table>
<tbody>
<tr>
    <th>bit a</th>
    <th>NOT a</th>
<tr>
<td>0</td>
<td>1</td>
<tr>
<td>0</td>
<td>1</td></tr>
<tr>
<td>1</td>
<td>0</td></tr>
<tr>
<td>1</td>
<td>0</td></tr></tbody>
</table>


### Bitwise OR |

<table>
<tbody><tr>
<th>bit a</th>
<th>bit b</th>
<th>a | b (a OR b)
</th></tr>
<tr>
<td>0</td>
<td>0</td>
<td>0
</td></tr>
<tr>
<td>0</td>
<td>1</td>
<td>1
</td></tr>
<tr>
<td>1</td>
<td>0</td>
<td>1
</td></tr>
<tr>
<td>1</td>
<td>1</td>
<td>1
</td></tr></tbody>
</table>


### Bitwise XOR ^

<table>
<tbody><tr>
<th>bit a</th>
<th>bit b</th>
<th><code>a ^ b</code> (a XOR b)
</th></tr>
<tr>
<td>0</td>
<td>0</td>
<td>0
</td></tr>
<tr>
<td>0</td>
<td>1</td>
<td>1
</td></tr>
<tr>
<td>1</td>
<td>0</td>
<td>1
</td></tr>
<tr>
<td>1</td>
<td>1</td>
<td>0
</td></tr></tbody></table>

Example how to use bitwise operations:

```asm
mov rax, 0b0011
mov rbx, 0b0001
and rax, rbx
```
result is **1**

Another example

```asm
xor rax, rax
```

Always is **0**

### JUMP instruction

This is how obfuscator works

```asm
jmp qword ptr ds:[0x0000000000407070]
mov rbx, 3
```

Address of command **mov rbx, 3** is **0000000000401570**
So jump on memory address where that contains address of our command.

Important!
This technic did not work in **.data** segment. It works in **.bss** segment

**TEST** does the same as **AND** , but the result of the **AND** operation is discarded; just the appropriate flags are set. \
And it's the same with **CMP** vs **SUB** - **CMP** doesn't save the result of the subtraction, it just sets the appropriate flags (e.g. zero, carry)

**TEST** instruction uses **AND** logic
**CMP** instruction uses **SUB** instruction

JZ = Jump if Zero \
JE = JUmp if Equal \
JNZ = Jump if Not Zero \
JNE = Jump if Not Equal \

JC = Jump if Carry \
JB = Jump if Below \
JNC = Jump if No Carry \
JAE = Jump if Above or Equal




