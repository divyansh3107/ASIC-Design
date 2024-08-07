# ASIC-Design
# Divyansh Singhal (IMT2021522)
<details>
<summary><strong>Lab 1:</strong> Create a C program to find the sum of `n` natural numbers, compile it using the GCC compiler, and verify the output after execution.</summary>

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
ls -ltr sum1ton.o
```
   <img width="1440" alt="Screenshot 2024-08-07 at 7 28 27 PM" src="https://github.com/user-attachments/assets/f60df2fe-f66f-4625-925f-1a60843c4adc">

- `riscv64-unknown-elf-gcc`: The compiler for RISC-V 64-bit target.
- `O1`: Applies moderate optimizations for a good balance between performance and compilation time.
- `mabi=lp64`: Specifies the ABI (Application Binary Interface) as LP64, meaning "Long and Pointer are 64-bit."
- `march=rv64i`: Sets the architecture to RISC-V 64-bit with the RV64I instruction set.
- `o sum1ton.o`: Outputs the compiled code to a file named sum1ton.o.
- `sum1ton.c`: The source file to be compiled.

2. Now we open it using objdump and then | less to see the <main> segment of the code.
```bash
riscv64-unknown-elf-objdump -d sum1toN.o
```
```bash
riscv64-unknown-elf-objdump -d sum1toN.o | less
```

<img width="1440" alt="Screenshot 2024-08-07 at 7 31 08 PM" src="https://github.com/user-attachments/assets/78e7bcc5-9afc-42a8-ab06-d7c6f1784067">

<img width="1440" alt="Screenshot 2024-08-07 at 7 35 36 PM" src="https://github.com/user-attachments/assets/76695dd4-cca5-462a-b5e5-a2e0f78dd52e">
3. Now calucation the number os instructions a shown. We got 11 instructions.
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64-march=rv64i -o sum1toN.o  sum1toN.c
```
<img width="1440" alt="Screenshot 2024-08-07 at 11 39 52 PM" src="https://github.com/user-attachments/assets/74589e55-5abc-40aa-acc9-78860badf14d">
4. Now we will run using Ofast instead of O1 and check the number of instructions.
   <img width="1440" alt="Screenshot 2024-08-07 at 11 41 58 PM" src="https://github.com/user-attachments/assets/67983ca8-f045-4fef-b99d-eaf2cafb6913">
 - **O1**: Provides moderate optimizations, balancing performance and compilation time, and adheres strictly to standards.
- **Ofast**: Applies aggressive optimizations for maximum performance, but might break some programs as it may not follow all standards.
</details>

<details>
<summary><strong>Lab 2:To debug the assembly code of your compiled C program using the Spike simulator</strong> </summary>

# Lab-2
## Debugging the code in Spike on RISC V




</details>

