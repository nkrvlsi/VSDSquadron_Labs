# VSDSquadron_Labs
*Task 1. C-lab & RISC-V lab: counting sum of numbers from 1 to n *
============================

Note: we already installed RISC-V toolchain.
1. write c-code file with gvim editor

   cmd: **gvim sum1ton.c**

 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/06629957-d926-46cb-8b0d-e05974537719)

It will open gvim editor and you can write your c source code. Here I'm writing **counting sum of numbers from 1 to n**.

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/3f0af1b4-4109-41c6-b2d1-19e495630b76)

I. c-file compile with GCC and run
----------------------------------
  2. compile c file (below cmd does 4 steps: prepocess, compile, assemble, link to create executable file)
  	
   cmd: **gcc file.c**
	
 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/28dfdce0-1c2e-4d35-a498-2f3227ae6add)

It will create a.out executable file

  3. run executable file
  	
   cmd: **./a.out**

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/4c1da9e5-5ad0-4bda-9098-5365e96112b2)
   
   check output in the terminal: 
       **sum of numbers from 1 to 5 is 15**


II. compile same c file with RISC-V gcc compiler & see genearted assemmbly code
-------------------------------------------------------------------------------
  4. cmd: **riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c**
	
 above cmd will create **sum1ton.o** file
 
	other options
      
	 -O1/-Ofast
 
	 -time                    Time the execution of each subprocess.
 
	 -o <file>                Place the output into <file>.
 
	 -march=rv32i -mabi=ilp32 to the linker. 

 
![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/6f9c67f3-b4ec-4ba2-9e94-aed6b5aa0edd)


  5. check how assembly level instructions for the c-code getting generated with below command

     cmd: **riscv64-unknown-elf-objdump -d sum1ton.o**

	     here
	
	     	objdump is object dump
	     
	     	d is disassemble
	     
	     	use |less cmd at end & search for /main

     cmd: **riscv64-unknown-elf-objdump -d sum1ton.o | less**


![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/dad96f6c-ca64-4200-b0e6-4b028a2c216b)

     serach for main function in this disassembly code

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/d53b4b53-bbcc-4d15-bdd1-557cadb7f75a)

Count no of instructions in the code. main() function started at address 0x1_0184, next subtask executed at address 0x1_01b0. difference between  0x1_01b0 -  0x1_0184 = 2c/4 we are diving here becoz of byte aligned address, so address jumps by 4. = 'd11. So total 11 instructions present in main to next subtask.

Now lets try other option like -Ofast and check the assembly code

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/0fb56f59-25e1-4f85-80b2-3670b6e16c83)


![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/72e5f7f7-d88b-4b9a-b4f1-ced926d0f959)

now count number of instructions 100dc-100b0= 2C/4=B='d11. No of instrctions didnt changed even though if we change -O1/-Ofast.




     
