# VSDSquadron_Labs
**1. C-lab & RISC-V lab:**
==========================
Note: we already installed RISC-V toolchain.
	write c-code file with gvim editor 

 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/06629957-d926-46cb-8b0d-e05974537719)

it will open gvim editor and write below c source code.

Here I'm writing counting sum of numbers from 1 to n.

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/3f0af1b4-4109-41c6-b2d1-19e495630b76)

I. c-file compile with GCC and run
----------------------------------
  1. compile c file (below cmd does 4 steps: prepocess, compile, assemble, link to create executable file)
  	
   cmd: gcc file.c
	
 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/28dfdce0-1c2e-4d35-a498-2f3227ae6add)

It will create a.out executable file

  2. run executable file
  	
   cmd: ./a.out

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/4c1da9e5-5ad0-4bda-9098-5365e96112b2)
   
   check output in the terminal: 
       **sum of numbers from 1 to 5 is 15**


II. compile same c file with RISC-V gcc compiler & see genearted assemmbly code
-------------------------------------------------------------------------------
  3. cmd: riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
	this should create *.o object file ex:sum1ton.o
	try -O1/-Ofast options

  4. Open assembly level instructions for the c-code with objectdump with below command
	cmd: riscv64-unknown-elf-objdump -d sum1ton.o
		here 
		 objdump is object dump
		 d is disassemble
		 use |less cmd at end & search for /main
		 
