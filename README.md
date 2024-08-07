# ASIC-Design
# Divyansh Singhal (IMT2021522)
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
   <img width="1440" alt="Screenshot 2024-08-07 at 7 28 27 PM" src="https://github.com/user-attachments/assets/f60df2fe-f66f-4625-925f-1a60843c4adc">
2. Now we open it using objdump and then | less to see the <main> segment of the code.
<img width="1440" alt="Screenshot 2024-08-07 at 7 31 08 PM" src="https://github.com/user-attachments/assets/78e7bcc5-9afc-42a8-ab06-d7c6f1784067">
<img width="1440" alt="Screenshot 2024-08-07 at 7 35 36 PM" src="https://github.com/user-attachments/assets/76695dd4-cca5-462a-b5e5-a2e0f78dd52e">
3. Now calucation the number os instructions a shown. We got 11 instructions.
<img width="1440" alt="Screenshot 2024-08-07 at 11 39 52 PM" src="https://github.com/user-attachments/assets/74589e55-5abc-40aa-acc9-78860badf14d">
4. Now we will run using Ofast instead of O1 and check the instructions.
   <img width="1440" alt="Screenshot 2024-08-07 at 11 41 58 PM" src="https://github.com/user-attachments/assets/67983ca8-f045-4fef-b99d-eaf2cafb6913">

