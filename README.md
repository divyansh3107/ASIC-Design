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
```
$out = ! $in;
```
Output waveform


<img width="1440" alt="Screenshot 2024-08-21 at 12 23 56 AM" src="https://github.com/user-attachments/assets/c80f3716-a073-42ae-bb91-b5144c247275">


### 2. 2-input AND gate

Code
```
 $out = $in1 && $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 26 29 AM" src="https://github.com/user-attachments/assets/db8c83c7-17d3-4a07-91a6-94f1b0ca2785">


### 3. 2-input OR gate

Code
```
 $out = $in1 || $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 28 09 AM" src="https://github.com/user-attachments/assets/5c7cf51b-0c3d-41de-ae1d-a676d79de5b7">



### 4. 2-input XOR gate

Code
```
$out = $in1 ^ $in2;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 29 45 AM" src="https://github.com/user-attachments/assets/a8d584a7-28b3-43b1-b9f2-2e094751ea34">


### 5. Operation on Vector

Code
```
$out[2:0] = $in1[1:0] + $in2[1:0];
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 31 11 AM" src="https://github.com/user-attachments/assets/a3563e6a-1194-404b-b763-bb1ce0bcfcc0">


### 6. 2:1 MUX

Code
```
$out = $sel ? $in1 : $in0;
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 32 49 AM" src="https://github.com/user-attachments/assets/10d4aadb-694e-4121-91be-39f144d842b1">



### 7. 2:1 MUX using Vectors

Code
```
$out[3:0] = $sel ? $in1[3:0] : $in0[3:0];
```
Output waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 34 22 AM" src="https://github.com/user-attachments/assets/717c7204-c96b-4857-a0c5-d1a40944121c">


### 8. Combinational Calculator Implementation
The calculator performs four fundamental arithmetic operations: addition, subtraction, multiplication, and division.
Code
```
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
```
$reset = *reset;
$num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```
Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 45 04 AM" src="https://github.com/user-attachments/assets/32d223f4-30e2-49ab-808f-2941b3cf7f79">


### 2. Free Running Counter
Increases Previous value by 1.
<img width="198" alt="359608319-d53228bc-fda0-4d34-a14e-2885486ce536" src="https://github.com/user-attachments/assets/9176b013-fb95-4ea4-9cf6-272eb8042b57">

Code
```
$reset = *reset;
$cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
Output Waveform
<img width="1440" alt="Screenshot 2024-08-21 at 12 50 14 AM" src="https://github.com/user-attachments/assets/2d3b7f3b-ccb0-47c5-ad1a-4d0388996880">



### 3. Sequential Calculator
It functions similarly to a combinational calculator but simulates a real-world scenario where the result of a previous operation is used a

Code
```
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
```
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
```
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
```
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
```

```

Output Waveform


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

```
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
```
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







</details>

