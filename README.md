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

1. **User visible Base integer registers** - General-Purpose Register and PC
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
	
 	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e637167b-e54d-42f5-b7ed-ce753219d848)

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

1. **R-format (Register)**	- Arithmetic and logical operations


![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/04038e2b-6c21-4eb0-9c06-a9fbef6a0d4e)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e18a6ee2-b21c-416a-b8c7-41c4ccaf1998)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/3e6777fc-92ca-4446-8e9c-0f9e6c722caa)


   - Each field is viewed as its own unsigned int
	– 5-bit fields can represent any number 0-31, while 7-bit fields can represent any number 0-128, etc.
   - **opcode [6:0]bits** (7 bits): partially specifies operation
   		- e.g. R-types have opcode = 0b0110011=0x33, SB (branch) types have opcode = 0b1100011=0x63.
   - funct7 [31:25]bits (7) + funct3 [14:12] (3) : total 10 bits.  combined with opcode, these two fields describe what operation to perform.
   		- How many R-instructions can we encode? Ans: with opcode fixed at 0x33, just funct varies:  (2<sup>7</sup>) x (2<sup>3</sup>)= (2<sup>10</sup>) = 1024
   - rs1 (5): 1st operand (“source register 1”)
   - rs2 (5): 2nd operand (second source register)
   - rd (5): “destination register” — receives the result of computation
   - lets take an instruction example **add x5,x6,x7** - as simple as **add rd,r1,r2**. Instruction Code: 0x0073_02B3
     ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/a8f2f8ae-4ae7-4d2d-8d92-0f4524bf0ae5)

   examples:-
	1. ADD r6, r2, r1	addition
	
		- Opcode: 0110011
		- Funct3: 000
		- Funct7: 0000000
		- rd: 00110 // represent as 5'b6
		- rs1: 00001 // represent as 5'b1
		- rs2: 00010 // represent as 5'b2
		- Instruction Code: 0000000 00010 00001 000 00110 0110011
		
	2. SUB r7, r1, r2	subtraction
	
		- Opcode: 0110011
		- Funct3: 000
		- Funct7: 0100000
		- rd: 00111
		- rs1: 00001
		- rs2: 00010
		- Instruction Code: 0100000 00010 00001 000 00111 0110011
	
	3. AND r8, r1, r3	AND operation between r1 & r3
	
		- Opcode: 0110011
		- Funct3: 111
		- Funct7: 0000000
		- rd: 01000
		- rs1: 00001
		- rs2: 00011
		- Instruction Code: 0000000 00011 00001 111 01000 0110011
	
	4. OR r9, r2, r5	OR operation between r2 and r5
	
		- Opcode: 0110011
		- Funct3: 110
		- Funct7: 0000000
		- rd: 01001
		- rs1: 00010
		- rs2: 00101
		- Instruction Code: 0000000 00101 00010 110 01001 0110011
	
	5. XOR r10, r1, r4	XOR operation between r1 and r4
	
		- Opcode: 0110011
		- Funct3: 100
		- Funct7: 0000000
		- rd: 01010
		- rs1: 00001
		- rs2: 00100
		- Instruction Code: 0000000 00100 00001 100 01010 0110011
	
	6. SLT r11, r2, r4	r11=1 if x is negative; r11=0 if 0 or positive
	
		- Opcode: 0110011
		- Funct3: 010
		- Funct7: 0000000
		- rd: 01011
		- rs1: 00010
		- rs2: 00100
		- Instruction Code: 0000000 00100 00010 010 01011 0110011
	
	7. SRL r16, r14, r2
	
		- Opcode: 0110011
		- Funct3: 101
		- Funct7: 0000000
		- rd: 10000
		- rs1: 01110
		- rs2: 00010
		- Instruction Code: `0000000 00010 01110 101 10000 0110011
	
	8. SLL r15, r1, r2
	
		- Opcode: 0110011
		- Funct3: 001
		- Funct7: 0000000
		- rd: 01111
		- rs1: 00001
		- rs2: 00010
		- Instruction Code: 0000000 00010 00001 001 01111 0110011

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/df097697-02fb-438a-98a4-d14376179d4f)

2. **I-format** (immediate, loads)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e29b28fe-ec73-432f-9f5a-d79246c0272c)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/81669faf-9209-44bd-aab6-5cb53586526e)

   The upper 12 bits of I-type is an immediate number. The opcode is different from other instruction formats because the corresponding specific operations are different, and other parts are very similar to R-type

   - In a RISC processor, access to memory is only done through special load and store instructions.
   - These instructions come in a number of variants to be able to load values of different bit-size.
   - The LD (Load Doubleword) is only supported on RV64I as it loads a 64-bit value.
   - A word refers to a 32-bit value, so LW (Load Word) could be used to load a regular 32-bit integer. While working with strings you may want to load individual bytes by using LB.
   - First notice that, if instruction has immediate, then it uses at most 2 registers (1 src, 1 dst).
   - Key difference: Only **imm** field is different from R-format: rs2 and funct7 replaced by 12-bit signed immediate, mm[11:0]

     - opcode (7): uniquely specifies the instruction
     - rs1 (5): specifies a register operand
     - rd (5): specifies destination register that receives result of computation
     - immediate (12): 12 bit number
     		- All computations done in words, so 12-bit immediate must be extended to 32 bits
      		- always sign-extended to 32-bits before use in an arithmetic operation
     - Can represent 2<sup>12</sup> different immediates i.e., imm[11:0] can hold values in range [-2<sup>11</sup> , +2<sup>11</sup>)
     - example: RISCV Instruction: **addi x15,x1,-50**
       
	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/ebebf09a-3f65-4e84-b630-6417fd436284)
	
 	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/9c12dca3-0a11-4269-8646-83a8a1a2ebee)

	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/f2e3efc5-d583-4991-a169-7d30239accd3)

   - **Load Instructions are also I-Type**

     ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/ba2c187d-9542-4592-8285-82cb90d4ecbb)

     	- The 12-bit signed immediate is added to the base address in register **rs1** to form the memory address
		– This is very similar to the add-immediate operation but **used to create address, not to create final result**
	- Value loaded from memory is stored in rd
 	- Example:  **lw x14, 8(x2)**
   	    ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/5b2e27b8-0714-408b-b610-1750b8627624)



4. **S-format** (store)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/379041a4-e802-4c65-9a8b-7e23e62c151c)

    ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/15d6c10b-4a19-47ea-a2f6-feea2dd2be79)

   - The characteristic of S-type instruction is that **there is no rd register**. In this type of instruction, the **immediate is divided into two parts**, the first part is in bit11-5, and the second part is in bit4-0. The 5 bits of the immediate 4-0 occupy the position of rd in other instruction formats, and 5-11occupy the position of funct7. Explain that the command format does not need to write back. That is, read the two values from the two registers and perform the operation together with theimmediate, and write the result to the register after the operation is over.

5. **U-format** (Upper immediate)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/d55d7d53-13da-48e8-861d-09eb6c919067)

6. **SB-format** (Branch)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/7f3718f4-fff8-40b4-9bfe-e9f8234ec1d4)

7. **UJ-format** (jump)

	**Subroutine calls, jumps (UJ), and branches (SB)**
	- RISC-V's subroutine call jal (jump and link) places its return address in a register. This is faster in many computer designs, because it saves a memory access compared to systems that push a return address directly on a stack in memory. jal has a 20-bit signed (two's complement) offset. The offset is multiplied by 2, then added to the PC to generate a relative address to a 32-bit instruction. If the result is not at a 32-bit address (i.e., evenly divisible by 4), the CPU may force an exception
	 - RISC-V CPUs jump to calculated addresses using a jump and link-register, jalr instruction. jalr is similar to jal, but gets its destination address by adding a 12-bit offset to a base register. (In contrast,jal adds a larger 20-bit offset to the PC.) 

	![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c1edc5bb-06e1-440c-b9a0-6b2d5a2a1f41)

  ## RISC-V ISA cheatsheet
  - 47 base instructions modular ISA
     [RISC-V-cheatsheet-RV32I-4-3.pdf](https://github.com/nkrvlsi/VSDSquadron_Labs/files/15483063/RISC-V-cheatsheet-RV32I-4-3.pdf)

Another useful thing to point out about this sheet is the pseudo instructions. They are not real instructions supported by RISC-V processors. Instead, they are just convenient ways of writing other instructions. For instance, if I want to move the value of one register say x3 to x4 then it would be nice if RISC-V had a move instruction. MV x4, x3 would accomplish this, except it doesn’t really exist. Why? Because the same can be accomplished with:

	**ADDI x4, x3, 0  # x4 ← x3 + 0**
 
That means you can avoid adding encoding for an MV instruction to the instruction-set architecture (ISA).

One great example of the benefits of pseudo instructions is the LI and LA instructions. Because all RISC-V instructions must be 32-bit wide, they cannot contain a full 32-bit address. Thus loading a 32-bit address into a register has to be done as a two-step process. First, we load the top 20 bits with either LUI or AUIP and then we add the remaining 12 bits with ADDI.
```
	.section .text             # Mark code section
	  LUI  a1,     %hi(msg)    # Load upper 20 bits of msg address
	  ADDI a1, a1, %lo(msg)    # Load lower 12 bits of msg address
	  CALL puts                # Call puts function to show string
	loop:
	  J loop                   # Jump to loop - Infinite loop
	.section .data             # Mark section for R/W data storage
	  msg: .string "Hello World\n"
```
To create code that can be loaded into any memory address (position independent code) we use the LA instruction which translated into AUIP and ADDI.

By using pseudo-instructions we greatly simplify this code:

``` 
	.section .text             # Mark code section
	  LI a1, msg               # Load immediate. Julia expands to 
	                           # multiple instructions as needed.
	  CALL puts                # Call puts function to show string
	loop:
	  J loop                   # Jump to loop - Infinite loop
	.section .data             # Mark section for R/W data storage
	msg: .string "Hello World\n"
```

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c7a89676-efad-4040-8297-1ba7fe1b2e59)



