# VSDSquadron_Labs
# *Task 1. C-lab & RISC-V lab: counting sum of numbers from 1 to n *
==============================
## Objective: write c-code for counting sum of numbers from 1 to n. Identify number of assembly instructions for the same code. 

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
## Objective: understand various types of RISC-V instructions and where it can be used.
RISC-V stands for **Reduced Instruction set Computer**. 

- it is a **free & open ISA**
	- **ISA** stands for **Instruction Set Architecture** 
- RISC-V is a one of the ISA's family
	- RISC-V processors have following 5 different **Instruction Cycles**
 		- 1.Fetch 2.decode 3.execute 4.Memory 5.writeback
   - **instruction cycle** consists of several steps, each performs a specific function in the execution of the instruction. The major steps in the instruction cycle are:

     **1.   Instruction Fetch (IF):**	Read instruction from instruction memory.
     -   In the instruction fetch cycle, the CPU retrieves the instruction from memory. The instruction then stored at the address specified by the program counter (PC). For every clock cycle, PC incremented by 4 (Byte aligned) to point next instruction in memory.

     **2.   Instruction Decode (ID):**	 Read program registers.
     - In the decode cycle, the CPU interprets the instruction and determines what operation needs to be performed. This involves identifying the opcode and any operands that are needed to execute the instruction. It basically decodes the instruction given by the Program Counter current address.

     **3.   Execute:**	Compute value or address.
     -   In the execute cycle, the CPU **performs the operation specified by the instruction**. This may involve reading or writing data from or to memory, **performing arithmetic or logic operations on data**, or manipulating the control flow of the program.
     -    ex: Read **rs1 ([19:15]bits)** and **rs2 ([24:20]bits)** souce registers (5 bits each) and write rersult into **rd ([11:7]bits)** target register. The [31:25] funct7 and [14:12] funct3 fields select the type of operation, finally the opcode for R-type instruction is specified at [6:0]bits.

     **4.   Memory (Memory access):** 	Read or write back data
     -   load and store instructions transfer a value between the registers and memory. (register ---> memory). LOAD is encoded in I type format where as the STORE is encoded in S type instruction format.
     
     **5.   Write Back (WB):**	Write program registers.
     -	It is the final stage in execution of the instruction in RISC-V pipeline. During this stage, the result of instruction will be written back to the register file, that will be avialable for the future instructions.
     -	The control signals ensure that the correct register (destination register) is updated with the correct data.
     -	Example: ADD rd, rs1, rs2 In the WB stage, the result from the ALU is written back to the destination register rd.
     -	Example: LW rd, offset(rs1) In the WB stage, the data read from memory is written to the destination register rd.

     **6.   PC:**		Update the program counter **address by 4** for the next instruction to be executed .
        
 	- currently 4 base ISA's
 		- Each **base integer instruction set** is characterized by the **width of the integer registers** and the corresponding **size of the address space** and by the **number of integer registers**  
 		- Two primary base integer variants - **1. RV32I and 2. RV64I** - XLEN - refers width of an integer register in bits (either 32 or 64).
   		- 6 types of instruction formats (R/I/S/U/SB/UJ)
         	- **R**-format **I**-formar **S**-format **U**-format **SB**-Format **UJ**-format
          		- R stands for **Register**
          		- I stands for **immediate, loads**
          		- S refers to **store**
          		- U refers to **Upper immediate**
          		- B refers to **branch** type
          		- J refers to **Jump** 
            
          		![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/4d049f09-3209-4f1a-9fa5-2f07aee13feb)

1. **User visible Base integer registers**
   -  there are **31 general-purpose registers x1–x31**, which hold integer values.
   	- Register x0 is hardwired to the constant 0.
   	- For **RV32 the x registers are 32 bits wide**, and for **RV64 they are 64 bits wide**
   		-  each RISC-V instruction = 32 bits = 1 Word = 4 Bytes
   	 	- we uses the term **XLEN** to refer to the current **width of an x register in bits** (either **32 or 64**).
   	  - <img width="224" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/b524a4b5-2a74-4d76-9848-b397a562b735">
 
   - There is 1 additional user-visible register: **PC - program counter**
 		- pc holds the **address of the current instruction**.

2. **Base Instruction Format**

	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/dbb47cd6-90fe-4951-a994-6f203fb48e0b)
	- All are fixed **32 bits in length** and must **4-byte boundary aligned** in memory
	- The RISC-V ISA keeps the **source (rs1 and rs2) and destination (rd) registers** at the same position in all formats to simplify decoding.
	- **Immediates** are packed towards the leftmost available bits in the instruction
	- **sign bit** for all immediates always at the 31-bit of the instruction
 - There are a further two variants of the instruction formats (SB/UJ) based on the handling of immediates, as shown in Figure 2.3.

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/dffed509-5520-4033-ba61-c679827f5422)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/a0142f55-c687-42f6-a1db-c18bd5cc2cfd)

 - In Figure 2.3 each immediate subfield is labeled with the bit position (**imm[x]**) in the immediate value being produced.
 - The only difference between the S and SB formats is that the 12-bit immediate field is used to encode branch offsets in multiples of 2 in the SB format.
 	- Instead of shifting all bits in the instructionencoded immediate left by one in hardware as is conventionally done, the middle bits (imm[10:1])

    ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c025508c-aa3a-41e6-af81-69e92abbecbb)
 - Figure 2.4: Types of immediate produced by RISC-V instructions. The fields are labeled with the instruction bits used to construct their value. Sign extension always uses inst[31].
 - and sign bit stay in fixed positions, while the lowest bit in S format (inst[7]) encodes a high-order bit in SB format.
 - Similarly, the only difference between the U and UJ formats is that the 20-bit immediate is shifted left by 12 bits to form U immediates and by 1 bit to form J immediates.
	- The location of instruction bits in the U and UJ format immediates is chosen to maximize overlap with the other formats and with each other.

## Now lets discuss each instruction formats (R/I/S/U/SB/UJ) in details below

1. R-format (Register)	- Arithmetic and logical operations

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e32e4c2a-77fd-408a-9c3a-b44d52304754)

   - Each field is viewed as its own unsigned int
	– 5-bit fields can represent any number 0-31, while 7-bit fields can represent any number 0-128, etc.
   - **opcode [6:0]bits** (7 bits): partially specifies operation
	– e.g. R-types have opcode = 0b0110011=0x33, SB (branch) types have opcode = 0b1100011=0x63.
   - funct7 [31:25]bits (7) + funct3 [14:12] (3) : total 10 bits.  combined with opcode, these two fields describe what operation to perform.
   	- How many R-instructions can we encode? Ans: with opcode fixed at 0x33, just funct varies:  (2^7$) x (2^3$)= (2^10$) = 1024
3. I-format (immediate, loads)
4. S-format (store)
5. U-format (Upper immediate)
6. SB-format (Branch)
7. UJ-format (jump)


  ## RISC-V ISA
  - 47 base instructions modular ISA
     
