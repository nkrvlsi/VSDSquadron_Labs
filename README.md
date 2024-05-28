# VSDSquadron_Labs
# *Task 1. C-lab & RISC-V lab: counting sum of numbers from 1 to n *
==============================

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



# **Task2: RISC-V Instruction set**
===================================

RISC-V stands for **Reduced Instruction set Computer**. 

- it is a **free & open ISA**
	- **ISA** stands for **Instruction Set Architecture** 
- RISC-V is a family of related ISA's
	- RISC-V processors have following 5 different **Instruction Cycles**
 		- 1.Fetch 2.decode 3.execute 4.Memory 5.writeback
   			- **instruction cycle** consists of several steps, each performs a specific function in the execution of the instruction. The major steps in the instruction cycle are:
      			- **1   Fetch:**	Read instruction from instruction memory.
         			-   In the fetch cycle, the CPU retrieves the instruction from memory. The instruction is typically stored at the address specified by the program counter (PC). The PC is then incremented to point to the next instruction in memory.
         		- **2   Decode:**	 Read program registers.
            			- In the decode cycle, the CPU interprets the instruction and determines what operation needs to be performed. This involves identifying the opcode and any operands that are needed to execute the instruction.
           		- **3   Execute:**	Compute value or address.
             			-   In the execute cycle, the CPU performs the operation specified by the instruction. This may involve reading or writing data from or to memory, performing arithmetic or logic operations on data, or manipulating the control flow of the program.
             	- **4   Memory:** 	Read or write back data
             	- **5   Write Back:**	Write program registers.
             	- **6   PC:**		Update the program counter.
        
 	- currently 4 base ISA's
 		- Each **base integer instruction set** is characterized by the **width of the integer registers** and the corresponding **size of the address space** and by the **number of integer registers**  
 		- Two primary base integer variants - **1. RV32I and 2. RV64I** - XLEN - refers width of an integer register in bits (either 32 or 64).
     1. 6 types of instruction formats (R/I/S/U/SB/UJ)
         	- **R**-format **I**-formar **S**-format **U**-format **SB**-Format **UJ**-format
     2. **User visible Base integer registers** -  there are **31 general-purpose registers x1–x31**, which hold integer values.
       	   	- Register x0 is hardwired to the constant 0.
          	- For **RV32 the x registers are 32 bits wide**, and for **RV64 they are 64 bits wide**
       	   		- each RISC-V instruction = 32 bits = 1 Word = 4 Bytes
            		- we uses the term **XLEN** to refer to the current **width of an x register in bits** (either **32 or 64**).
              		- <img width="224" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/b524a4b5-2a74-4d76-9848-b397a562b735">
 
	      	- There is 1 additional user-visible register: **PC - program counter**
       			- pc holds the **address of the current instruction**.
  			
          		 
  ## RISC-V ISA
  - 47 base instructions modular ISA
     
