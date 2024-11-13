# ASIC-Design
# Divyansh Singhal (IMT2021522)
<details>
<summary><strong>Lab 1:</strong> Create a C program to find the sum of `n` natural numbers, compile it using the GCC compiler, and verify the output after execution and after that also using RISC-V compiler.</summary>

# Lab-1
## Compiling the C code in GCC. Calculate sum of numbers from 1 to 10

1. Go the home directory using cd and then write the code for calulation sum of first 10 natural numbers.
 <img width="1440" alt="Screenshot 2024-08-07 at 7 15 59 PM" src="https://github.com/user-attachments/assets/b87cbda7-5bf9-4223-8053-b73ce5519f65">
 
2. Wrote the code for the same.
<img width="1440" alt="Screenshot 2024-08-07 at 7 21 13 PM" src="https://github.com/user-attachments/assets/b1114600-c686-4a9d-9137-f89eb2f52b49">

3. Used gcc and ./a.out to get the output as shown.
<img width="1440" alt="Screenshot 2024-08-07 at 7 22 40 PM" src="https://github.com/user-attachments/assets/5ee93190-aa00-4ca7-bec6-2851465fc895">


## Compiling the same C code in RISC V compiler
1. Now we will compile it using RISC-V compiler. It generates a file sum1toN.o
   
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64-march=rv64i -o sum1toN.o  sum1toN.c
```

```
ls -ltr sum1toN.o
```
   <img width="1440" alt="Screenshot 2024-08-07 at 7 28 27 PM" src="https://github.com/user-attachments/assets/f60df2fe-f66f-4625-925f-1a60843c4adc">

- `riscv64-unknown-elf-gcc`: The compiler for RISC-V 64-bit target.
- `O1`: Applies moderate optimizations for a good balance between performance and compilation time.
- `mabi=lp64`: Specifies the ABI (Application Binary Interface) as LP64, meaning "Long and Pointer are 64-bit."
- `march=rv64i`: Sets the architecture to RISC-V 64-bit with the RV64I instruction set.
- `o sum1toN.o`: Outputs the compiled code to a file named sum1ton.o.
- `sum1toN.c`: The source file to be compiled.

2. Now we open it using objdump and then | less to see the <main> segment of the code.
```
riscv64-unknown-elf-objdump -d sum1toN.o
```
```
riscv64-unknown-elf-objdump -d sum1toN.o | less
```

<img width="1440" alt="Screenshot 2024-08-08 at 1 19 20 AM" src="https://github.com/user-attachments/assets/e89c54fa-bb63-4afb-820e-f4edd5e67a8c">

<img width="1440" alt="Screenshot 2024-08-08 at 1 19 44 AM" src="https://github.com/user-attachments/assets/2b1e4053-63c8-4531-ba83-ae80d8d1f412">


3. Now calucation the number os instructions a shown. We got 15 instructions also shown in calculator.

```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64-march=rv64i -o sum1toN.o  sum1toN.c
```
<img width="1440" alt="Screenshot 2024-08-08 at 1 03 00 AM" src="https://github.com/user-attachments/assets/529c6e56-2324-486f-827d-1c296fbe8228">

4. Now we will run using Ofast instead of O1 and check the number of instructions (reduced to 12).
   <img width="1440" alt="Screenshot 2024-08-07 at 11 41 58 PM" src="https://github.com/user-attachments/assets/67983ca8-f045-4fef-b99d-eaf2cafb6913">
   <img width="1440" alt="Screenshot 2024-08-08 at 1 04 02 AM" src="https://github.com/user-attachments/assets/50f518ac-c120-473c-b360-8dcb99773811">

   
- **O1**: Provides moderate optimizations, balancing performance and compilation time, and adheres strictly to standards.
- **Ofast**: Applies aggressive optimizations for maximum performance, but might break some programs as it may not follow all standards.
</details>

<details>
<summary><strong>Lab 2:</strong>To debug the assembly code of your compiled C program using the Spike simulator</summary>

# Lab-2
## Debugging the code in Spike on RISC V

1. Run the command
 ```
spike pk sum1toN.o
```
<img width="1440" alt="Screenshot 2024-08-08 at 1 15 12 AM" src="https://github.com/user-attachments/assets/a2e99939-3135-48d2-a224-8c61826a7c05">


- `spike`: The Spike RISC-V simulator.
- `d`: Starts the simulator in debug mode.
- `pk`: Proxy kernel, a small environment that provides minimal OS services.
- `sum1toN.o`: The compiled object file of your C program.

2. Check using the following commands in the spike debugger


 ```
spike -d pk sum1toN.o
```
```
until pc 0 100b0
```
To check a registers Value type the following command 
```
reg 0 a2
```
To check stack pointer's Value type the following command 
```
reg 0 sp
```

<img width="1440" alt="Screenshot 2024-08-08 at 1 12 11 AM" src="https://github.com/user-attachments/assets/fa264e40-87a9-4845-8a26-3a03f7e03994">
<img width="1440" alt="Screenshot 2024-08-08 at 1 14 39 AM" src="https://github.com/user-attachments/assets/3f85c504-7630-4d59-8e87-113016fc4d33">

- In the assembly code, the stack pointer's value is being decreased by `0x10` in hexadecimal notation. 10->16 in Hexa decimal. 
</details>


<details>
<summary><strong>Lab 3:</strong> Identify various RISC-V instruction type (R, I, S, B, U, J) and extract 32-bit instruction code in the instruction type format for below RISC-V instructions.</summary>

# Lab-3
## RISC-V instruction type (R, I, S, B, U, J) and extract 32-bit instruction code

### RISC-V Instruction Formats

Instruction formats in RISC-V act as a 'contract' between the assembly language and the hardware. When an assembly instruction is executed, the hardware understands exactly what to do based on this contract. Each instruction type has a specific format, defined by a series of 0s and 1s, that includes details such as the type of operation and the location of data.
There are 6 types of instructions.
<img width="661" alt="Screenshot 2024-08-10 at 7 35 56 PM" src="https://github.com/user-attachments/assets/8357de63-b6a2-43e6-b74d-04e8cf306e0c">

Now before jumping into each format directly, lets take a look of subfields which are going to be there in the instructions. I'll be explaining them here. In case any new instruction is there, it will be explained in that format.  

**1.opcode**   
Reffered to as opearation code. It is of 7 bit length and it specifies what the instruction does, such as arithmetic operations, logical operations, memory operations, or control flow operations. For example, opcode 0110011 specifies R format.  

**2.rd**   
It stands for destination register. It is a field in the instruction format that specifies which register will receive the result of an operation.   

**3.rs1 and rs2**    
They stand for source register 1 and source register 2, respectively. They are fields in the instruction format that specify which registers contain the operands or data used by an instruction.

**4.func3 and func7**  
func3 provides additional details about the specific operation within the opcode category and func7 basically complements it by providing additional details of same information.

**5.Immediate**  
It is a constant value that is part of an instruction. It is directly encoded within the instruction's binary representation, typically following the opcode and other necessary fields. In different formats, they occupies different bits, which will be explained furthur in the formats.  


### Given Instructions:
1. ADD r8, r9, r10 <br>
2. SUB r10, r8, r9  <br>
3. AND r9, r8, r10 <br>
4. OR r8, r9, r5   <br>
5. .XOR r8, r8, r4    <br>
6. SLT r00, r1, r4  <br>
7. ADDI r02, r2, 5  <br>
8. SW r2, r0, 4      <br>
9. SRL r06, r01, r1  <br>
10. BNE r0, r0, 20   <br>
11. BEQ r0, r0, 15    <br>
12. LW r03, r01, 2    <br>
13. SLL r05, r01, r1  <br>


The RISC-V ISA , Hardcoded ISA and Instruction format of the given instructions.

| S.no.| Operation         | RISC-V ISA      | Hardcoded ISA   | Instruction Format |
|-----|-------------------|-----------------|-----------------|---------------------|
|1.| ADD r8, r9, r10   | 32’h00A482B3    | 32'h02208300    | R-type              |
|2.| SUB r10, r8, r9   | 32’h409482B3    | 32'h02209380    | R-type              |
|3.| AND r9, r8, r10   | 32’h00A4C2B3    | 32'h0230A400    | R-type              |
|4.| OR r8, r9, r5     | 32’h005482B3    | 32'h02513480    | R-type              |
|5.| XOR r8, r8, r4    | 32’h004482B3    | 32'h0240C500    | R-type              |
|6.| SLT r00, r1, r4   | 32’h004002B3    | 32'h02415580    | R-type              |
|7.| ADDI r02, r2, 5   | 32’h00510113    | 32'h00520600    | I-type              |
|8.| SW r2, r0, 4      | 32’h00412023    | 32'h00209181    | S-type              |
|9.| SRL r06, r01, r1  | 32’h00119533    | 32'h00271803    | R-type              |
|10.| BNE r0, r0, 20    | 32’h01400063    | 32'h01409002    | B-type              |
|11.| BEQ r0, r0, 15    | 32’h00F00063    | 32'h00F00002    | B-type              |
|12.| LW r03, r01, 2    | 32’h00210183    | 32'h00208681    | I-type              |
|13.| SLL r05, r01, r1  | 32’h00109533    | 32’h00208783    | R-type              |


Examples how instructions are decoded: **One of each type is explained** <br>
```
1) ADD r8, r9, r10
  - Opcode: 0110011
  - rd = r8 = 01000
  - rs1 = r9 = 01001
  - rs2 = r10 = 01010
  - funct3: 000
  - funct7: 0000000
  - R-Type
  - 32-bit Instruction: `0000000_01010_01001_000_01000_0110011`
```

```

7) ADDI r2, r2, 5
  - Opcode: 0010011
  - rd = r2 = 00010
  - rs1 = r2 = 00010
  - immediate: 000000000101
  - funct3: 000
  - I-Type
  - 32-bit Instruction: `000000000101_00010_000_00010_0010011`
```
```

8) SW r2, r0, 4
  - Opcode: 0100011
  - rs2 = r2 = 00010
  - rs1 = r0 = 00000
  - imm[11:5]: 0000000
  - imm[4:0]: 00100
  - funct3: 010
  - S-Type
  - 32-bit Instruction: `0000000_00010_00000_010_00100_0100011`

```

```

10) BNE r0, r0, 20
  - Opcode: 1100011
  - rs1 = r0 = 00000
  - rs2 = r0 = 00000
  - imm[12|10:5|4:1|11]: 0000000 00000 00010 0
  - funct3: 001
  - B-Type
  - 32-bit Instruction: `0000000_00000_00000_001_00010_0000000_1100011`

```

### RISC-V Functional Simulation
1. Cloning Repository and runnning the iiitb_rv32i.v code and generating .vcd file and opening it using gtkwave using the following commands.
```
git clone https://github.com/vinayrayapati/iiitb_rv32i
cd iiitb_rv32i
```

```
iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
./iiitb_rv32i
```

```
gtkwave iiitb_rv32i.vcd
```
<img width="1440" alt="Screenshot 2024-08-10 at 8 50 17 PM" src="https://github.com/user-attachments/assets/b0999c59-30c7-4953-8dd5-95e5f12fe711">



<img width="1440" alt="Screenshot 2024-08-10 at 8 55 24 PM" src="https://github.com/user-attachments/assets/8f257896-b956-4be0-9ee8-2dca63a72751">


<img width="1440" alt="Screenshot 2024-08-10 at 8 55 51 PM" src="https://github.com/user-attachments/assets/8552b494-344c-4288-b929-242c60b1168d">

### The output waveform:

The output waveform showing the instructions performed in a 5-stage pipelined architecture.

|S. no.| Operation          | Standard RISCV ISA | Hardcoded ISA |
|-----|--------------------|---------------------|---------------|
|1.| ADD R6, R2, R1     | 32'h00110333        | 32'h02208300  |
|2.| SUB R7, R1, R2     | 32'h402083b3        | 32'h02209380  |
|3.| AND R8, R1, R3     | 32'h0030f433        | 32'h0230a400  |
|4.| OR R9, R2, R5      | 32'h005164b3        | 32'h02513480  |
|5.| XOR R10, R1, R4    | 32'h0040c533        | 32'h0240c500  |
|6.| SLT R1, R2, R4     | 32'h0045a0b3        | 32'h02415580  |
|7.| ADDI R12, R4, 5    | 32'h004120b3        | 32'h00520600  |
|8.| BEQ R0, R0, 15     | 32'h00000f63        | 32'h00f00002  |
|9.| SW R3, R1, 2       | 32'h0030a123        | 32'h00209181  |
|10.| LW R13, R1, 2      | 32'h0020a683        | 32'h00208681  |
|11.| SRL R16, R14, R2   | 32'h0030a123        | 32'h00271803  |
|12.| SLL R15, R1, R2    | 32'h002097b3        | 32'h00208783  |


Output of the instructions:

1. ADD R6, R2, R1    <br>

<img width="1440" alt="Screenshot 2024-08-11 at 12 35 00 AM" src="https://github.com/user-attachments/assets/3a471dd7-e3b4-484b-9e67-db190086ee6b">





2. SUB R7, R1, R2     <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 50 24 AM" src="https://github.com/user-attachments/assets/111ebf0c-d928-4bfa-ac16-297f98b94c9d">


3. AND R8, R1, R3    <br>

<img width="1440" alt="Screenshot 2024-08-11 at 12 51 12 AM" src="https://github.com/user-attachments/assets/6fe1bf5d-b819-409a-9334-72d6ed16970e">

4. OR R9, R2, R5      <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 51 29 AM" src="https://github.com/user-attachments/assets/e0ba51c0-427d-46ed-8187-37170d579336">

   
5. XOR R10, R1, R4   <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 54 20 AM" src="https://github.com/user-attachments/assets/7c82fca6-7cfa-4994-8817-14bcee2e645f">


6. SLT R1, R2, R4     <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 54 44 AM" src="https://github.com/user-attachments/assets/7430321a-0f1c-43c3-bbfe-25e125d8d06f">


7. ADDI R12, R4, 5   <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 55 02 AM" src="https://github.com/user-attachments/assets/2995c5ce-8ea6-4810-89d1-c05b3a7c4c03">



8. BEQ R0, R0, 15     <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 55 32 AM" src="https://github.com/user-attachments/assets/c767a9a1-6830-453b-9b56-46489f48c5d6">


9. SW R3, R1, 2      <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 56 29 AM" src="https://github.com/user-attachments/assets/bad3ac36-ce09-4346-97ec-00d4ff519126">



10. LW R13, R1, 2     <br>
<img width="1440" alt="Screenshot 2024-08-11 at 12 56 10 AM" src="https://github.com/user-attachments/assets/d3578340-f241-49f6-aca3-f1bdfdcb3d35">
</details>


<details>
 
<summary><strong>Lab 4:</strong> A C program which reverses an integer, compile with gcc and Risc-V architecture compilers and verify the output suing Spike.</summary>

# Lab-4
## Compiling the C code in GCC. Reverse an integer.

1. Create a file named reverse.c using gedit.
```
gedit reverse.c
```

<img width="1440" alt="Screenshot 2024-08-14 at 7 53 36 PM" src="https://github.com/user-attachments/assets/50257fec-ca33-4349-9888-a2d811e41cdd">

2. Write the code of reversing an integer and save it.
```c
#include <stdio.h>

int main() {

  int n, reverse = 0, remainder;

  printf("Enter an integer: ");
  scanf("%d", &n);

  while (n != 0) {
    remainder = n % 10;
    reverse = reverse * 10 + remainder;
    n /= 10;
  }

  printf("Reversed number = %d", reverse);

  return 0;
}
```   
<img width="1440" alt="Screenshot 2024-08-14 at 7 54 03 PM" src="https://github.com/user-attachments/assets/d70572b1-913c-44e0-bced-f52302fa6f09">

3. Now compile the program using gcc and get output.
```
gcc reverse.c
```
```
./a.out
```
<img width="1440" alt="Screenshot 2024-08-14 at 7 55 22 PM" src="https://github.com/user-attachments/assets/72ab847a-084a-471d-aaef-2b1bd2258f1d">

## Compiling the C code using RISC-V GCC.

1. Compile the code using RISC-V gcc. reverse.o file will be generated.
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o reverse.o reverse.c
```
```
ls -ltr reverse.c
```

<img width="1440" alt="Screenshot 2024-08-14 at 7 59 36 PM" src="https://github.com/user-attachments/assets/976c1f39-dc52-446b-bd5e-fca05c548880">

2. Check the assembly language using this command.
```
riscv64-unknown-elf-objdump -d reverse | less
```
<img width="1440" alt="Screenshot 2024-08-14 at 8 00 47 PM" src="https://github.com/user-attachments/assets/a224ee7f-d20e-47d2-9768-09e3dc6d8780">

3. Now check and verify the output using spike.
```
spike pk reverse.o
```

   <img width="1440" alt="Screenshot 2024-08-14 at 8 01 47 PM" src="https://github.com/user-attachments/assets/c6c97870-9e04-4b19-9ed0-eb038c06f11c">

4. Debug your code using spike and verify
```
spike -d pk reverse.o
```
<img width="1440" alt="Screenshot 2024-08-14 at 8 06 52 PM" src="https://github.com/user-attachments/assets/1b148863-18b3-4dfe-9173-f8c6f4db28e8">

</details>



<details>
 
<summary><strong>Lab 5:</strong> Digital Logic with TL-Verilog using Makerchip</summary>

# Lab-5
## Combinational Circuits in TL-Verilog.

**Introduction to TL-Verilog and Makerchip:**
Makerchip supports the Transaction-Level Verilog (TL-Verilog) standard, which represents a significant advancement by removing the need for the legacy features of traditional Verilog and introducing a more streamlined syntax. TL-Verilog enhances design efficiency by adding powerful constructs for pipelines and transactions, making it easier to develop complex digital circuits.


![symbols-truth-tables-of-common-logic-gates jpg copy](https://github.com/user-attachments/assets/4a4ab848-ca4e-45b5-b207-a3acc221aad6)



### 1. Inverter

Code
```tl-verilog
$out = ! $in;
```
Output waveform


<img width="1440" alt="Screenshot 2024-08-21 at 12 23 56 AM" src="https://github.com/user-attachments/assets/c80f3716-a073-42ae-bb91-b5144c247275">


### 2. 2-input AND gate

Code
```tl-verilog
 $out = $in1 && $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 26 29 AM" src="https://github.com/user-attachments/assets/db8c83c7-17d3-4a07-91a6-94f1b0ca2785">


### 3. 2-input OR gate

Code
```tl-verilog
 $out = $in1 || $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 28 09 AM" src="https://github.com/user-attachments/assets/5c7cf51b-0c3d-41de-ae1d-a676d79de5b7">



### 4. 2-input XOR gate

Code
```tl-verilog
$out = $in1 ^ $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 29 45 AM" src="https://github.com/user-attachments/assets/a8d584a7-28b3-43b1-b9f2-2e094751ea34">


### 5. Operation on Vector

Code
```tl-verilog
$out[2:0] = $in1[1:0] + $in2[1:0];
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 31 11 AM" src="https://github.com/user-attachments/assets/a3563e6a-1194-404b-b763-bb1ce0bcfcc0">


### 6. 2:1 MUX

Code
```tl-verilog
$out = $sel ? $in1 : $in0;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 32 49 AM" src="https://github.com/user-attachments/assets/10d4aadb-694e-4121-91be-39f144d842b1">



### 7. 2:1 MUX using Vectors

Code
```tl-verilog
$out[3:0] = $sel ? $in1[3:0] : $in0[3:0];
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 34 22 AM" src="https://github.com/user-attachments/assets/717c7204-c96b-4857-a0c5-d1a40944121c">


### 8. Combinational Calculator Implementation
The calculator performs four fundamental arithmetic operations: addition, subtraction, multiplication, and division.
Code
```tl-verilog
$val1[31:0] = $rand1[3:0];
$val2[31:0] = $rand2[3:0];

$sum[31:0]  = $val1[31:0] + $val2[31:0];
$diff[31:0] = $val1[31:0] - $val2[31:0];
$prod[31:0] = $val1[31:0] * $val2[31:0];
$quot[31:0] = $val1[31:0] / $val2[31:0];

$out[31:0]  = $sel[1] ? ($sel[0] ? $quot[31:0] : $prod[31:0])
                      : ($sel[0] ? $diff[31:0] : $sum[31:0]);
```
Output waveform

<img width="1440" alt="Screenshot 2024-08-21 at 12 39 29 AM" src="https://github.com/user-attachments/assets/889e7e54-9cf9-4c37-a8e1-0b3e963f60a4">


## Sequential Circuits in TL-Verilog
**Introduction:**
A sequential circuit is a type of digital circuit that uses memory components to retain data, enabling it to generate outputs based on both the current inputs and the circuit's prior state. This distinguishes it from combinational circuits, where the output is solely determined by the present inputs without any regard to past activity. Sequential circuits rely on feedback loops and storage elements like flip-flops or registers to keep track of their internal state over time. This internal state, combined with the present input, influences the circuit's behavior, allowing it to perform tasks that require a history of previous inputs or operations, such as counting, storing data, or sequencing events.

### 1. Fibonacci Series
In mathematics, the Fibonacci sequence is a sequence in which each number is the sum of the two preceding ones.
<img width="579" alt="359608348-6be0adb4-59a0-4c13-978e-5dfe9b1f74e9" src="https://github.com/user-attachments/assets/7245e902-cca7-4175-8548-a33a7855b010">

Code
```tl-verilog
$reset = *reset;
$num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```
Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 45 04 AM" src="https://github.com/user-attachments/assets/32d223f4-30e2-49ab-808f-2941b3cf7f79">


### 2. Free Running Counter
Increases Previous value by 1.
<img width="198" alt="359608319-d53228bc-fda0-4d34-a14e-2885486ce536" src="https://github.com/user-attachments/assets/9176b013-fb95-4ea4-9cf6-272eb8042b57">

Code
```tl-verilog
$reset = *reset;
$cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 50 14 AM" src="https://github.com/user-attachments/assets/2d3b7f3b-ccb0-47c5-ad1a-4d0388996880">



### 3. Sequential Calculator
It functions similarly to a combinational calculator but simulates a real-world scenario where the result of a previous operation is used a

Code
```tl-verilog
$reset = *reset; 
$val1[31:0] = >>1$out;
$val2[31:0] = $rand[3:0]; 
$sum[31:0] =  $val1[31:0] +  $val2[31:0];
$diff[31:0] =  $val1[31:0] -  $val2[31:0];
$prod[31:0] =  $val1[31:0] *  $val2[31:0];
$quot[31:0] =  $val1[31:0] /  $val2[31:0];
$out[31:0] = $reset ? 32'h0 : ($choose[1] ? ($choose[0] ? $quot : $prod):($choose[0] ? $diff : $sum));

```
Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 52 13 AM" src="https://github.com/user-attachments/assets/4296f661-c13f-4f9f-a906-932ce295819c">


## Pipelined Logic

In Transaction-Level Verilog (TL-Verilog), pipelined logic is effectively expressed using pipeline constructs that naturally represent data flow through various stages of a digital design. Each stage in a TL-Verilog pipeline corresponds to a clock cycle, performing operations on data as it moves forward. This method enables clear and concise modeling of sequential logic, where each stage automatically manages the propagation of states and values to the next cycle. By utilizing TL-Verilog's pipeline notation, designers can describe complex, multi-stage operations with an emphasis on transaction flow, which simplifies the design and verification process while improving readability and maintainability.

### 1. To produce a Pipelined Design

<img width="1440" alt="Screenshot 2024-08-21 at 12 55 42 AM" src="https://github.com/user-attachments/assets/6c4aee9a-ebf9-4909-abdd-5c303a5e3fdf">

Code
```tl-verilog
$reset = *reset;
$clk_div = *clk;
|comp
  @1
    $err1 = $bad_input || $illegal_op;
  @3
    $err2 = $over_flow || $err1;
  @6
    $err3 = $div_by_zero || $err2;
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 59 09 AM" src="https://github.com/user-attachments/assets/59121537-c8ec-47f0-96a0-ecbdf5938cf8">

### 2. Cycle Calculator

<img width="756" alt="Screenshot 2024-08-21 at 1 00 22 AM" src="https://github.com/user-attachments/assets/e3525284-26c0-469b-bdec-12f3b73f0cd1">


Code
```tl-verilog
|calc
  @1
    $reset = *reset;
    $clk_div = *clk;
   
    $val1[31:0] = >>2$out[31:0];
    $val2[31:0] = $rand2[3:0];
    $sel[1:0] = $rand3[1:0];
   
    $sum[31:0] = $val1[31:0] + $val2[31:0];
    $diff[31:0] = $val1[31:0] - $val2[31:0];
    $prod[31:0] = $val1[31:0] * $val2[31:0];
    $quot[31:0] = $val1[31:0] / $val2[31:0];
         
    $count = $reset ? 0 : >>1$count + 1;
         
  @2
    $valid = $count;
    $inv_valid = !$valid;
    $calc_reset = $reset | $inv_valid;
    $out[31:0] = $calc_reset ? 32'b0 : ($op[1] ? ($op[0] ? $quot[31:0] : $prod[31:0])
                                             : ($op[0] ? $diff[31:0] 
                                                        : $sum[31:0]));



```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 1 03 48 AM" src="https://github.com/user-attachments/assets/599a36ff-1f3d-4610-bee0-c99586ff3f5a">

## Validity
**Introduction:**
When we generate a waveform, as in previous cases, we receive results for every clock cycle. While there are no compilation errors, logical errors may still be present. These errors can be overlooked during compilation, making them difficult to detect by simply examining the waveforms. Additionally, there may be situations where a "don't care" condition arises, which is irrelevant to our design and should be ignored. To address these issues, we use the concept of validity.

The global clock runs continuously, triggering operations even when they are not needed, which can lead to unnecessary power consumption. In physical implementations, clocks are driven by voltage or current sources, consuming power during each clock cycle. In complex circuits, ignoring unnecessary operations can result in significant power wastage. To minimize power consumption, we remove the clock signal during these unneeded cycles—a process known as clock gating. Validity plays a crucial role in enabling clock gating and ensuring that only necessary operations are executed.


### 1. Validity- Total Distance Calculator


Code
```tl-verilog
|calc
@1 
         $reset = *reset;
         $clk_div = *clk;
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa[3:0];
            $bb_sq[31:0] = $bb[3:0] * $bb[3:0];
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;
         @3
            $cc[31:0] = sqrt($cc_sq);
         @4
            $tot_dist[63:0] = $reset ? 0 : $valid ? (>>1$tot_dist +$cc) : >>1$tot_dist;
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 2 36 41 PM" src="https://github.com/user-attachments/assets/cb1a995c-2da7-4620-b681-23ee8496ae2f">



### 2.Validity- Cycle Calculator


Code
```tl-verilog
$reset = *reset;
   $clk_div = *clk;
   
   |cal
      @1
         $reset = *reset;
         $clk_div = *clk;
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
         $valid = $cnt;
         $valid_or_reset = ($reset | $valid);
         
      
      ?$valid
         @1
            $val1[31:0] = >>2$out;
            $val2[31:0] = $rand2[3:0];
            
            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
            
         @2
            $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                         ($sel[1:0] == 2'b01) ? $diff[31:0]:
                         ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                                $quot[31:0];
            
            $out[31:0] = $valid_or_reset ? 32'h0 : $nxt;
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 44 44 AM" src="https://github.com/user-attachments/assets/03c05bf5-2de7-4705-9b64-b8d399461da7">


</details>




<details>
<summary><strong>Lab 6:</strong> Basic RISCV CPU Micro-architecture.</summary>

# Lab-6
## Implementation of the RISC-V CPU Core

Block Diagram

![1](https://github.com/user-attachments/assets/36c614a2-b5c1-4eab-a2ea-36c7c54f2755)



### 1. Program Counter(PC) and next PC Logic

The Program Counter (PC) is a register that holds the address of the next instruction to be executed, acting as a pointer to the instruction memory. Since the memory is byte-addressable and each instruction is 32 bits long, the PC increments by 4 bytes after each instruction to point to the subsequent one. When the program begins, a reset signal initializes the PC to 0, ensuring that the first instruction is fetched from the correct starting point. For branch instructions, an immediate value is added to the current PC, producing a new address calculated as: NextPC = Incremented PC + Offset value. Typically, the PC increments by 4 to fetch the next sequential instruction, but it resets to zero if a reset signal is received. The accompanying diagram shows the PC's operation, illustrating its progression through instructions and its behavior during resets and branch operations.


<img width="777" alt="2" src="https://github.com/user-attachments/assets/c085854d-b31f-428e-a0f1-c4ef73cec312">

Code

```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
   
\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         $pc[31:0] = $reset ? '0 : >>1$pc + 32'd4;
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 3 08 52 PM" src="https://github.com/user-attachments/assets/f4f71ac6-506e-44c7-ae1c-13e2e9cedf41">

### 2. Instruction Fetch

The Instruction Fetch Unit (IFU) in a CPU is responsible for managing the sequence in which program instructions are fetched from memory and executed, forming a crucial part of the core's control logic. The program counter (PC) indicates the address of the next instruction stored in the instruction memory. This instruction needs to be fetched to continue with processing and subsequent calculations. In this process, the instruction memory is integrated within the program. The Instruction Fetch logic retrieves instructions from the instruction memory and passes them to the Decode logic for further processing. The read address for accessing the instruction memory is provided by the program counter, which then outputs a 32-bit instruction (instr[31:0]).


![4](https://github.com/user-attachments/assets/18cf58fa-38ff-44b8-8053-62171384a168)



Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   //  This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
   
\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         $pc[31:0] = $reset ? '0 : >>1$pc + 32'd4;
         
         $imem_rd_en = !$reset ? 1 : 0;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];

      @1
         $instr[31:0] = $imem_rd_data[31:0];
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 3 20 03 PM" src="https://github.com/user-attachments/assets/ca42d2e1-f9b8-4fd7-8db8-55260d891b44">

### 3. Instruction Decode
 In the decode stage, the objective is to extract detailed information from the instruction fetched in the previous stage. This involves identifying the instruction set, recognizing any immediate values, and extracting register values. During Instruction Decode, each instruction is analyzed to determine its type, whether it includes immediate values, and to extract specific fields. The opcode is mapped to its corresponding instruction, and the bit fields are interpreted based on the RISC-V ISA specifications.


### RISC-V Instruction Formats

Instruction formats in RISC-V act as a 'contract' between the assembly language and the hardware. When an assembly instruction is executed, the hardware understands exactly what to do based on this contract. Each instruction type has a specific format, defined by a series of 0s and 1s, that includes details such as the type of operation and the location of data.
There are 6 types of instructions.
<img width="661" alt="Screenshot 2024-08-10 at 7 35 56 PM" src="https://github.com/user-attachments/assets/8357de63-b6a2-43e6-b74d-04e8cf306e0c">

Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
  m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         //NEXT PC
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
            
            
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
         
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```


Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 6 41 01 PM" src="https://github.com/user-attachments/assets/b02cf520-8e70-42d4-af43-fa55c541efa8">


### 4. Register File Read

Most instructions, especially arithmetic ones, operate on source registers, necessitating a read from these registers. The CPU's register file is designed to support two simultaneous reads for the source operands (rs1 and rs2) and one write per cycle to the destination register. The inputs rs1 and rs2 are provided to the register file, which outputs the corresponding register contents. Enable bits are set based on the validity conditions of rs1 and rs2, as determined earlier. This configuration, known as a 2-port register file, enables the simultaneous reading of two registers. The read instructions are then stored in registers and forwarded to the ALU for processing.


Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         //NEXT PC
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
        
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
          //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
         //REGISTER FILE READ
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 6 59 56 PM" src="https://github.com/user-attachments/assets/904aee3b-a3bf-47a6-b8ef-2dcfc20ddf29">




### 5. Arithmetic Logic Unit (ALU)
The Arithmetic Logic Unit (ALU) is tasked with computing results based on the selected operation. It takes data from two registers provided by the register file, performs the specified arithmetic operation, and then writes the ALU's result back to memory through the register file's write port. At present, the code is designed to support only ADD and ADDI operations for executing the test code.


Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = clk;
         //NEXT PC
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
         //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
         //REGISTER FILE READ
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
         //ARITHMETIC AND LOGIC UNIT (ALU)
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 6 54 05 PM" src="https://github.com/user-attachments/assets/ff502670-3de7-485e-a366-0b9d6c1ef123">





### 6. Register File Write
This step is essential for managing instructions that require storing the output in a destination register (rd). The result from the ALU is written back to memory through the register_file_write port, with the register_file_write_enable signal determined by the validity of the destination register (rd). The register_file_write_index then assigns the value from the destination register (rd) to the appropriate memory location. In the RISC-V architecture, the x0 register is hardwired to always be zero, so an additional condition is implemented to prevent any write operations to the x0 register. After the ALU completes its operations on the register values, the results may need to be written back into the registers. This process ensures that no write occurs to x0, preserving its constant value of zero.


Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
   

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         
         //NEXT PC
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
         //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
         //REGISTER FILE READ         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
         //ARITHMETIC AND LOGIC UNIT (ALU)
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
         
         //REGISTER FILE WRITE
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 6 55 25 PM" src="https://github.com/user-attachments/assets/8debe955-da3e-429b-870a-4903876d29ab">



### 7. Branch instruction
The final step is to incorporate support for branch instructions. In the RISC-V ISA, branches are conditional, meaning that a branch is executed only if a specific condition is met. Additionally, the branch target address (PC) must be calculated, and if the branch condition is satisfied, the PC is updated to this new branch target address. This ensures that the program counter correctly points to the intended instruction when a branch is taken.


Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         //MODIFIED NEXT PC LOGIC FOR INCLUDING BRANCH INSTRCUTIONS
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>1$taken_branch ? >>1$br_target_pc :
                     >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
         //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
         //REGISTER FILE READ         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
         //ARITHMETIC AND LOGIC UNIT (ALU)
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
         
         //REGISTER FILE WRITE
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
         //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 6 57 31 PM" src="https://github.com/user-attachments/assets/3608bf92-c601-4ec4-9985-321c77addfc6">

### RISC-V CPU CORE WITHOUT PIPELINE


Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module
\TLV

   m4_asm(ADD, r10, r0, r0)
   m4_asm(ADD, r14, r10, r0)
   m4_asm(ADDI, r12, r10, 1010)
   m4_asm(ADD, r13, r10, r0)
   m4_asm(ADD, r14, r13, r14)
   m4_asm(ADDI, r13, r13, 1)
   m4_asm(BLT, r13, r12, 1111111111000)
   m4_asm(ADD, r10, r14, r0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>1$taken_branch ? >>1$br_target_pc :
                     >>1$pc + 32'd4;
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         $opcode[6:0] = $instr[6:0];
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         $funct7_valid = $is_r_instr;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         $br_target_pc[31:0] = $pc +$imm;
         *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   |cpu
      m4+imem(@1)
      m4+rf(@1, @1)
   m4+cpu_viz(@4)
\SV
   endmodule
```
Output 
<img width="719" alt="Screenshot 2024-08-22 at 1 14 22 AM" src="https://github.com/user-attachments/assets/90fd7d18-d594-46c7-b1f9-edc05a8d7f8b">
<img width="602" alt="Screenshot 2024-08-22 at 1 14 47 AM" src="https://github.com/user-attachments/assets/01b4ae77-a2a8-49a9-9e3d-e406a1906591">
<img width="713" alt="Screenshot 2024-08-22 at 1 14 00 AM" src="https://github.com/user-attachments/assets/aefeacf8-ce93-4576-930d-2bbcb848997c">



<img width="679" alt="Screenshot 2024-08-22 at 1 13 50 AM" src="https://github.com/user-attachments/assets/17e1abe2-aa84-4c4a-8d40-510f6b4f8fa6">






### Pipelining the RISC-V CPU Core
The RISC-V core designed is divided into 5 pipeline stages. Pipelining in Makerchip is extremely simple. To define a pipeline use the following syntax:

```tl-verilog
|<pipeline_name>
  @<pipeline_stage>
    instruction1 in the current stage
    instruction2 in the current stage
    .
    .
  @<pipeline_stage>
    instruction1 in the current stage
    instruction2 in the current stage
    .
    .

```
 Staging in a pipeline is a physical attribute with no impact to behaviour. At this point support for register file bypass is provided. 

 ### Load/Store Instructions
 Load/store and jump support is added along with the following two extra lines of code to test load and store.
 
```tl-verilog
m4_asm(SW, r0, r10, 10000)
m4_asm(LW, r17, r0, 10000)
```

##  Testing the core with a Testbench
Now that the implementation is complete, a simple testbench statement can be added to ensure whether the core is working correctly or not. The "passed" and "failed" signals are used to communicate with the Makerchip platform to control the simulation. It tells the platform whther the simulation passed without any errors, failed with a list of errors that can be inferred from the log files, and hence to stop the simulation, if failed.

When the following line of code as mentioned below is added on Makerchip, the simulation will pass only if the value stored in r10 = sum of numbers from 1 to 9.
 
```tl-verilog
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);
```
Code
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1011)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==? 11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU implementation
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch equations
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
   *passed = |cpu/xreg[14]>>10$value == (1+2+3+4+5+6+7+8+9+10);
   //Run for 80 cycles without any checks
   *passed = *cyc_cnt > 80;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

Output
<img width="396" alt="3" src="https://github.com/user-attachments/assets/b11bd1c1-8d5c-4fc0-9663-54b8a18f5451">
![360046541-19867260-2437-433f-b6f4-9e8d8496b2f2](https://github.com/user-attachments/assets/662cf470-07b7-487c-ac4d-e1e021587cc3)
![360049026-b9f6e0df-c21a-4024-9aa0-2decdd9c440c](https://github.com/user-attachments/assets/99233d36-f970-439d-9224-69a250979bce)
![360048714-c228e752-a8b4-45f4-8776-0a578b7a7bf1](https://github.com/user-attachments/assets/c7bd9ab0-aac7-41b2-9180-f51f93eccaf9)
![360048222-4187e1ec-16ca-4996-86b5-636452acff7b](https://github.com/user-attachments/assets/a95869d8-bc97-43b9-8c5b-0dd7d49cd844)
<img width="1439" alt="Screenshot 2024-08-22 at 1 51 42 AM" src="https://github.com/user-attachments/assets/1a477614-078f-49d7-a115-633edf8ddac2">
<img width="720" alt="Screenshot 2024-08-22 at 1 51 49 AM" src="https://github.com/user-attachments/assets/fa09e516-e6a0-4284-a612-a2a9c8a72978">

<img width="1439" alt="Screenshot 2024-08-22 at 1 51 42 AM" src="https://github.com/user-attachments/assets/0bb620a3-6aa7-4654-a086-580ddc923089">

Observation:- A 5-stage pipeline design, using clk_div, computes the sum of numbers from 1 to 9 across various stages. The stages include Instruction Fetch, Instruction Decode, Execute, Memory Access, and Write-back. The entire process takes 58 cycles to complete.



</details>

<details>
<summary><strong>Lab 7:</strong> Converting TL-Verilog to Verilog and Simulating with a Testbench.</summary>

# Lab-7
## Converting TL-Verilog to Verilog and Simulating with a Testbench

The RISC-V processor was originally designed using TL-Verilog within the Makerchip IDE. To deploy this design onto an FPGA, it was necessary to convert the TL-Verilog code into standard Verilog. This conversion was successfully carried out using the Sandpiper-SaaS compiler, which facilitated the transition from high-level TL-Verilog to the more widely used Verilog language, suitable for FPGA implementation. After the conversion, pre-synthesis simulations will be performed using the GTKWave simulator to thoroughly verify the functionality and correctness of the design before proceeding to the synthesis and hardware deployment stages. These simulations are crucial for ensuring that the design behaves as expected in a real hardware environment.


1. Install sandpiper-sass using following command:
   ```
   pip3 install pyyaml click sandpiper-saas
   ```
   <img width="1440" alt="Screenshot 2024-08-26 at 2 12 58 PM" src="https://github.com/user-attachments/assets/4200d92a-e61e-49b5-a177-cc981669b21a">

2. Clone the Github and go to VSDbabySoc folder
   ```
   git clone https://github.com/manili/VSDBabySoC.git
   cd VSDBabySoc
   ```
<img width="1440" alt="Screenshot 2024-08-26 at 2 25 02 PM" src="https://github.com/user-attachments/assets/9e42738c-3002-4848-9ba5-3e618a6f1617">

3. Replace the following code in src/module with the rvmyth.tlv given and also change the testbench according to our makerchip code.
```tl-verilog
\m4_TLV_version 1d: tl-x.org
\SV
   m4_include_lib(['https://raw.githubusercontent.com/shivanishah269/risc-v-core/master/FPGA_Implementation/riscv_shell_lib.tlv'])
   
   // Module interface, either for Makerchip, or not.
   m4_ifelse_block(M4_MAKERCHIP, 1, ['
   // Makerchip module interface.
   m4_makerchip_module
   wire CLK = clk;
   logic [9:0] OUT;
   assign passed = cyc_cnt > 300;
   '], ['
   // Custom module interface for BabySoC.
   module rvmyth(
      output reg [9:0] OUT,
      input CLK,
      input reset
   );
   wire clk = CLK;
   '])
   
\TLV
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store the final result value to byte address 16
   m4_asm(LW, r17, r0, 10000)           // Load the final result value from adress 16 to x17
   //
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
   //
   |cpu
      @0
         $reset = *reset;
         $clk_div = *clk;
         `BOGUS_USE($clk_div)
      //Fetch
         // Next PC
         $pc[31:0] = (>>1$reset) ? 32'd0 : 
                     (>>3$valid_taken_br) ? >>3$br_tgt_pc : 
                     (>>3$valid_load) ? >>3$inc_pc : 
                     (>>3$valid_jump && >>3$is_jal) ? >>3$br_tgt_pc :
                     (>>3$valid_jump && >>3$is_jalr) ? >>3$jalr_tgt_pc : >>1$inc_pc;
         
         $imem_rd_en = !$reset;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
      @1         
         $instr[31:0] = $imem_rd_data[31:0];
         $inc_pc[31:0] = $pc + 32'd4;          
      // Decode   
         $is_i_instr = $instr[6:2] == 5'b00000 ||
                       $instr[6:2] == 5'b00001 ||
                       $instr[6:2] == 5'b00100 ||
                       $instr[6:2] == 5'b00110 ||
                       $instr[6:2] == 5'b11001;
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] == 5'b10100 ||
                       $instr[6:2] == 5'b01100 ||
                       $instr[6:2] == 5'b01101;                       
         $is_b_instr = $instr[6:2] == 5'b11000;
         $is_u_instr = $instr[6:2] == 5'b00101 ||
                       $instr[6:2] == 5'b01101;
         $is_s_instr = $instr[6:2] == 5'b01000 ||
                       $instr[6:2] == 5'b01001;
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20] } :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7] } :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31:12] , 12'b0} : 
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $funct7_valid = $is_r_instr;
         
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         // Branch Instruction
         $is_beq = $dec_bits[9:0] == 10'b000_1100011;
         $is_bne = $dec_bits[9:0] == 10'b001_1100011;
         $is_blt = $dec_bits[9:0] == 10'b100_1100011;
         $is_bge = $dec_bits[9:0] == 10'b101_1100011;
         $is_bltu = $dec_bits[9:0] == 10'b110_1100011;
         $is_bgeu = $dec_bits[9:0] == 10'b111_1100011;
         
         // Arithmetic Instruction
         $is_add = $dec_bits == 11'b0_000_0110011;
         $is_addi = $dec_bits[9:0] == 10'b000_0010011;
         $is_or = $dec_bits == 11'b0_110_0110011;
         $is_ori = $dec_bits[9:0] == 10'b110_0010011;
         $is_xor = $dec_bits == 11'b0_100_0110011;
         $is_xori = $dec_bits[9:0] == 10'b100_0010011;
         $is_and = $dec_bits == 11'b0_111_0110011;
         $is_andi = $dec_bits[9:0] == 10'b111_0010011;
         $is_sub = $dec_bits == 11'b1_000_0110011;
         $is_slti = $dec_bits[9:0] == 10'b010_0010011;
         $is_sltiu = $dec_bits[9:0] == 10'b011_0010011;
         $is_slli = $dec_bits == 11'b0_001_0010011;
         $is_srli = $dec_bits == 11'b0_101_0010011;
         $is_srai = $dec_bits == 11'b1_101_0010011;
         $is_sll = $dec_bits == 11'b0_001_0110011;
         $is_slt = $dec_bits == 11'b0_010_0110011;
         $is_sltu = $dec_bits == 11'b0_011_0110011;
         $is_srl = $dec_bits == 11'b0_101_0110011;
         $is_sra = $dec_bits == 11'b1_101_0110011;
         
         // Load Instruction
         $is_load = $dec_bits[6:0] == 7'b0000011;
         
         // Store Instruction
         $is_sb = $dec_bits[9:0] == 10'b000_0100011;
         $is_sh = $dec_bits[9:0] == 10'b001_0100011;
         $is_sw = $dec_bits[9:0] == 10'b010_0100011;
         
         // Jump Instruction
         $is_lui = $dec_bits[6:0] == 7'b0110111;
         $is_auipc = $dec_bits[6:0] == 7'b0010111;
         $is_jal = $dec_bits[6:0] == 7'b1101111;
         $is_jalr = $dec_bits[9:0] == 10'b000_1100111;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2   
      // Register File Read
         $rf_rd_en1 = $rs1_valid;
         ?$rf_rd_en1
            $rf_rd_index1[4:0] = $rs1[4:0];
         
         $rf_rd_en2 = $rs2_valid;
         ?$rf_rd_en2
            $rf_rd_index2[4:0] = $rs2[4:0];
            
      // Branch Target PC       
         $br_tgt_pc[31:0] = $pc + $imm;
      
      // Jump Target PC
         $jalr_tgt_pc[31:0] = $src1_value + $imm;
         
      // Input signals to ALU
         $src1_value[31:0] = ((>>1$rd == $rs1) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data1[31:0];
         $src2_value[31:0] = ((>>1$rd == $rs2) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data2[31:0];
         
      @3   
         
      // ALU
         $sltu_result = $src1_value < $src2_value ;
         $sltiu_result = $src1_value < $imm ;
         
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value : 
                         $is_or ? $src1_value | $src2_value : 
                         $is_ori ? $src1_value | $imm :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_xori ? $src1_value ^ $imm :
                         $is_and ? $src1_value & $src2_value :
                         $is_andi ? $src1_value & $imm :
                         $is_sub ? $src1_value - $src2_value :
                         $is_slti ? (($src1_value[31] == $imm[31]) ? $sltiu_result : {31'b0,$src1_value[31]}) :
                         $is_sltiu ? $sltiu_result :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_srai ? ({{32{$src1_value[31]}}, $src1_value} >> $imm[4:0]) :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_slt ? (($src1_value[31] == $src2_value[31]) ? $sltu_result : {31'b0,$src1_value[31]}) :
                         $is_sltu ? $sltu_result :
                         $is_srl ? $src1_value >> $src2_value[5:0] :
                         $is_sra ? ({{32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_lui ? ({$imm[31:12], 12'b0}) :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 : 
                         ($is_load || $is_s_instr) ? $src1_value + $imm : 32'bx;
                         
      // Register File Write
         $rf_wr_en = ($rd_valid && $valid && $rd != 5'b0) || >>2$valid_load;
         ?$rf_wr_en
            $rf_wr_index[4:0] = !$valid ? >>2$rd[4:0] : $rd[4:0];
      
         $rf_wr_data[31:0] = !$valid ? >>2$ld_data[31:0] : $result[31:0];
      
      // Branch
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) : 1'b0;
                     
         $valid_taken_br = $valid && $taken_br;
         
      // Load
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load || >>1$valid_jump || >>2$valid_jump);
      
      // Jump
         $valid_jump = $valid && $is_jump;
                  
      @4
         $dmem_rd_en = $valid_load;
         $dmem_wr_en = $valid && $is_s_instr;
         $dmem_addr[3:0] = $result[5:2];
         $dmem_wr_data[31:0] = $src2_value[31:0];
         
      @5   
         $ld_data[31:0] = $dmem_rd_data[31:0];
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

         `BOGUS_USE($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu)
         `BOGUS_USE($is_sb $is_sh $is_sw)
   // Assert these to end simulation (before Makerchip cycle limit).
   \SV_plus
      always @ (posedge CLK) begin
         *OUT = |cpu/xreg[14]>>5$value;                
      end
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)

     
\SV
   
   endmodule
```
<img width="1440" alt="Screenshot 2024-08-26 at 2 23 56 PM" src="https://github.com/user-attachments/assets/9fc83cd0-4496-439a-9d41-96e5cd1e0979">


4. Now run the following command to convert the code.
   ```
   sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
   ```
   <img width="1440" alt="Screenshot 2024-08-26 at 2 25 02 PM" src="https://github.com/user-attachments/assets/03457a24-c689-4526-85b0-2ca47254779f">

5. Now create the pre_synth_sim.vcd by running the following command.
   ```
   make pre_synth_sim
   ```
   <img width="1440" alt="Screenshot 2024-08-26 at 2 26 52 PM" src="https://github.com/user-attachments/assets/1bb2a8fc-3858-429f-99d5-4e38f617cdb7">

6. To compile and simulate RISC-V design run the following code:
```
iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
cd output
./pre_synth_sim.out
```
<img width="1440" alt="Screenshot 2024-08-26 at 2 27 37 PM" src="https://github.com/user-attachments/assets/49c5cc8c-879c-405e-a587-1c178910d5f2">

7. To open the Simulation file in gtkwave tool, run the following command. 
```
gtkwave pre_synth_sim.vcd
```
<img width="1440" alt="Screenshot 2024-08-26 at 2 27 49 PM" src="https://github.com/user-attachments/assets/271a8ead-4f28-403f-ad42-8d5fd63d2569">


### Waveforms from Makerchip platform IDE by running .tlv file for comparison

#### Clk

<img width="1439" alt="3" src="https://github.com/user-attachments/assets/6eb96e21-d6b7-4719-ba0c-e2a4a2d1b11b">

#### Reset
![2](https://github.com/user-attachments/assets/15dfbeff-b072-4e64-8310-5379aca39a95)

#### Waveform
![1](https://github.com/user-attachments/assets/3729db08-2662-4615-8d2c-865b0256c363)


### Waveforms from GTKwave platform by running .v file after conversion 
#### Clk,Reset, Waveforms in one
<img width="1440" alt="Screenshot 2024-08-26 at 2 30 03 PM" src="https://github.com/user-attachments/assets/31215721-f406-43b1-b5a0-1cd827cda11b">

We can see the gradual increment in sum from 0 to 9 in the end the sum of numbers from 0 to 9 is 45 which is Ox2D in hexadecimal.
</details>

<details>
<summary><strong>Lab 8:</strong> To generate waveform for DAC and PLL peripheral for Risc-V processor.</summary>

# Lab-8
## To generate waveform for DAC and PLL peripheral for Risc-V processor.


The VSDBabySoC is a compact yet powerful SoC based on the RISCV architecture. It was designed with the primary goal of testing three open-source IP cores together for the first time and calibrating the analog components. The SoC includes an RVMYTH microprocessor, an 8x-PLL for stable clock generation, and a 10-bit DAC for communication with other analog devices.

![6](https://github.com/user-attachments/assets/00eb65f1-60fc-4758-afe6-38b293beaf8f)

### BabySoC Simulation
Developing and simulating the complete micro-architecture of a RISC-V CPU is a complex task. For this simulation, we'll focus on incorporating two key IP blocks: PLL and DAC.


### Phase-Locked Loop (PLL)
A Phase-Locked Loop (PLL) is an electronic circuit designed to synchronize the phase and frequency of an output signal with that of a reference signal. It typically comprises three main components:

#### Phase Detector: This component compares the phases of the reference signal and the output signal, generating an error signal that represents their difference.
#### Loop Filter: It smooths out the error signal to reduce noise and enhance system stability.
Voltage-Controlled Oscillator (VCO): The VCO adjusts its output frequency in response to the filtered error signal, working to minimize the phase difference.
PLLs are commonly used in applications like clock generation, frequency synthesis, and data recovery in communication systems.

### Digital-to-Analog Converter (DAC)
A Digital-to-Analog Converter (DAC) translates digital signals (usually in binary form) into analog signals, such as voltage or current. This conversion is crucial in systems where digital data must interact with analog devices or be presented in a form perceivable by humans, like in audio or video output.

DACs are widely used in applications such as audio playback, video display, and signal processing.


1. Clone this repo: https://github.com/Subhasis-Sahu/BabySoC_Simulation.git using the following command.
```
git clone https://github.com/Subhasis-Sahu/BabySoC_Simulation.git
```
```bash
cd BabySoC_Simulation
```

<img width="1440" alt="Screenshot 2024-09-02 at 1 01 19 AM" src="https://github.com/user-attachments/assets/c0f2e9e8-aae6-4497-80e7-c772f87ca9a1">

2. Then run the following commands, also you have to change rvmyth.v from the previous labs as shown.
<img width="1440" alt="Screenshot 2024-09-02 at 1 01 29 AM" src="https://github.com/user-attachments/assets/a6160320-cc39-4380-ae2d-cd99df69dca5">
<img width="1440" alt="Screenshot 2024-09-02 at 1 02 03 AM" src="https://github.com/user-attachments/assets/e24af8bd-30d2-45a0-84c6-b0648ab05771">

```bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
```
<img width="1440" alt="Screenshot 2024-09-02 at 1 02 29 AM" src="https://github.com/user-attachments/assets/3696b017-24bc-4d1f-8018-db247c52285e">


```bash
./pre_synth_sim.out
```
<img width="1440" alt="Screenshot 2024-09-02 at 1 02 38 AM" src="https://github.com/user-attachments/assets/792cb409-c962-4a2e-ac54-7208671e23d2">

3. Open GTKwave
```
gtkwave pre_synth_sim.vcd
```
<img width="1440" alt="Screenshot 2024-09-02 at 1 21 20 AM" src="https://github.com/user-attachments/assets/d9afa8ae-4a71-4833-a91c-c89fba5c9c33">


### Waveforms 
<img width="1440" alt="Screenshot 2024-09-02 at 1 06 22 AM" src="https://github.com/user-attachments/assets/e4e33b42-3d4b-450b-8c7d-b877dc889d8e">



NOTE: This my system of college os SARL-LAB (I have created a folder to show my name). The simulation successfully demonstrates the integration of DAC and PLL peripherals with the RISC-V processor, converting digital outputs to analog signals.
</details>



<details>
<summary><strong>Laboratory 9:</strong> RTL design using Verilog with SKY130 Technology.</summary>

---
<details>
<summary><strong>Day 1:</strong> Introduction to Verilog RTL design and Synthesis.</summary>

### 1.1. Introduction to open source simulator iverilog

In digital circuit design, **register-transfer level** (RTL) is an abstraction that models a synchronous digital circuit by describing how data flows between hardware registers and how logic operations are applied to these signals. This RTL abstraction is used in HDL (Hardware Description Language) to create high-level models of a circuit, which can then be used to derive lower-level representations and, eventually, the actual hardware layout.

**Simulator**: A tool used to verify the design. In this workshop, we utilize the iverilog tool. Simulation involves generating models that replicate the behavior of the intended device (simulation models) and creating test models to validate the device (test benches). RTL Design: Consists of one or more Verilog files that implement the required design specifications and functionality for the circuit.

**Test Bench**: The configuration used to provide stimulus (test vectors) to the design in order to verify its functionality.

<img width="819" alt="Screenshot 2024-10-21 at 8 58 40 PM" src="https://github.com/user-attachments/assets/8bde6f58-15b1-42b2-afba-d07dcfb4a91e">

#### SIMULATION FLOW

<img width="809" alt="Screenshot 2024-10-21 at 9 03 57 PM" src="https://github.com/user-attachments/assets/0e9e65c0-9884-4f32-9253-6670dba8c4ba">



#### LAB-1
Steps:
```
mkdir VLSI 
cd VLSI
git clone https://github.com/kunalg123/vsdflow.git
cd vsdflow
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
ls
```
Verilog files which will be used in this lab
<img width="1440" alt="Screenshot 2024-10-21 at 9 08 17 PM" src="https://github.com/user-attachments/assets/7076b1cb-8ac5-4d07-b48f-33867d569c72">

```
cd sky130RTLDesignAndSynthesisWorkshop
ls -R
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 09 57 PM" src="https://github.com/user-attachments/assets/25ea9cc4-c79c-4b45-a4d3-9e11e124773f">

#### LAB-2

To compile the verilog and testbench file use the following commands which will generate an executable file and will dump the waveform to view it using the gtkwave.
In this lab we will implement a 2:1 multiplexer.

Steps:
```
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```


<img width="1440" alt="Screenshot 2024-10-21 at 9 14 57 PM" src="https://github.com/user-attachments/assets/3471a86a-ad5c-43c8-b4c5-ca3ab2fa1ee7">
<img width="1440" alt="Screenshot 2024-10-21 at 9 15 54 PM" src="https://github.com/user-attachments/assets/4ace2376-aa4b-4e39-b06c-a8420971d33b">


To view the files
```
gedit good_mux.v
gedit tb_good_mux.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 19 06 PM" src="https://github.com/user-attachments/assets/07f06d52-991b-4c51-9e84-e8e278854192">

<img width="1440" alt="Screenshot 2024-10-21 at 9 17 20 PM" src="https://github.com/user-attachments/assets/f496e505-c73b-4492-ac2f-290ad9ba670a">
<img width="1440" alt="Screenshot 2024-10-21 at 9 17 49 PM" src="https://github.com/user-attachments/assets/10e0e63f-263f-411c-b236-55f24bab8988">


#### LAB-3
**Introduction to Yosys & Logic Synthesis**

Synthesizer is the tool used for converting the RTL to netlist. Yosys is one such open source synthesizer. Yosys optimizes the design, mapping it to specific target technology libraries or FPGA architectures, and generates an optimized netlist that can be further analyzed and prepared for physical layout and fabrication.
<img width="843" alt="Screenshot 2024-10-21 at 9 23 58 PM" src="https://github.com/user-attachments/assets/7280efc4-db86-45d8-ad15-8fb42160c057">


**Verifiying the Synthesis**
The same testbench that is used for the simulation can be used for the synthesized netlist as well.

<img width="830" alt="Screenshot 2024-10-21 at 9 24 04 PM" src="https://github.com/user-attachments/assets/0c0bdc6b-db3b-4061-b1bd-c45d9e19f3dc">

Synthesis has three steps: RTL to Gate level translation, The design is then converted into gates and the connections are made between the gates and the output is given out as a file called netlist.
<img width="564" alt="Screenshot 2024-10-21 at 9 26 49 PM" src="https://github.com/user-attachments/assets/1089ca5d-e80e-432b-8c0d-c52b794466fd">


RTL Design - behavioral representation in HDL form for the required specification.

 **Synthesis** - RTL to Gate level translation.
 The design is converted int gates and connections are made. This given outas a file called **netlist**.

>_.lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._
<img width="859" alt="Screenshot 2024-10-21 at 9 27 00 PM" src="https://github.com/user-attachments/assets/cb82d057-9ca0-41b2-8e6e-d3ffd1398f7c">


#### Faster cells and Slower Cells

A cell delay in the digital logic circuit depends on the load of the circuit which here is Capacitance.

Faster the charging / discharging of the capacitance --> Lesser is the Cell Delay

Inorder to charge/discharge the capacitance faster, we use wider transistors that can source more current. This will help us reduce the cell delay but at the same time, wider transistors consumer more power and area. Similarly, using narrower transistors help in reduced area and power but the circuit will have a higher cell delay. Hence, we have to compromise on area and power if we are to design a circuit with low cell delay.

#### Constraints

A Constraint is a guidance file given to a synthesizer inorder to enable an optimum implementation of the logic circuit by selecting the appropriate flavour of cells (fast or slow).


**Yosys flow**
1. start yosys.
          
```
yosys
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 31 02 PM" src="https://github.com/user-attachments/assets/c82c195e-3951-4b7d-94b5-fe052630fee6">

2. load the sky130 standard library.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 31 17 PM" src="https://github.com/user-attachments/assets/912265e2-c5f9-4df0-a131-ea9e1c3a2c57">

3. Read the design files
```
read_verilog good_mux.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 31 57 PM" src="https://github.com/user-attachments/assets/e32133d8-8815-438f-8f22-ab429e12e2fa">

4. Synthesize the top level module
```
synth -top good_mux
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 32 24 PM" src="https://github.com/user-attachments/assets/af44ed1a-fda9-4e45-afb4-78a92bc3142e">

        
5. Map to the standard library
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 32 41 PM" src="https://github.com/user-attachments/assets/1551bdb6-1135-4dd4-b434-7416d10673d1">


6. Two view the result as a graphich use the show command.
```
show
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 32 48 PM" src="https://github.com/user-attachments/assets/4275b1e9-875d-44c9-8c23-b1efd0436154">

7. To write the result netlist to a file use the write_veriog command. This will output the netlist to a file in the current directory.
```
write_verilog -noattr good_mux_netlist.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 34 22 PM" src="https://github.com/user-attachments/assets/1f44c563-211c-4b32-a031-0bb5781b1d47">

</details>


<details>
<summary><strong>Day 2:</strong> Timing libs, hierarchical vs flat synthesis and efficient flop coding styles.</summary>


### 2.1. Introduction to timing labs
navigate to the verilog_files directory then type these below command
Command to open the libary file
```
gedit ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1440" alt="Screenshot 2024-10-21 at 9 41 13 PM" src="https://github.com/user-attachments/assets/68467fce-e9ee-49d1-ac6c-1b67ced239b0">

#### Library file

<img width="1440" alt="Screenshot 2024-10-21 at 9 40 21 PM" src="https://github.com/user-attachments/assets/fc5539a2-00b9-4bed-bb54-44fc7be09215">

#### Contents
For a design to work, there are three important parameters that determines how the Silicon works: Process (Variations due to Fabrications), Voltage (Changes in the behavior of the circuit) and Temperature (Sensitivity of semiconductors). Libraries are characterized to model these variations. 

<img width="832" alt="Screenshot 2024-10-21 at 9 42 37 PM" src="https://github.com/user-attachments/assets/e6c1ca5c-0122-499f-9ae9-98de0700defc">



### The .lib(liberty) File contents
The timing data of standard cells is provided in the liberty format. Every .lib file will provide timing, power, noise, area information for a single corner ie process,voltage, temperature etc.
1. Library\
general information common to all cells in the library.
2. Cell\
specific information about each standard cell.
3. Pin\
Timing, power, capacitance, leakage functionality etc characteristics for each pin in each cell.
4. technology("cmos"): Specifies the technology as CMOS.
5. delay_model : "table_lookup": Defines the delay model.
6. bus_naming_style : "%s[%d]": Defines the naming convention for buses.
7. time_unit : "1ns": Sets the unit of time.
8. voltage_unit : "1V": Sets the unit of voltage.
9. leakage_power_unit : "1nW": Defines the unit for leakage power.
10. current_unit : "1mA": Sets the unit of current.
11. pulling_resistance_unit : "1kohm": Specifies the unit for pulling resistance.
12. capacitive_load_unit(1.0000000000, "pf"): Defines the unit for capacitive load.

We can also find different versions of the same cell. For example, consider the AND gate

<img width="1440" alt="Screenshot 2024-10-21 at 9 44 15 PM" src="https://github.com/user-attachments/assets/3c4e61fe-cf3a-40ef-a2bb-be7feb196969">
<img width="1440" alt="Screenshot 2024-10-21 at 9 45 20 PM" src="https://github.com/user-attachments/assets/25964e87-7169-4746-a363-e419cc253910">
<img width="1440" alt="Screenshot 2024-10-21 at 9 44 28 PM" src="https://github.com/user-attachments/assets/794dd81a-8ada-42d7-904c-4eba024d8926">

We can observe that:

- and2_0 -- taking the least area, more delay and low power.
- and2_1 -- taking more area, less delay and high power.
- and2_2 -- taking the largest area, larger delay and highest power.


### Hierarchial synthesis vs Flat synthesis 
#### Hierarchial synthesis 

Hierarchical synthesis involves synthesizing a complex design by breaking it down into various sub-modules, where each module is synthesized separately to generate gate-level netlists and then integrated. Hierarchical synthesis allows for better organization, reuse of modules, and incremental changes to the design without affecting the entire system. Flat synthesis, on the other hand, treats the entire design as a single, monolithic unit during the synthesis process and regardless of any hierarchical relations, it is synthesized into a single netlist. Flat synthesis can be useful for optimizing certain designs but it becomes challenging to maintain, analyze, and modify the design due to its lack of structural modularity.

Consider the verilog file `multiple_modules.v` which is given in the verilog_files directory

```
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
    sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

To perform **hierarchical synthesis** on the `multiple_modules.v` file type the following commands:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
!gedit multiple_modules_hier.v
```

The following statistics are displayed:


<img width="1440" alt="Screenshot 2024-10-21 at 9 54 38 PM" src="https://github.com/user-attachments/assets/d217778d-ee53-421f-9eb4-16c8c31742da">

<img width="1440" alt="Screenshot 2024-10-21 at 9 54 44 PM" src="https://github.com/user-attachments/assets/aa25e694-eacc-48ff-a29f-e8a9fe67c498">
<img width="1440" alt="Screenshot 2024-10-21 at 9 55 04 PM" src="https://github.com/user-attachments/assets/a5d8fece-e803-44bb-bb24-ecf83960afbf">


**Netlist:**
<img width="1440" alt="Screenshot 2024-10-21 at 9 55 18 PM" src="https://github.com/user-attachments/assets/cf8d1cca-324c-4bef-bc87-3ff2be597027">

**Hierarchical netlist code:**
<img width="1440" alt="Screenshot 2024-10-21 at 9 58 51 PM" src="https://github.com/user-attachments/assets/1255d548-0505-4d19-96b0-11eddf0b9d62">





#### Flat synthesis 


To perform **flat synthesis** on the `multiple_modules.v` file type the following commands:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_modules_flat.v
```

The following statistics are displayed:
<img width="1440" alt="Screenshot 2024-10-21 at 10 03 13 PM" src="https://github.com/user-attachments/assets/d6ef6538-d030-42f4-b8b7-93534884f6a9">
<img width="1440" alt="Screenshot 2024-10-21 at 10 03 21 PM" src="https://github.com/user-attachments/assets/099e7916-a9e7-4e24-97c5-348b61a88380">
<img width="1440" alt="Screenshot 2024-10-21 at 10 03 41 PM" src="https://github.com/user-attachments/assets/95c912a3-7b08-4bcd-a8de-9dcf3051d4ca">


**Netlist:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 03 48 PM" src="https://github.com/user-attachments/assets/fb8e1ef9-6904-4b83-85a0-e787029394b8">

**Flat synthesis netlist code:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 04 49 PM" src="https://github.com/user-attachments/assets/2f835a08-fe0e-4c6e-a8fd-071607ab1114">


#### Sub Module synthesis 


To perform **sub module synthesis**. type the below commands:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog multiple_modules.v 
synth -top sub_module
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr multiple_modules_sub1.v
!gedit multiple_modules_sub1.v
```

The following statistics are displayed:
<img width="1440" alt="Screenshot 2024-10-21 at 10 07 26 PM" src="https://github.com/user-attachments/assets/546cfb60-23dc-4669-bbc4-39c9b7871435">

<img width="1440" alt="Screenshot 2024-10-21 at 10 08 12 PM" src="https://github.com/user-attachments/assets/b5f3c736-3509-4e72-8105-62e2f8d9aa89">


**Netlist:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 08 22 PM" src="https://github.com/user-attachments/assets/54520478-181a-4e13-9e1a-07cff394a2a5">



**Netlist code:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 10 08 PM" src="https://github.com/user-attachments/assets/5b29e34c-de93-42ae-b7b0-5518cc60089f">




## D Flip-Flop Design and Simulation Using Icarus Verilog, GTKWave, and Yosys:

### Various Flop coding styles and optimization
In a digital design, when an input signal changes state, the output changes after a propogation delay. All logic gates add some delay to singals. These delays cause expected and unwanted transitions in the output, called as _Glitches_ where the output value is momentarily different from the expected value. An increased delay in one path can cause glitch when those signals are combined at the output gate. In short, more combinational circuits lead to more glitchy outputs that will not settle down with the output value. 

#### Flip flop overview
A D flip-flop is a sequential element that follows the input pin d at the clock's given edge. D flip-flop is a fundamental component in digital logic circuits.
There are two types of D Flip-Flops being implemented: Rising-Edge D Flip Flop and Falling-Edge D Flip Flop.


Every flop element needs an initial state, else the combinational circuit will evaluate to a garbage value. In order to achieve this, there are control pins in the flop namely: Set and Reset which can either be Synchronous or Asynchronous. 



#### FLIP FLOP SIMULATION

**Asynchronous Reset Flip-flop:**

Verilog Code:

```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

Run the below code to view the simulation:

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 18 02 PM" src="https://github.com/user-attachments/assets/3f3295e2-162c-4a22-86b1-2ae2b5db9256">

Waveform:
<img width="1440" alt="Screenshot 2024-10-21 at 10 20 57 PM" src="https://github.com/user-attachments/assets/0c034c1b-1926-41ea-b7b8-1053df6b5325">



Run the below code to view the netlist:
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 25 33 PM" src="https://github.com/user-attachments/assets/b4f60ca0-87f0-449b-9692-efe944f84407">

Netlist:
<img width="1440" alt="Screenshot 2024-10-21 at 10 25 55 PM" src="https://github.com/user-attachments/assets/21aa4958-71e1-47b7-8653-cfdfb755adcc">



**Synchronous Reset Flip-flop:**

Verilog Code:

```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

Run the below code to view the simulation:

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 27 59 PM" src="https://github.com/user-attachments/assets/094c9ec5-4179-4da5-99b4-03cbdcdc2bb0">

Waveform:
<img width="1440" alt="Screenshot 2024-10-21 at 10 27 50 PM" src="https://github.com/user-attachments/assets/d2239400-7f5c-45d5-86b6-0b5ad191c47e">



Run the below code to view the netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 28 45 PM" src="https://github.com/user-attachments/assets/fc5bd7f2-4ad0-48c1-9eee-2c40aaa5e5ac">

Netlist:

<img width="1440" alt="Screenshot 2024-10-21 at 10 29 06 PM" src="https://github.com/user-attachments/assets/0c47326a-ff33-42f6-b990-7cb5ff105997">


**Asynchronous Set Flip-flop:**

Verilog Code:

```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

Run the below code to view the simulation:
```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 31 05 PM" src="https://github.com/user-attachments/assets/d78cb1c1-221d-4ff2-b8a1-048a36204740">

Waveform:
<img width="1440" alt="Screenshot 2024-10-21 at 10 30 53 PM" src="https://github.com/user-attachments/assets/46865392-35a6-4eac-9900-8969cf99385f">



Run the below code to view the netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 31 51 PM" src="https://github.com/user-attachments/assets/50aa460a-0a62-4ab9-93fa-ca2b3c7d34c5">

Netlist:
<img width="1440" alt="Screenshot 2024-10-21 at 10 32 11 PM" src="https://github.com/user-attachments/assets/25a46a1d-abcb-43c0-af64-6934f05ad26e">



#### Optimizations

```
modules used are opened using the command
vim mult_*.v -o
yosys 
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
write_verilog -noattr mult_2.v
!gedit mult_2.v
```
## Example 1: mult_2.v 
We can see the multiplication of a number by 2 doesnt really need any extra hardware we just need to append the LSB's with zeroes and the remaining bits are the input bits of same, It can be realised by grouding the LSB's and wiring the input properly to the output.

**Expected Logic**
```
module mul2 (input [2:0] a, output [3:0] y);
assign y = a * 2;
endmodule
```


**Statistics:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 37 17 PM" src="https://github.com/user-attachments/assets/7110cd0f-694e-4b92-bc63-5f255d5f6960">



 ##### No hardware requirements - No # of memories, memory bites, processes and cells. Number of cells inferred is 0.


 **Netlist:**
 <img width="1440" alt="Screenshot 2024-10-21 at 10 37 39 PM" src="https://github.com/user-attachments/assets/8cb15905-93a1-4c90-aab6-20ff0adc3e93">
**NetList File:**
 <img width="1440" alt="Screenshot 2024-10-21 at 10 38 10 PM" src="https://github.com/user-attachments/assets/c5873d5b-3403-436b-b033-c8175f8583e2">

## Example 2: mult_8.v

Consider the verilog code 'mult_8.v' :

```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

In this design the 3-bit input number "a" is multiplied by 9 i.e (a*9) which can be re-written as (a*8) + a . The term (a*8) is nothing but a left shifting the number a by three bits. Consider that a = a2 a1 a0. (a*8) results in a2 a1 a0 0 0 0. (a*9)=(a*8)+a = a2 a1 a0 a2 a1 a0 = aa(in 6 bit format). Hence in this case no hardware realization is required. The synthesized netlist of this design is shown below:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult8
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_8_net.v
```

**Statistics:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 40 56 PM" src="https://github.com/user-attachments/assets/4fa58ebd-c677-4c51-bac3-8b03ef9b28cb">



**Netlist:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 41 14 PM" src="https://github.com/user-attachments/assets/172dc09b-d00e-49b2-a86c-5f85f7a74ae3">




**Netlist code:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 41 35 PM" src="https://github.com/user-attachments/assets/1f10058d-50b2-4363-a230-227c65f9d30b">
</details>


<details>
<summary><strong>Day 3:</strong> Combinational and sequential optimizations.</summary>

**Logic Circuits**

Combinational circuits are defined as the time independent circuits which do not depends upon previous inputs to generate any output are termed as combinational circuits. Sequential circuits are those which are dependent on clock cycles and depends on present as well as past inputs to generate any output.

### 3.1. Introduction to Logic Optimizations

### Combinational Logic Optimization
**Why do we need Combinational Logic Optimizations?**

* Primarily to squeeze the logic to get the most optimized design.
    * An optimized design results in comprehensive Area and Power saving.

#### Types of Combinational Optimizations

* Constant Propagation 
	* Direct Optimization technique
* Boolean Logic Optimization.
	* Karnaugh map
	* Quine Mckluskey

#### CONSTANT PROPAGATION

In Constant propagation techniques, inputs that are no way related or affecting the changes in the output are ignored/optimized to simplify the combination logic thereby saving area and power usage by those input pins.
<img width="752" alt="Screenshot 2024-10-21 at 11 20 55 PM" src="https://github.com/user-attachments/assets/b91cc863-ebb2-43b9-98f2-5182b9f40887">


```
Y =((AB)+ C)'
If A = 0
Y =((0)+ C)' = C'
```
#### BOOLEAN LOGIC OPTIMIZATION


Boolean logic optimization is nothing simplifying a complex boolean expression into a simplified expression by utilizing the laws of boolean logic algebra.
```
assign y = a?(b?c:(c?a:0)):(!c)
```
above is simplified as
```
y = a'c' + a(bc + b'ca) 
y = a'c' + abc + ab'c 
y = a'c' + ac(b+b') 
y = a'c' + ac
y = a xnor c
```
<img width="527" alt="Screenshot 2024-10-21 at 11 21 35 PM" src="https://github.com/user-attachments/assets/49eeea6c-46dc-46c4-9c4c-ec08763f6779">

The circuit can be optimised as follows:
<img width="745" alt="Screenshot 2024-10-21 at 11 21 43 PM" src="https://github.com/user-attachments/assets/863a6e80-85ae-4972-95ec-77b78135f20b">


### Sequential Logic Optimization

#### Types of Sequential Optimizations

* Basic Technique
	* Sequential Constant Propagation
* Advanced Technique
	* State Optimization
	* Retiming
	* Sequential Logic cloning(Floorplan aware synthesis)
	
#### COMBINATIONAL LOGIC OPTIMIZATION  

**Example 1:**

Verllog code:

```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

The above code infers a multiplexer and since one of the inputs of the multiplexer is always connected to the ground it will infer an AND gate on optimisation.

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check_net.v
!gedit opt_check_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 23 20 PM" src="https://github.com/user-attachments/assets/57bf19f3-bd4c-45b2-bca5-d5d82834d01d">

<img width="1440" alt="Screenshot 2024-10-21 at 11 23 43 PM" src="https://github.com/user-attachments/assets/48e7e104-2ed5-4e9b-8593-d4f7981b1513">
<img width="1440" alt="Screenshot 2024-10-21 at 11 24 02 PM" src="https://github.com/user-attachments/assets/75b7a358-35e2-4517-bf8a-0278fe2ef0bf">


**Example 2:**

Verllog code:

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

Since one of the inputs of the multiplexer is always connected to the logic 1 it will infer an OR gate on optimisation.The OR gate will be NAND implementation since NOR gate has stacked pmos while NAND implementation has stacked nmos.

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check2_net.v
!gedit opt_check2_net.v
```

<img width="1440" alt="Screenshot 2024-10-21 at 11 25 53 PM" src="https://github.com/user-attachments/assets/2ad9f1e9-9e3a-4947-8127-ef5df917411e">
<img width="1440" alt="Screenshot 2024-10-21 at 11 26 10 PM" src="https://github.com/user-attachments/assets/425de2e8-b1e9-42b4-b8e6-52dc0fdd9073">
<img width="1440" alt="Screenshot 2024-10-21 at 11 26 23 PM" src="https://github.com/user-attachments/assets/dba2a189-472e-4403-8347-1a9fd61a3ec0">


**Example 3:**

Verilog code:

```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 3 input AND gate.

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check3_net.v
!gedit opt_check3_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 29 24 PM" src="https://github.com/user-attachments/assets/96f9ad9d-1e28-4043-a6f5-385613fcfe5d">
<img width="1440" alt="Screenshot 2024-10-21 at 11 29 45 PM" src="https://github.com/user-attachments/assets/b712e088-1b2e-478d-a674-bbede3d9cedb">
<img width="1440" alt="Screenshot 2024-10-21 at 11 29 59 PM" src="https://github.com/user-attachments/assets/58d4a28e-ba14-4c9e-9575-9211dba5b0e0">




**Example 4:**

Verilog code:

```
module opt_check4 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 2 input XNOR gate.

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v
synth -top opt_check4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check4_net.v
!gedit opt_check4_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 31 13 PM" src="https://github.com/user-attachments/assets/e9da858c-a090-4df5-a6fb-106ff984e13e">
<img width="1440" alt="Screenshot 2024-10-21 at 11 31 32 PM" src="https://github.com/user-attachments/assets/fd41e344-ceeb-4547-9aa8-951294381b5b">
<img width="1440" alt="Screenshot 2024-10-21 at 11 31 49 PM" src="https://github.com/user-attachments/assets/42c72335-aa17-40a7-bc8d-5ce9f2f34b34">


**Example 5:**
Verilog code:

```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 

endmodule
```

On optimisation the above design becomes a AND OR gate

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt_net.v
!gedit multiple_module_opt_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 34 07 PM" src="https://github.com/user-attachments/assets/d4828f92-2fdc-445d-8a77-01381715edc8">
<img width="1440" alt="Screenshot 2024-10-21 at 11 34 44 PM" src="https://github.com/user-attachments/assets/39e9ba2a-d6fb-4b94-8952-d954a2d1725d">
<img width="1440" alt="Screenshot 2024-10-21 at 11 34 58 PM" src="https://github.com/user-attachments/assets/f91aaf7c-7832-4329-94fd-caa1b5d03a71">



**Example 6:**

Verilog code:

```
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule
```

On optimisation the above design becomes Y=0 

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt2.v
synth -top multiple_module_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt2_net.v
!gedit multiple_module_opt2_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 35 28 PM" src="https://github.com/user-attachments/assets/4a4ad34f-329b-4e43-9704-51ea88c8dbac">
<img width="1440" alt="Screenshot 2024-10-21 at 11 38 15 PM" src="https://github.com/user-attachments/assets/b80afba3-3c62-4606-b7a8-147c1173c92b">

<img width="1440" alt="Screenshot 2024-10-21 at 11 36 09 PM" src="https://github.com/user-attachments/assets/7c837dd3-214d-4f50-9e1c-4e56fa9e85d9">
<img width="1440" alt="Screenshot 2024-10-21 at 11 36 11 PM" src="https://github.com/user-attachments/assets/bb0d744e-8c5c-4ed2-9c51-ed6e1715d06d">



**Sequential Logic Optimizations**

**Example 1:**

Verilog code:

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const1_net.v
!gedit dff_const1_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 44 05 PM" src="https://github.com/user-attachments/assets/07a812d5-3a41-4f50-b74c-032707358075">
<img width="1440" alt="Screenshot 2024-10-21 at 11 44 44 PM" src="https://github.com/user-attachments/assets/c6f72d08-5f3a-49eb-a0c9-5054b90fafe4">
<img width="1440" alt="Screenshot 2024-10-21 at 11 44 59 PM" src="https://github.com/user-attachments/assets/3773fa53-a74c-46ff-973c-3eb8e910c989">



GTKWave Output:

```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 46 04 PM" src="https://github.com/user-attachments/assets/dc41716a-c8b3-4739-b493-f9d7465329a6">


<img width="1440" alt="Screenshot 2024-10-21 at 11 46 01 PM" src="https://github.com/user-attachments/assets/5c63f175-c2a7-4677-965a-7b2727bf4692">





**Example 2:**

Verilog code:

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const2_net.v
!gedit dff_const2_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 46 51 PM" src="https://github.com/user-attachments/assets/319bed6d-2b62-4396-a190-e1a710e20c38">
<img width="1440" alt="Screenshot 2024-10-21 at 11 47 13 PM" src="https://github.com/user-attachments/assets/ccb77e20-ad82-420f-824a-dd7caed02f6c">
<img width="1440" alt="Screenshot 2024-10-21 at 11 47 23 PM" src="https://github.com/user-attachments/assets/8ee9c16b-44f4-4b51-8450-a6cc3527353c">


GTKWave Output:

```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 48 05 PM" src="https://github.com/user-attachments/assets/0097275e-b53e-43e1-8e2d-175e4fe8ca87">

<img width="1440" alt="Screenshot 2024-10-21 at 11 48 01 PM" src="https://github.com/user-attachments/assets/2c2c8a34-ce08-43fe-95bf-fa12e2be16bb">



**Example 3:**

Verilog code:

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const3_net.v
!gedit dff_const3_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 48 47 PM" src="https://github.com/user-attachments/assets/674f301d-f685-4f8c-87ae-b14c69ea7d6e">
<img width="1440" alt="Screenshot 2024-10-21 at 11 49 05 PM" src="https://github.com/user-attachments/assets/7cafcb21-80c2-40c6-9a90-2befaceac4e2">
<img width="1440" alt="Screenshot 2024-10-21 at 11 49 15 PM" src="https://github.com/user-attachments/assets/f0157e94-a5c6-432c-9f6d-242f17ac9e0b">



GTKWave Output:

```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 50 48 PM" src="https://github.com/user-attachments/assets/528e0e5a-3701-4268-a449-0f5b47998d2a">

<img width="1440" alt="Screenshot 2024-10-21 at 11 49 48 PM" src="https://github.com/user-attachments/assets/638be51b-217c-4df1-9389-46664504812d">



**Example 4:**

Verilog code:

```
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const4_net.v
!gedit dff_const4_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 51 26 PM" src="https://github.com/user-attachments/assets/dc9f5e9a-e84a-4997-b8bc-de33f495561b">
<img width="1440" alt="Screenshot 2024-10-21 at 11 51 47 PM" src="https://github.com/user-attachments/assets/b57db890-8178-47af-93e5-e40b2e489623">
<img width="1440" alt="Screenshot 2024-10-21 at 11 52 10 PM" src="https://github.com/user-attachments/assets/41ef9cee-15f7-4110-aed8-8d3147ccdf3f">


 
GTKWave Output:

```
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave tb_dff_const4.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 53 39 PM" src="https://github.com/user-attachments/assets/1265f73e-4fa9-4332-b093-c50da090aa77">
<img width="1440" alt="Screenshot 2024-10-21 at 11 53 36 PM" src="https://github.com/user-attachments/assets/c645b33d-d9f7-442c-94d9-a97badea21ca">



**Example 5:**

Verilog code:

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
	else
		begin
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const5_net.v
!gedit dff_const5_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 54 44 PM" src="https://github.com/user-attachments/assets/9963c7c2-8235-4dfc-9d9e-33d27b663f84">
<img width="1440" alt="Screenshot 2024-10-21 at 11 55 02 PM" src="https://github.com/user-attachments/assets/54d98495-6414-4289-b84f-d6cb2369a243">
<img width="1440" alt="Screenshot 2024-10-21 at 11 55 14 PM" src="https://github.com/user-attachments/assets/d8b64c12-138a-462a-af60-13ea808af710">



GTKWave Output:

```
iverilog dff_const5.v tb_dff_const5.v
./a.out
gtkwave tb_dff_const5.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 56 26 PM" src="https://github.com/user-attachments/assets/9c36a5e9-14d7-46b8-87d7-4313a04c29e5">
<img width="1440" alt="Screenshot 2024-10-21 at 11 56 22 PM" src="https://github.com/user-attachments/assets/95ef85eb-206f-4b1e-9d91-4f10b6a1beba">


**Sequential Logic Optimizations for unused outputs**

**Example 1:**

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt_net.v
!gedit counter_opt_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 59 44 PM" src="https://github.com/user-attachments/assets/aecac502-9930-492f-a3bb-4a094c083710">
<img width="1440" alt="Screenshot 2024-10-22 at 12 00 06 AM" src="https://github.com/user-attachments/assets/54244b6a-304a-42a6-9578-bda5f4f62a7d">
<img width="1440" alt="Screenshot 2024-10-22 at 12 00 17 AM" src="https://github.com/user-attachments/assets/6a4434b6-b848-4d1c-ad56-a41c1ed15dde">


GTKWave Output:

```
iverilog counter_opt.v tb_counter_opt.v
./a.out
gtkwave tb_counter_opt.vcd
```
<img width="1440" alt="Screenshot 2024-10-22 at 12 01 21 AM" src="https://github.com/user-attachments/assets/2f767ea5-7ac7-4314-80aa-b6a016bf4ce3">
<img width="1440" alt="Screenshot 2024-10-22 at 12 01 18 AM" src="https://github.com/user-attachments/assets/32a5f91b-8fb2-4f08-86a4-5b17d3af0aea">



Modified counter logic:

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```
Run the below code for netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt2_net.v
!gedit counter_opt2_net.v
```
<img width="1440" alt="Screenshot 2024-10-22 at 12 06 19 AM" src="https://github.com/user-attachments/assets/8ab9cb63-b311-4c39-bbfc-625fbe966c34">
<img width="1440" alt="Screenshot 2024-10-22 at 12 06 40 AM" src="https://github.com/user-attachments/assets/ce6239e6-b970-43fd-b765-68c7f431e8d8">
<img width="1440" alt="Screenshot 2024-10-22 at 12 07 12 AM" src="https://github.com/user-attachments/assets/88f6db2e-e307-417a-865e-73ca006fa7a5">
<img width="1440" alt="Screenshot 2024-10-22 at 12 07 14 AM" src="https://github.com/user-attachments/assets/83a96156-460d-4552-be12-362e3788c604">



GTKWave Output:

```
iverilog counter_opt2.v tb_counter_opt.v
./a.out
gtkwave tb_counter_opt.vcd
```

<img width="1440" alt="Screenshot 2024-10-22 at 12 09 41 AM" src="https://github.com/user-attachments/assets/a7fc338b-a20e-401f-ab17-a4b33f196a25">

<img width="1440" alt="Screenshot 2024-10-22 at 12 09 37 AM" src="https://github.com/user-attachments/assets/1529153a-cbb6-422f-bfe6-8f1b119f54b0">
<img width="1440" alt="Screenshot 2024-10-22 at 12 09 22 AM" src="https://github.com/user-attachments/assets/43b6c197-df51-4a2f-9b48-d4274b85cc4d">





</details>
<details>
<summary><strong>Day 4:</strong> GLS, blocking vs non-blocking and Synthesis-Simulation mismatch.</summary>

### GLS

Gate Level Simulation (GLS) is a crucial step in the verification process of digital circuits. It involves simulating the synthesized netlist, which is a lower-level representation of the design, using a testbench to verify its logical correctness and timing behavior. By comparing the simulated outputs to the expected outputs, GLS ensures that the synthesis process has not introduced any errors and that the design meets its performance requirements.

<img width="817" alt="Screenshot 2024-10-21 at 10 47 10 PM" src="https://github.com/user-attachments/assets/8b0854f1-bfaf-428f-a77d-212eb02bc802">

Sensitivity lists are vital for ensuring correct circuit behavior. An incomplete sensitivity list can result in unintended latches. The execution behavior of blocking and non-blocking assignments in always blocks differs. Misusing blocking assignments may inadvertently generate latches, leading to mismatches between synthesis and simulation. To prevent these problems, it's important to thoroughly assess circuit behavior and verify that both the sensitivity list and assignments correspond to the intended functionality.

**GLS Simulation**

**Example 1:**

Verilog code:

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```



```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 51 15 PM" src="https://github.com/user-attachments/assets/8f18c09c-a00f-4ada-b8b7-d215599f0e2d">

Simulation:
<img width="1440" alt="Screenshot 2024-10-21 at 10 50 07 PM" src="https://github.com/user-attachments/assets/e3a49915-50d4-484a-85c1-9ce745906fac">


Netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr ternary_operator_mux_net.v
!gedit ternary_operator_mux_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 52 29 PM" src="https://github.com/user-attachments/assets/45260a6a-d884-4574-9776-50b2ef2cba6b">
<img width="1440" alt="Screenshot 2024-10-21 at 10 52 45 PM" src="https://github.com/user-attachments/assets/12e99d0e-9e63-4de0-9a7d-cf54d2a5b5ca">
<img width="1440" alt="Screenshot 2024-10-21 at 10 53 29 PM" src="https://github.com/user-attachments/assets/386d3eeb-297e-49ec-bddb-183173c8a776">

GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 55 32 PM" src="https://github.com/user-attachments/assets/f285722e-25c1-401b-ab38-7519c9477568">

**Simulation:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 55 28 PM" src="https://github.com/user-attachments/assets/04fe342b-f553-4344-86e3-dac29e99c734">


In this case there is no mismatch between the waveforms before and after synthesis



**Example 2:**

Verilog code:

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```



```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 10 58 48 PM" src="https://github.com/user-attachments/assets/a3661103-24a3-49ff-9c36-b49feaa51857">

**Simulation:**
<img width="1440" alt="Screenshot 2024-10-21 at 10 58 45 PM" src="https://github.com/user-attachments/assets/4ee6bd4a-b040-4b1d-9d6e-a65ea821d10d">


Netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr bad_mux_net.v
!gedit bad_mux_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 00 12 PM" src="https://github.com/user-attachments/assets/4d1358c6-61d0-4823-a59a-a011449f39b3">
<img width="1440" alt="Screenshot 2024-10-21 at 11 00 27 PM" src="https://github.com/user-attachments/assets/afe3c734-f5f8-457c-888b-cf129734c8e1">
<img width="1440" alt="Screenshot 2024-10-21 at 11 00 49 PM" src="https://github.com/user-attachments/assets/2f6ec283-420e-439a-8f01-d4fcdc4d0785">


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 02 04 PM" src="https://github.com/user-attachments/assets/23a5fb9d-c9bb-4502-a7b7-05bac7786365">
<img width="1440" alt="Screenshot 2024-10-21 at 11 02 01 PM" src="https://github.com/user-attachments/assets/fb4aa994-a037-44cc-b093-582a276e8c9b">



In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the sensitivity list error.


**Labs on Synthesis-Simulation mismatch for blocking statements**

Verilog code:

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
d = x & c;
x = a | b;
end
endmodule
```

Simulation:

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 05 09 PM" src="https://github.com/user-attachments/assets/76874de9-72ca-4207-9b95-9a672f78262c">
<img width="1440" alt="Screenshot 2024-10-21 at 11 05 06 PM" src="https://github.com/user-attachments/assets/4be45286-34ca-4cf5-b5ce-86b769dd03d9">



Netlist:

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr blocking_caveat_net.v
gedit blocking_caveat_net.v
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 05 45 PM" src="https://github.com/user-attachments/assets/e9f95aba-730d-468a-871d-badc73674e18">

<img width="1440" alt="Screenshot 2024-10-21 at 11 05 59 PM" src="https://github.com/user-attachments/assets/0282266f-b884-4e38-8f1c-d68aebc7e83e">
<img width="1440" alt="Screenshot 2024-10-21 at 11 06 17 PM" src="https://github.com/user-attachments/assets/eb9790d8-d181-4f3d-9ab2-e98f6a578703">


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
<img width="1440" alt="Screenshot 2024-10-21 at 11 07 14 PM" src="https://github.com/user-attachments/assets/48ca08db-5d36-47e6-ab3e-39f59a3584bb">


<img width="1440" alt="Screenshot 2024-10-21 at 11 07 11 PM" src="https://github.com/user-attachments/assets/eb9eaa84-faa3-4bc0-9d71-46fb11f3d886">



In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the latch error.



</details>
</details>

<details>
<summary><strong>Lab 10:</strong> Synthesize RISC-V and compare output with functional simulations. </summary>

### To Synthesize RISC-V and compare output with functional simulations


##### Post-Synthesis

Steps:
1. Copy the src folder from your VSDBabySoC folder to your VLSI folder.
2.  Go the required Directory:

```
cd /home/sarl-lab/Desktop/Divyansh/VLSI/vsdflow/sky130RTLDesignAndSynthesisWorkshop/src/module
```
<img width="1440" alt="Screenshot 2024-10-23 at 8 53 07 PM" src="https://github.com/user-attachments/assets/1fd8e739-38ea-4f5b-868b-00448e11e2b8">
<img width="1440" alt="Screenshot 2024-10-23 at 8 55 23 PM" src="https://github.com/user-attachments/assets/8b557f20-a92f-4274-94ac-513470f25c9d">


There are two ways:

#### Method-1
1. Now run these command in the VSDBabySOC folder to get output
   

```
make post_synth_sim
gtkwave output/post_synth_sim/post_synth_sim.vcd
```
<img width="1440" alt="Screenshot 2024-10-23 at 11 23 58 PM" src="https://github.com/user-attachments/assets/4559e770-8561-46ed-bf63-cf30f530f906">
<img width="1440" alt="Screenshot 2024-10-23 at 11 24 01 PM" src="https://github.com/user-attachments/assets/bee553d0-76e1-4c95-8f34-26202f8e90e3">
<img width="1440" alt="Screenshot 2024-10-23 at 11 27 56 PM" src="https://github.com/user-attachments/assets/36884ca5-08ae-4483-ba7c-ba99335c9886">
<img width="1440" alt="Screenshot 2024-10-23 at 11 43 27 PM" src="https://github.com/user-attachments/assets/c4acaf25-5cc4-4029-b163-8499d97a548e">
<img width="1440" alt="Screenshot 2024-10-23 at 11 43 36 PM" src="https://github.com/user-attachments/assets/ea88c2e6-3337-44b9-807a-d2419c095a49">

Simulations:

<img width="1440" alt="Screenshot 2024-10-23 at 11 50 00 PM" src="https://github.com/user-attachments/assets/d0ab80a3-fcc7-4180-9ddd-4e11f30e00ef">
<img width="1440" alt="Screenshot 2024-10-23 at 11 43 27 PM" src="https://github.com/user-attachments/assets/3da2bb1b-3a1c-42ae-be8d-be9ad0d41dcf">
<img width="1440" alt="Screenshot 2024-10-23 at 11 43 36 PM" src="https://github.com/user-attachments/assets/9228f1e6-90ac-4195-93e0-4f070ab181f1">


#### Method-2
1. Using Yosys

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog clk_gate.v
read_verilog rvmyth.v
synth -top rvmyth
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr rvmyth_net.v
!gedit rvmyth_net.v
exit

```
<img width="1440" alt="Screenshot 2024-10-24 at 4 45 13 AM" src="https://github.com/user-attachments/assets/541742c8-0e61-46e4-8c2a-7018306b43cf">



**Netlist:**
<img width="1440" alt="Screenshot 2024-10-24 at 4 53 40 AM" src="https://github.com/user-attachments/assets/e8af9e04-65ee-4abf-9c3a-beb3e0ec5010">


**Simulation:**

```
iverilog ../../my_lib/verilog_model/primitives.v ../../my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v
./a.out
gtkwave dump.vcd
```
![WhatsApp Image 2024-10-24 at 04 39 47](https://github.com/user-attachments/assets/fbee526a-3884-4ece-87fb-1f660e70e62b)



**Netlist of VSDBabySOC**
Command
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib ../lib/avsddac.lib
read_liberty -lib ../lib/avsdpll.lib 
read_verilog vsdbabysoc.v
read_verilog clk_gate.v
read_verilog rvmyth.v
synth -top rvmyth
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
select vsdbabysoc
show
```
<img width="1440" alt="Screenshot 2024-10-24 at 4 48 29 AM" src="https://github.com/user-attachments/assets/11ca1002-06c9-4c6a-92cd-a8f372444671">
<img width="1440" alt="Screenshot 2024-10-24 at 4 48 31 AM" src="https://github.com/user-attachments/assets/731420e8-5a9f-41ba-b3a2-c19b06256076">
<img width="1440" alt="Screenshot 2024-10-24 at 4 49 35 AM" src="https://github.com/user-attachments/assets/f7634fd6-505e-4f3a-b3f3-2cfed488b309">



Comparison by taking example of 7056:
<img width="1440" alt="Screenshot 2024-10-24 at 2 20 20 AM" src="https://github.com/user-attachments/assets/a852f568-4ad0-48e6-bd0d-9c9a6944ef85">
<img width="1378" alt="Screenshot 2024-10-24 at 2 21 12 AM" src="https://github.com/user-attachments/assets/7223940c-52af-461e-a21e-f35d9b234ac9">



##### Pre-Synthesis (Functional Simulation)

**Steps:**

```
cd ~
cd VSDBabySoC
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```
<img width="1440" alt="Screenshot 2024-10-23 at 11 48 26 PM" src="https://github.com/user-attachments/assets/d5fd51de-3b95-434b-a6d2-a42c8c778728">
<img width="1440" alt="Screenshot 2024-10-23 at 11 45 26 PM" src="https://github.com/user-attachments/assets/015f6f08-07a2-4c1b-ac88-cf85281483dc">
<img width="1440" alt="Screenshot 2024-10-23 at 11 45 34 PM" src="https://github.com/user-attachments/assets/2f4d3638-7969-4ce4-8450-c74b685e4b32">




</details>
<details>
<summary><strong>Lab 11:</strong> Post Synthesis Static Timing Analysis using OpenSTA.</summary>
	
The contents of VSDBabySoc/src/sdc/vsdbabysoc_synthesis.sdc.
Change the contents as shown:
```
set PERIOD 11
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

<img width="1440" alt="Screenshot 2024-10-28 at 10 23 14 PM" src="https://github.com/user-attachments/assets/2965241f-32f9-45e2-9cfb-629b10731092">
<img width="1440" alt="Screenshot 2024-10-28 at 10 23 38 PM" src="https://github.com/user-attachments/assets/c722694d-761b-455e-b39d-0122ac3156a0">
<img width="1440" alt="Screenshot 2024-10-28 at 11 24 00 PM" src="https://github.com/user-attachments/assets/fd7f3145-2eb9-4025-9c91-8772c2575355">

Now, run the below commands:

```
cd VSDBabySoc/src
sta
read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min ./lib/avsdpll.lib
read_liberty -min ./lib/avsddac.lib
read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max ./lib/avsdpll.lib
read_liberty -max ./lib/avsddac.lib
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```




Hold:

<img width="706" alt="Screenshot 2024-10-28 at 11 29 14 PM" src="https://github.com/user-attachments/assets/315d0db1-9cba-4bf1-be6c-983a4e3d10d3">



Setup:

<img width="610" alt="Screenshot 2024-10-28 at 11 29 20 PM" src="https://github.com/user-attachments/assets/34780b25-0557-4fec-8bad-824e2c7ae1f4">


</details>


<details>
<summary><strong>Lab 12:</strong>Post Synthesis Static Timing Analysis using OpenSTA for all the sky130 lib files. </summary>

Snapshot of the sdc file vsdbabysoc_synthesis.sdc:
```
set PERIOD 11
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

<img width="1440" alt="Screenshot 2024-10-30 at 9 48 12 PM" src="https://github.com/user-attachments/assets/b25be6fe-5057-42d4-b8c7-f87400a38ded">


Store all the `lib` files in a folder named `timing_libs` and keep it in the `VSDBabySoC/src` directory.

<img width="1440" alt="Screenshot 2024-10-30 at 10 03 48 PM" src="https://github.com/user-attachments/assets/b2032944-f860-4f28-8aa4-35e07fdea567">

Now create a `sta_pvt.tcl` file and copy the below content in that file using gedit.

```
gedit sta_pvt.tcl
```
```
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2"
set list_of_lib_files(9) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(17) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1"
set list_of_lib_files(18) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2"
set list_of_lib_files(19) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3"
set list_of_lib_files(20) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(21) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(22) "sky130_fd_sc_hd__tt_100C_1v80.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing_libs/$list_of_lib_files($i)
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

}
```
<img width="1440" alt="Screenshot 2024-10-30 at 9 46 51 PM" src="https://github.com/user-attachments/assets/39c26710-f929-40e8-bc5b-615fed7d803e">
<img width="1440" alt="Screenshot 2024-10-30 at 9 47 13 PM" src="https://github.com/user-attachments/assets/492fc315-715f-438a-bae6-bd89d6ebab57">


Now, run the following commands:

```
cd VSDBabySoC/src
sta
source sta_pvt.tcl
```

<img width="1440" alt="Screenshot 2024-10-30 at 9 56 05 PM" src="https://github.com/user-attachments/assets/f39cd591-9419-4bbe-ba9c-d77fe87bfbc8">

Before that create a folder named `sta_output` in `VSDBabySoC/src` 

<img width="1440" alt="Screenshot 2024-10-30 at 9 56 12 PM" src="https://github.com/user-attachments/assets/c36955f4-d8ff-46da-8815-b5062bd1c018">


These `.txt` file will be generated as shown along with a sample file.

<img width="1440" alt="Screenshot 2024-10-30 at 9 56 15 PM" src="https://github.com/user-attachments/assets/b122eced-c51f-46f7-b766-59597df7e242">
<img width="1440" alt="Screenshot 2024-10-30 at 9 58 09 PM" src="https://github.com/user-attachments/assets/9fbd1f07-0943-46a0-ad79-bcc12d8e7eb6">


Now put the values in excel and plot the graphs as shown:

<img width="713" alt="Screenshot 2024-10-30 at 10 44 41 PM" src="https://github.com/user-attachments/assets/1fa2bcbf-178f-41cb-b9cf-4cb20d842ef9">

<img width="817" alt="Screenshot 2024-10-30 at 10 52 16 PM" src="https://github.com/user-attachments/assets/5c88adbb-ecc1-44d0-abc5-e23e41243381">

<img width="816" alt="Screenshot 2024-10-30 at 10 52 26 PM" src="https://github.com/user-attachments/assets/a2413f58-ddfa-405f-ad96-d3bd50b1477f">


</details>


<details>
<summary><strong>Lab 13:</strong> Advanced Physical Design using OpenLane using Sky130.</summary>
---
<details>
<summary><strong>Day-1:</strong> Open-source EDA, OpenLane and Sky130 PDK</summary>

**QFN-48 Package:** A Quad Flat No-leads (QFN) 48 package is a leadless IC package with 48 connection pads around the perimeter. It offers good thermal and electrical performance in a compact form, making it ideal for high-density applications.

<img width="821" alt="Screenshot 2024-11-13 at 3 52 27 PM" src="https://github.com/user-attachments/assets/17b934d8-ca72-49fb-b2af-2464eaf37a7f">


**Chip:** An integrated circuit (IC) that contains various functional blocks like memory, processing units, and I/O in a silicon substrate, typically used for specific applications in electronics.
<img width="645" alt="Screenshot 2024-11-13 at 3 55 28 PM" src="https://github.com/user-attachments/assets/aa806ec2-ab76-45f9-9405-d570cd237f2e">




**Pads:** Small metallic areas on a chip or package used to connect internal circuitry to external connections, enabling signals to be transferred to and from the IC.

**Core:** The central part of a chip containing the main processing unit and functional logic, often optimized for power and performance.

**Die:** The section of a silicon wafer containing an individual IC before it is packaged, housing all active circuits and elements for the chip's functions.
<img width="650" alt="Screenshot 2024-11-13 at 3 55 39 PM" src="https://github.com/user-attachments/assets/7289f604-ed43-4eef-98a9-bc8552b98bf0">




**IPs (Intellectual Properties):** Pre-designed functional blocks or modules within a chip, such as USB controllers or memory interfaces, licensed for reuse across various designs to save time and cost.
<img width="644" alt="Screenshot 2024-11-13 at 3 55 48 PM" src="https://github.com/user-attachments/assets/851cfe52-80e7-428c-b6cc-01d4174c66d3">

**From Software Applications to Hardware Flow**

To run an application on hardware, several processes take place. First, the application enters a layer known as the system software, which prepares it for execution by translating the application program into binary format, understandable by hardware. Key components within system software include the Operating System (OS), Compiler, and Assembler.

The process starts with the OS, which breaks down application functions written in high-level languages such as C, C++, Java, or Visual Basic. These functions are passed to a suitable compiler, which translates them into low-level instructions. The syntax and format of these instructions are tailored to the specific hardware architecture in use.

Next, the assembler converts these hardware-specific instructions into binary format, known as machine language. This binary code is then fed to the hardware, enabling it to perform specific tasks as defined by the received instructions.
<img width="662" alt="Screenshot 2024-11-13 at 3 57 07 PM" src="https://github.com/user-attachments/assets/a1d3dfc0-23a0-468b-aad7-5558d0d7d05a">



For example, consider a stopwatch app running on a RISC-V core. Here, the OS might generate a small function in C, which is then passed to a compiler. The compiler outputs RISC-V-specific instructions, tailored to the architecture. These instructions are subsequently processed by the assembler, which converts them into binary code. This binary code then flows into the chip layout, where the hardware executes the desired functionality.
<img width="661" alt="Screenshot 2024-11-13 at 3 57 14 PM" src="https://github.com/user-attachments/assets/1585bf59-e863-4d91-a54a-642baed146be">



For the above stopwatch the below figure shows the input and output of the compiler and assembler.

<img width="655" alt="Screenshot 2024-11-13 at 3 57 20 PM" src="https://github.com/user-attachments/assets/fbf8a58a-4527-4092-893c-0e1bcfcb22eb">


The compiler generates architecture-specific instructions, while the assembler produces the corresponding binary patterns. To execute these instructions on hardware, an RTL (written in a Hardware Description Language) is used to interpret and implement the instructions. This RTL design is then synthesized into a netlist, represented as interconnected logic gates. Finally, the netlist undergoes physical design implementation to be fabricated onto the chip.

<img width="659" alt="Screenshot 2024-11-13 at 3 57 27 PM" src="https://github.com/user-attachments/assets/ed43e41d-380b-4961-a62d-c4e1afd8dd42">


**Components of ASIC Design**

<img width="663" alt="Screenshot 2024-11-13 at 3 57 34 PM" src="https://github.com/user-attachments/assets/da897293-8eed-4036-a07b-a6106ed316ca">


- RTL IPs: Pre-designed, verified digital circuit blocks (like adders, flip-flops, memory) in HDL (e.g., Verilog, VHDL). They save design time by providing ready-to-use components for complex circuits.

- EDA Tools: Software that automates ASIC design tasks (e.g., synthesis, optimization, placement, timing analysis). Essential for improving productivity and ensuring performance and power requirements are met.

- PDK Data: A set of files and parameters from a semiconductor foundry, detailing its manufacturing process (e.g., transistor models, design rules). PDKs ensure ASIC designs are compatible with the foundry’s fabrication process.

**Simplified RTL to GDS flow**

<img width="670" alt="Screenshot 2024-11-13 at 3 57 44 PM" src="https://github.com/user-attachments/assets/892b1ed5-a4d0-40ab-aa4c-398248be21a1">

- **RTL Design:** Describes the circuit's functional behavior using HDLs like Verilog or VHDL, defining its logic and data paths.

- **RTL Synthesis:** Converts RTL code to a gate-level netlist which is a collection of standard cells like AND gates, flip-flops, and multiplexers by mapping it to standard cells and optimizing for area, power, and timing. 
- **Floor and Power Planning:** Partitions chip area, places major components, and defines power grid and I/O placement to optimize area, power distribution, and signal flow. This step optimizes the physical layout, aiming to reduce power consumption and improve signal integrity by considering the placement of I/O pads and power distribution cells

- **Placement:** Assigns physical locations to cells, aiming to minimize wirelength, reduce signal delay, and meet design constraints. The placement tool carefully arranges the cells to balance the overall chip design for optimal performance and area utilization.

- **Clock Tree Synthesis (CTS):** Clock Tree Synthesis (CTS) is a critical step that focuses on creating an optimized clock distribution network. CTS ensures the clock is distributed evenly to all flip-flops and registers. It builds an optimized clock network to balance clock signal distribution and reduce clock skew.

- **Routing:** Connects components based on placement, optimizing wire paths to ensure signal integrity, minimize congestion, and meet design rules.

- **Sign-off:** Final verification stage, ensuring the design meets functionality, performance, power, and reliability targets. Timing analysis is performed to check setup and hold times, power analysis ensures the design doesn’t exceed power limits, and physical verification checks ensure that the layout meets manufacturing rules. This stage confirms the design is ready for fabrication.

- **GDSII File Generation:** Creates the GDSII file containing the complete layout details needed for chip fabrication. This file represents the final physical design and is used by manufacturers to create the photomasks required for chip production. The GDSII file serves as the blueprint for the actual fabrication of the chip.

**OpenLane ASIC Flow:**

<img width="699" alt="Screenshot 2024-11-13 at 4 00 39 PM" src="https://github.com/user-attachments/assets/7bad05ff-0884-434a-b49d-04ea50802ad9">



1. RTL Synthesis, Technology Mapping, and Formal Verification: The tools used are Yosys (for RTL synthesis), ABC (for technology mapping and formal verification).
2. Static Timing Analysis: The tools used are OpenSTA (for static timing analysis).
3. Floor Planning: The tools used are init_fp (initial floorplanning), ioPlacer (I/O placement), pdn (power distribution network planning), tapcell (tap cell insertion).
4. Placement: The tools used are RePLace (global placement), Resizer (optional for resizing cells), OpenPhySyn (formerly used for placement), OpenDP (detailed placement).
5. Clock Tree Synthesis: The tools used are TritonCTS (for clock tree synthesis).
6. Fill Insertion: The tools used are OpenDP (for filler placement).
7. Routing: The tools used for global routing are FastRoute or CU-GR (formerly used) and for the detailed routing , we use TritonRoute (for detailed routing) or DR-CU (formerly used).
8. SPEF Extraction: The tools used are OpenRCX (or SPEF-Extractor, formerly used) for Standard Parasitic Exchange Format (SPEF) extraction.
9. GDSII Streaming Out: The tools used are Magic and KLayout (for viewing and editing GDSII files).
10. Design Rule Checking (DRC) Checks: The tools used are Magic and KLayout (for DRC checks).
11. Layout vs. Schematic (LVS) Check: The tools used are Netgen (for LVS checks).
12. Antenna Checks: The tools used are Magic (for antenna checks).

**OpenLANE Directory structure**

``` 
├── OOpenLane             -> directory where the tool can be invoked (run docker first)
│   ├── designs          -> All designs must be extracted from this folder
│   │   │   ├── picorv32a -> Design used as case study for this workshop
│   |   |   ├── ...
|   |   ├── ...
├── pdks                 -> contains pdk related files 
│   ├── skywater-pdk     -> all Skywater 130nm PDKs
│   ├── open-pdks        -> contains scripts that makes the commerical PDK (which is normally just compatible to commercial tools) to also be compatible with the open-source EDA tool
│   ├── sky130A          -> pdk variant made especially compatible for open-source tools
│   │   │  ├── libs.ref  -> files specific to node process (timing lib, cell lef, tech lef) for example is `sky130_fd_sc_hd` (Sky130nm Foundry Standard Cell High Density)  
│   │   │  ├── libs.tech -> files specific for the tool (klayout,netgen,magic...) 
```

**Synthesis in Openlane:**

Go to VSD Virtual Box and run the following commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis

```
<img width="1440" alt="Screenshot 2024-11-13 at 4 09 09 PM" src="https://github.com/user-attachments/assets/92ee2484-6c81-4135-933e-5d1fb1f03e6b">
<img width="1440" alt="Screenshot 2024-11-13 at 4 09 48 PM" src="https://github.com/user-attachments/assets/b7c0a5f1-bf81-4edf-bd9c-d4b6c5fb2188">



To view the netlist:

```
cd designs/picorv32a/runs/09-11_06-33/results/synthesis/
gedit picorv32a.synthesis.v
```

<img width="1440" alt="Screenshot 2024-11-13 at 4 11 57 PM" src="https://github.com/user-attachments/assets/2296e1a7-fa2f-423c-a7f6-0e871a40af99">


Netlist code:

<img width="1440" alt="Screenshot 2024-11-13 at 4 13 26 PM" src="https://github.com/user-attachments/assets/cd13744d-8ddd-44aa-8a20-0e20611be1b2">


To view the yosys report:

```
cd ../..
cd reports/synthesis
gedit 1-yosys_4.stat.rpt
```

<img width="1440" alt="Screenshot 2024-11-13 at 4 15 19 PM" src="https://github.com/user-attachments/assets/eb522d2d-2475-4dcd-84aa-2e4f9f9d70d7">


Report:

```
28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
 sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400
```

```
Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876
```
 
</details>




<details>

 
<summary><strong>Day-2:</strong> Good floorplan vs bad floorplan and introduction to library cells</summary>

**Utilization Factor and Aspect Ratio**: In IC floor planning, utilization factor and aspect ratio are key parameters. The utilization factor is the ratio of the area occupied by the netlist to the total core area. While a perfect utilization of 1 (100%) is ideal, practical designs target a factor of 0.5 to 0.6 to allow space for buffer zones, routing channels, and future adjustments. The aspect ratio, defined as height divided by width, indicates the chip’s shape; an aspect ratio of 1 denotes a square, while other values result in a rectangular layout. The aspect ratio is chosen based on functional, packaging, and manufacturing needs.

```
Utilisation Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
```

**Pre-placed cells** : Pre-placed cells are essential functional blocks, such as memory, custom processors, and analog circuits, positioned manually in fixed locations. These blocks are crucial for the chip’s performance and remain fixed during placement and routing to preserve their functionality and layout integrity.

**Decoupling Capacitors** : Decoupling capacitors are placed near logic circuits to stabilize power supply voltages during transient events. Acting as local energy reserves, they help reduce voltage fluctuations, crosstalk, and electromagnetic interference (EMI), ensuring reliable power delivery to sensitive circuits.

**Power Planning**: A robust power planning strategy includes creating a power and ground mesh to distribute VDD and VSS evenly across the chip. This setup ensures stable power delivery, minimizes voltage drops, and improves overall efficiency. Multiple power and ground points reduce the risk of instability and voltage drop issues, supporting the design’s power needs effectively.

**Pin Placement**: Pin placement (I/O planning) is crucial for functionality and reliability. Strategic pin assignment minimizes signal degradation, preserves data integrity, and helps manage heat dissipation. Proper positioning of power and ground pins supports thermal management and enhances signal strength, contributing to overall system stability and manufacturability.

Floorplaning using OpenLANE:

Run the following commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```
<img width="1440" alt="Screenshot 2024-11-13 at 4 29 25 PM" src="https://github.com/user-attachments/assets/d50f2b2a-baaf-40b9-83d5-ea66f985795b">



Now, run the below commands in a new terminal:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-11_07-10/results/floorplan
gedit picorv32a.floorplan.defpr 
```

1ls<img width="1440" alt="Screenshot 2024-11-13 at 4 34 32 PM" src="https://github.com/user-attachments/assets/c1b1cec6-9cd1-4d7e-bc7e-3e8af330fe63">

According to floorplan definition:

1000 Unit Distance = 1 Micron  

Die width in unit distance = 660685−0 = 660685 

Die height in unit distance = 671405−0 = 671405  

Distance in microns = Value in Unit Distance/1000  

​Die width in microns = 660685/1000 = 660.685 Microns  

Die height in microns = 671405/1000 = 671.405 Microns  

Area of die in microns = 660.685 × 671.405 = 443587.212425 Square Microns

To view the floorplan in magic. Open a new terminal and run the below commands:


```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

```
![Uploading Screenshot 2024-11-13 at 4.41.28 PM.png…]()


Decap and Tap Cells:

<img width="641" alt="Screenshot 2024-11-13 at 4 47 11 PM" src="https://github.com/user-attachments/assets/b46a7914-7674-4ffc-bad5-85c0d0f9ff0d">


Unplaces standard cells at origin:

<img width="642" alt="Screenshot 2024-11-13 at 4 47 17 PM" src="https://github.com/user-attachments/assets/af0e7044-a40f-40a7-80f0-e096d8da9f7e">


Command to run placement:

```
run_placement
```
<img width="1440" alt="Screenshot 2024-11-13 at 4 50 32 PM" src="https://github.com/user-attachments/assets/be3e3703-a79f-429d-bc3d-c0f810a1246d">


To view the placement in magic:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Uploading Screenshot 2024-11-13 at 10.10.12 PM.png…]()
<img width="1440" alt="Screenshot 2024-11-13 at 10 10 19 PM" src="https://github.com/user-attachments/assets/71dc121e-bff6-463f-849f-8e639d66e08e">



**Cell design and Characterization Flow**

Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. There is a typical cell design flow steps.

Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files

**Standard Cell Characterization Flow**

A typical standard cell characterization flow that is followed in the industry includes the following steps:

- Read in the models and tech files
- Read extracted spice Netlist
- Recognise behavior of the cells
- Read the subcircuits
- Attach power sources
- Apply stimulus to characterization setup
- Provide neccesary output capacitance loads
- Provide neccesary simulation commands
- Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.

**Timing parameters**

| Timing definition | Value |
|---|---|
| slew_low_rise_thr | 20% value |
| slew_high_rise_thr | 80% value |
| slew_low_fall_thr | 20% value |
| slew_high_fall_thr | 80% value |
| in_rise_thr | 50% value |
| in_fall_thr | 50% value |
| out_rise_thr | 50% value |
| out_fall_thr | 50% value |

**Propagation Delay**: It refers to the time it takes for a change in an input signal to reach 50% of its final value to produce a corresponding change in the output signal to reach 50% of its final value of a digital circuit.

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

**Transistion time**: The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```





</details>







<details>


 
<summary><strong>Day-3:</strong> Design library cell using Magic Layout and ngspice characterization</summary>

**CMOS inverter ngspice simulations**:

Creating a SPICE Deck for a CMOS Inverter Simulation

- Netlist Creation: Define the component connections (netlist) for a CMOS inverter circuit. Ensure each node is labeled appropriately for easy identification in the SPICE simulation. Typical nodes include input, output, ground, and supply nodes.
- Device Sizing: Specify the Width-to-Length (W/L) ratios for both the PMOS and NMOS transistors.For proper operation, the PMOS width should be larger than the NMOS width, usually 2x to 3x, to balance the drive strength
- Voltage Levels: Set gate and supply voltages, often in multiples of the transistor length. 
- Node Naming: Assign node names to each connection point around the components to clearly identify each element in the SPICE netlist (e.g., VDD, GND, IN, OUT). This helps SPICE recognize each component and simulate the circuit effectively.
  
<img width="658" alt="Screenshot 2024-11-13 at 4 55 17 PM" src="https://github.com/user-attachments/assets/ac3d7311-549c-4a35-aa4a-9d0d74047fe0">

```

**syntax for PMOS and NMOS desription**:

[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]

 ***simulation commands***

.op --- is the start of SPICE simulation operation where Vin sweeps from 0 to 2.5 with 0.5 steps
tsmc_025um_model.mod  ----  model file which contains the technological parameters for the 0.25um NMOS and PMOS 
```
Commands to simulate in SPICE:

```
source [filename].cir
run
setplot 
dc1 
plot out vs in 
```
<img width="642" alt="Screenshot 2024-11-13 at 4 56 16 PM" src="https://github.com/user-attachments/assets/ddb1db7a-4ebc-4e8b-b6fb-56815eebba58">
<img width="664" alt="Screenshot 2024-11-13 at 10 06 16 PM" src="https://github.com/user-attachments/assets/240adbe6-5621-4793-8e68-ba40d0ce6b89">

The switching threshold Vm is like a critical voltage level for a component called a CMOS inverter. It's the point at which this inverter switches between sending out a "0" or a "1" in a computer chip. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.

At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing directly from VDD to Ground called Leakage current.

To find the switching threshold

```
Vin in 0 2.5
***Simulation Command***

.op
.dc Vin 0 2.5 0.05
```
<img width="664" alt="Screenshot 2024-11-13 at 10 06 16 PM" src="https://github.com/user-attachments/assets/dbd20711-5163-4694-a1cb-61f4b8cdc9ed">


Transient analysis is used for finding propagation delay. SPICE transient analysis uses pulse input shown below:

<img width="254" alt="Screenshot 2024-11-13 at 10 06 23 PM" src="https://github.com/user-attachments/assets/cea834e3-9e3b-478a-a988-8a7f17bac567">


The simulation commands:

```
Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
*** Simulation Command ***
.op
.tran 10p 4n
```

Result of SPICE simulation for transient analysis:
<img width="662" alt="Screenshot 2024-11-13 at 10 06 30 PM" src="https://github.com/user-attachments/assets/284193d2-23e3-4277-84d1-776ada4a254c">



Now, we clone the custom inverter

```
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign
cd vsdstdcelldesign
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
ls
magic -T sky130A.tech sky130_inv.mag &
```


<img width="1440" alt="Screenshot 2024-11-13 at 10 29 42 PM" src="https://github.com/user-attachments/assets/ceb8e98b-60e2-4ba0-be92-11cb9215cc44">


<img width="1440" alt="Screenshot 2024-11-13 at 10 29 30 PM" src="https://github.com/user-attachments/assets/4f456cdb-c70a-44b9-a110-7d1bde7392aa">

**Inception of Layout CMOS fabrication process**

The 16-mask CMOS design fabrication process:

1. Substrate Preparation: The process begins with preparing a silicon wafer as the foundational substrate for the circuit.
2. N-Well Formation: The N-well regions are created on the substrate by introducing impurities, typically phosphorus, through ion implantation or diffusion
3. P-Well Formation: Similar to the N-well formation, P-well regions are created using ion implantation or diffusion with boron or other suitable dopants.
4. Gate Oxide Deposition: A thin silicon dioxide layer is deposited to form the gate oxide, which insulates the gate from the channel.
5. Poly-Silicon Deposition: A layer of polysilicon is deposited on the gate oxide to serve as the gate electrode.
6. Poly-Silicon Masking and Etching: A photoresist mask defines areas where polysilicon should remain, and etching removes exposed portions.
7. N-Well Masking and Implantation: A photoresist mask is used to define the areas where the N-well regions should be preserved. Phosphorus or other suitable impurities are then implanted into the exposed regions.
8. P-Well Masking and Implantation: Similarly, a photoresist mask is used to define the areas where the P-well regions should be preserved. Boron or other suitable impurities are implanted into the exposed regions.
9. Source/Drain Implantation: Using photoresist masks, dopants are implanted to create source and drain regions (e.g., arsenic for NMOS, boron for PMOS).
10. Gate Formation: The gate electrode is defined by etching the poly-silicon layer using a photoresist mask.
11. Source/Drain Masking and Etching: A photoresist mask is applied to define the source and drain regions followed by etching to remove the oxide layer in those areas.
12. Contact/Via Formation: Contact holes or vias are etched through the oxide layer to expose the underlying regions, such as the source/drain regions or poly-silicon gates.
13. Metal Deposition: A layer of metal, typically aluminum or copper, is deposited on the wafer surface to form the interconnects.
14. Metal Masking and Etching: A photoresist mask is used to define the metal interconnects, and etching is performed to remove the exposed metal, leaving behind the desired interconnect patterns.
15. Passivation Layer Deposition: A protective layer, often made of silicon dioxide or nitride, is deposited to isolate and shield the metal interconnects.
16. Final Testing and Packaging: The fabricated wafer undergoes rigorous testing to ensure the functionality of the integrated circuits. The working chips are then separated, packaged, and prepared for use in various electronic devices.

<img width="661" alt="Screenshot 2024-11-13 at 10 30 18 PM" src="https://github.com/user-attachments/assets/8bc3ee83-9476-4bb0-b797-a3520dc2b36e">


Inverter layout:

Identify NMOS:
<img width="1440" alt="Screenshot 2024-11-13 at 10 38 01 PM" src="https://github.com/user-attachments/assets/54301c39-d839-4f9e-a1cc-cf1111a4eaf2">



Identify PMOS:
<img width="1440" alt="Screenshot 2024-11-13 at 10 40 20 PM" src="https://github.com/user-attachments/assets/d8f0fb7c-2d22-428e-a3e9-e29d49a8aefa">



Output Y:
<img width="1440" alt="Screenshot 2024-11-13 at 10 41 09 PM" src="https://github.com/user-attachments/assets/ac2751ae-056d-48b6-a708-354b941e8555">



PMOS source connected to VDD:
<img width="1440" alt="Screenshot 2024-11-13 at 10 41 42 PM" src="https://github.com/user-attachments/assets/2670b34a-6304-433b-9c34-0a2e94f6448a">



NMOS source connected to VSS:

<img width="1440" alt="Screenshot 2024-11-13 at 10 42 13 PM" src="https://github.com/user-attachments/assets/7f5a7d71-85ac-4350-9a3d-6e502f538c58">


Spice extraction of inverter in Magic. Run these in the tkcon window:

```
# Check current directory
pwd
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
<img width="1440" alt="Screenshot 2024-11-13 at 10 43 16 PM" src="https://github.com/user-attachments/assets/79b5c9ad-e7f9-46e4-abbd-56c61238d473">

To view the spice file:
<img width="1440" alt="Screenshot 2024-11-13 at 10 44 31 PM" src="https://github.com/user-attachments/assets/4045d198-3eb9-4e70-8ed2-5e9b2a8af1a6">

<img width="1440" alt="Screenshot 2024-11-13 at 10 44 56 PM" src="https://github.com/user-attachments/assets/f3a4339a-8606-4a5f-b747-c541734888fc">




The contents of spice file:

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=10n

.subckt sky130_inv A Y VPWR VGND
X0 Y A VGND VGND sky130_fd_pr__nfet_01v8 ad=1.37n pd=0.148m as=1.37n ps=0.148m w=35 l=23
X1 Y A VPWR VPWR sky130_fd_pr__pfet_01v8 ad=1.44n pd=0.152m as=1.52n ps=0.156m w=37 l=23
C0 VPWR Y 0.11fF
C1 A Y 0.754fF
C2 A VPWR 0.277fF
C3 Y VGND 0.279fF
C4 A VGND 0.45fF
C5 VPWR VGND 0.781fF
.ends

```

Now modify the `sky130_inv.spice` file to find the transient respone:



```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.0774f
C1 VPWR Y 0.117f
C2 A Y 0.0754f
C3 Y VGND 2f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
//.ends

.tran 1n 20n
.control
run
.endc
.end
```

Now, simulate the spice netlist
```
ngspice sky130_inv.spice
```

<img width="1440" alt="Screenshot 2024-11-13 at 10 48 54 PM" src="https://github.com/user-attachments/assets/2f0e0938-5361-43b5-b149-81e17d6845b7">


To plot the waveform:

```
plot y vs time a
```
<img width="1440" alt="Screenshot 2024-11-13 at 10 49 23 PM" src="https://github.com/user-attachments/assets/a066eea7-ae9b-425b-b43e-00984a9b29ae">



Using this transient response, we will now characterize the cell's slew rate and propagation delay:

Rise Transition: Time taken for the output to rise from 20% to 80% of max value
Fall Transition: Time taken for the output to fall from 80% to 20% of max value
Cell Rise delay: difference in time(50% output rise) to time(50% input fall)
Cell Fall delay: difference in time(50% output fall) to time(50% input rise)

```
Rise Transition : 2.24638 - 2.18242 =  0.06396 ns = 63.96 ps
Fall Transition : 4.0955 - 4.05536 =  0.0419 ns = 41.9 ps
Cell Rise Delay : 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
Cell Fall Delay : 4.07807 - 4.05 =0.02 ns = 20 ps
```


Magic Tool options and DRC Rules:

Now, go to home directory and run the below commands:

```
cd
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests
ls -al
gvim .magicrc
magic -d XR &
```

<img width="1440" alt="Screenshot 2024-11-13 at 10 51 20 PM" src="https://github.com/user-attachments/assets/abcfa703-c252-4f8d-bd44-db43ece5bccf">

First load the poly file by load poly.mag on tkcon window.

<img width="1440" alt="Screenshot 2024-11-13 at 10 54 44 PM" src="https://github.com/user-attachments/assets/02f4597f-1657-48c4-a760-465b729c0e10">



We can see that Poly.9 is incorrect.

Add the below commands in the sky130A.tech

<img width="1440" alt="Screenshot 2024-11-13 at 11 02 29 PM" src="https://github.com/user-attachments/assets/eb30ace6-c9bf-43cd-bfa8-bad2acafe405">

<img width="1440" alt="Screenshot 2024-11-13 at 11 04 01 PM" src="https://github.com/user-attachments/assets/28ce728a-9267-4a43-a231-1938282bf327">




Run the commands in tkcon window:

```
tech load sky130A.tech
drc check
drc why
```
<img width="1440" alt="Screenshot 2024-11-13 at 11 06 29 PM" src="https://github.com/user-attachments/assets/d926bdb9-b000-47b5-8070-be54242ea076">


</details>




<details>

<summary><strong>Day-4:</strong> Pre-layout timing analysis and importance of good clock tree. </summary>

Commands to extract `tracks.info` file:

```
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cd ../../pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
less tracks.info
```


<img width="1440" alt="Screenshot 2024-11-13 at 11 10 46 PM" src="https://github.com/user-attachments/assets/b16726bb-edcf-42b7-99d3-8f177d341a75">


Commands for tkcon window to set grid as tracks of locali layer

```
grid 0.46um 0.34um 0.23um 0.17um
```
<img width="1440" alt="Screenshot 2024-11-13 at 11 13 53 PM" src="https://github.com/user-attachments/assets/5d998264-a0be-4cfb-bf45-c1400548e85f">



The grids show where the routing for the local-interconnet layer can only happen, the distance of the grid lines are the required pitch of the wire. Below, we can see that the guidelines are satisfied:

<img width="1440" alt="Screenshot 2024-11-13 at 11 14 11 PM" src="https://github.com/user-attachments/assets/f2af1763-21ad-4271-b090-3f127f1f5a84">


Now, save it by giving a custon mae

```
save sky130_divinv.mag
```

<img width="1440" alt="Screenshot 2024-11-13 at 11 16 14 PM" src="https://github.com/user-attachments/assets/77dfeaec-a6c7-48e9-9e1a-d6fd6a05a637">

Now, open it by using the following commands:

```
magic -T sky130A.tech sky130_divinv.mag &
```

<img width="1440" alt="Screenshot 2024-11-13 at 11 18 12 PM" src="https://github.com/user-attachments/assets/014dc827-62a2-4bf0-b136-469e76b16ede">


Now, type the following command in tkcon window:

```
lef write
```

<img width="1440" alt="Screenshot 2024-11-13 at 11 19 05 PM" src="https://github.com/user-attachments/assets/21645f42-41bc-4828-8b4d-babadbfd7a07">

<img width="1440" alt="Screenshot 2024-11-13 at 11 19 24 PM" src="https://github.com/user-attachments/assets/047e0725-b232-4781-a280-5d7646a86fa1">


Modify config.tcl:

```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILES) "./designs/picorv32a/src/picorv32a.sdc"


set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT) 


set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib "
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib "
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]   

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1 } {
  source $filename
}
```
Now, run openlane flow synthesis:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```

<img width="1440" alt="Screenshot 2024-11-14 at 12 36 59 AM" src="https://github.com/user-attachments/assets/629b40e5-24eb-4ebd-8650-5d42f3430920">
<img width="1440" alt="Screenshot 2024-11-14 at 12 38 21 AM" src="https://github.com/user-attachments/assets/29bcc5e8-405a-42fe-981f-3bfdfa3ee917">

<img width="1440" alt="Screenshot 2024-11-14 at 12 39 42 AM" src="https://github.com/user-attachments/assets/561ea552-2cdf-4957-8061-bdb7d4225175">

<img width="1440" alt="Screenshot 2024-11-14 at 12 39 51 AM" src="https://github.com/user-attachments/assets/2aec75fa-e971-4561-83e5-ac9ddf11e19d">

<img width="1440" alt="Screenshot 2024-11-14 at 2 40 08 AM" src="https://github.com/user-attachments/assets/1ba59f00-dad8-4b55-99c1-edb8c0621a23">




**Delay Tables**

Delay plays a crucial role in cell timing, impacted by input transition and output load. Cells of the same type can have different delays depending on wire length due to resistance and capacitance variations. To manage this, "delay tables" are created, using 2D arrays with input slew and load capacitance for each buffer size as timing models. Algorithms compute buffer delays from these tables, interpolating where exact data isn’t available to estimate delays accurately, preserving signal integrity across varying load conditions.

<img width="658" alt="Screenshot 2024-11-14 at 12 40 56 AM" src="https://github.com/user-attachments/assets/6179ace6-6d27-49fa-9322-6f03f4c130c9">



Fixing slack:

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 24-03_10-03 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
```

<img width="1440" alt="Screenshot 2024-11-14 at 12 43 12 AM" src="https://github.com/user-attachments/assets/dc3bcc51-4d58-4d4a-a33e-3ef11ea19732">
<img width="1440" alt="Screenshot 2024-11-14 at 2 44 14 AM" src="https://github.com/user-attachments/assets/575bde97-39fb-4e7b-8a1c-76d4f1b66b61">

<img width="1440" alt="Screenshot 2024-11-14 at 2 43 18 AM" src="https://github.com/user-attachments/assets/aa1e1a39-ab92-4eae-b7ff-6f312839e95f">





Now, run floorplan

```
run_floorplan
```
<img width="1440" alt="Screenshot 2024-11-14 at 2 45 12 AM" src="https://github.com/user-attachments/assets/baafebfc-15ef-4964-8142-debbc401c7e0">




Since we are facing unexpected un-explainable error while using run_floorplan command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on Floorplan Commands section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```
init_floorplan
place_io
tap_decap_or
```
<img width="1440" alt="Screenshot 2024-11-14 at 2 45 58 AM" src="https://github.com/user-attachments/assets/57ee0135-42b9-476f-945f-833c8cd33350">

<img width="1440" alt="Screenshot 2024-11-14 at 2 46 19 AM" src="https://github.com/user-attachments/assets/b03ed082-13db-4f08-8ddb-ebf4d3426376">





Now, do placement

```
run_placement
```



<img width="1440" alt="Screenshot 2024-11-14 at 2 47 40 AM" src="https://github.com/user-attachments/assets/777ac7d1-71b2-4021-9793-a135e50fd457">



Now, open a new terminal and run the below commands to load placement def in magic

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

<img width="1440" alt="Screenshot 2024-11-14 at 2 49 54 AM" src="https://github.com/user-attachments/assets/4f84bc60-f890-46b3-9df5-57759b3506ca">

<img width="1440" alt="Screenshot 2024-11-14 at 2 56 56 AM" src="https://github.com/user-attachments/assets/f655bc97-60be-44f6-96d2-137ada31a2e2">

**Timing analysis with ideal clocks using openSTA**

Pre-layout STA will include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).

<img width="655" alt="Screenshot 2024-11-14 at 3 00 10 AM" src="https://github.com/user-attachments/assets/cf7eda93-4be4-47c2-bb01-20895a799f98">


Since we are getting 0 wns after improved timing run, we will be doing the timing analysis on initial run of synthesis which has lots of violations and no parameters added to improve timing.

Commands to invoke the OpenLANE flow include new lef and perform synthesis:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9set
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
```

Go, to `Desktop/work/tools/openlane_working_dir/openlane` and create a file `pre_sta.conf`. The contents of the file are:

```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```
Contents of `my_base.sdc`:

```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 12.000
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.65
create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set IO_PCT  0.2
set input_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"


set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```
Commands to run STA:

```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```






</details>





<details>


 
<summary><strong>Day-5:</strong> Open-source EDA, OpenLane and Sky130 PDK</summary>

</details>





</details>



