# VSDSquadron_Labs
execution steps:
================
Note: we already installed RISC-V toolchain.
	write c-code file with gvim editor 

 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/06629957-d926-46cb-8b0d-e05974537719)

it will open gvim editor and write below code

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/3f0af1b4-4109-41c6-b2d1-19e495630b76)

I. c-file compile with GCC and run
----------------------------------
  1. compile c file 
  	cmd: gcc file.c
  2. run compiled object file
  	cmd: ./a.out

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
		 
