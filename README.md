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
   
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64-march=rv64i -o sum1toN.o  sum1toN.c
```

```bash
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
```bash
riscv64-unknown-elf-objdump -d sum1toN.o
```
```bash
riscv64-unknown-elf-objdump -d sum1toN.o | less
```

<img width="1440" alt="Screenshot 2024-08-08 at 1 19 20 AM" src="https://github.com/user-attachments/assets/e89c54fa-bb63-4afb-820e-f4edd5e67a8c">

<img width="1440" alt="Screenshot 2024-08-08 at 1 19 44 AM" src="https://github.com/user-attachments/assets/2b1e4053-63c8-4531-ba83-ae80d8d1f412">


3. Now calucation the number os instructions a shown. We got 15 instructions also shown in calculator.

```bash
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
 ```bash
spike pk sum1toN.o
```
<img width="1440" alt="Screenshot 2024-08-08 at 1 15 12 AM" src="https://github.com/user-attachments/assets/a2e99939-3135-48d2-a224-8c61826a7c05">


- `spike`: The Spike RISC-V simulator.
- `d`: Starts the simulator in debug mode.
- `pk`: Proxy kernel, a small environment that provides minimal OS services.
- `sum1toN.o`: The compiled object file of your C program.

2. Check using the following commands in the spike debugger


 ```bash
spike pk sum1toN.o
```
```bash
until pc 0 100b0
```
To check a registers Value type the following command 
```bash
reg 0 a2
```
To check stack pointer's Value type the following command 
```bash
reg 0 sp
```

<img width="1440" alt="Screenshot 2024-08-08 at 1 12 11 AM" src="https://github.com/user-attachments/assets/fa264e40-87a9-4845-8a26-3a03f7e03994">
<img width="1440" alt="Screenshot 2024-08-08 at 1 14 39 AM" src="https://github.com/user-attachments/assets/3f85c504-7630-4d59-8e87-113016fc4d33">

- In the assembly code, it's evident that the stack pointer's value is being decreased by `0x10` in hexadecimal notation. This hexadecimal value translates to a reduction of 16 in decimal notation. Thus, the stack pointer is effectively being reduced by `16` units in decimal form.
</details>

