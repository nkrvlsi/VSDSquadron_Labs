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
	- RISC-V processors have following 5 different **Instruction Cycles (IC)**
 		- 1.Fetch 2.decode 3.execute 4.Memory 5.writeback
   - **instruction cycle (IC)** consists of several steps, each performs a specific function in the execution of the instruction. The major steps in the instruction cycle are:

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

### 1. <ins>R-format</ins> (Register)	- Arithmetic and logical operations

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e55907f2-0002-4563-83e7-8dc05b3712da)

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

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/04038e2b-6c21-4eb0-9c06-a9fbef6a0d4e)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/eac5ae74-b48f-47ba-adf4-f5de51bd106d)


### 2. <ins>I-format</ins> - (immediate, loads)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/81669faf-9209-44bd-aab6-5cb53586526e)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/a5afbf04-adf2-442a-b582-f929abcd8850)

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

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/fcddbb52-5f5b-4362-aefb-16c89079c33e)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e29b28fe-ec73-432f-9f5a-d79246c0272c)

### 3. <ins>S-format</ins> (store)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/45eeb516-1ec6-4caf-9b11-456f3e4718f9)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/f5e9f74e-60ae-4144-8842-90b484fa850c)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/15d6c10b-4a19-47ea-a2f6-feea2dd2be79)

   - The characteristic of S-type instruction is that **there is no rd register**.
   - Store needs to read two registers, **rs1** for base memory address, and **rs2** for data to be stored, as well as need immediate offset!.
   - Can’t have both rs2 and immediate in same place as other instructions!
   - Note: stores don’t write a value to the register file, no rd!
   - RISC-V design decision is move low 5 bits of immediate to where rd field was in other instructions – keep rs1/rs2 fields in same place
   - register names more critical than immediate bits in hardware design.
   - Example: sw x14, 8(x2)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/f5bc6fe0-5cdc-4606-9168-5a175f2489fb)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e7cdddb7-2965-4a54-93c5-5c42d00692b9)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/379041a4-e802-4c65-9a8b-7e23e62c151c)

### 4. <ins>U-format</ins> (Upper immediate)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/d55d7d53-13da-48e8-861d-09eb6c919067)

   - **How do we deal with 32-bit immediates?**
   		- Our I-type instructions only give us 12 bits
   - **Solution:** Need a new instruction format for dealing with the rest of the 20 bits.
   - This instruction should deal with:
   		- a destination register to put the 20 bits into
   		- the immediate of 20 bits
   		- the instruction opcode

   **<ins>Upper Immediate Instructions format</ins>:**    

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/157cd689-08ac-4848-831d-f23f5f21e195)

- Has 20-bit immediate in upper 20 bits of 32-bit instruction word
- One destination register, rd
- Used for two instructions
  	- **LUI** – Load Upper Immediate
  	- **AUIPC** – Add Upper Immediate to PC
- LUI to create long immediates:
  	- lui writes the upper 20 bits of the destination with the immediate value, and clears the lower 12 bits.
  	- Together with an addi to set low 12 bits, can create any 32-bit value in a register using two instructions (lui/addi).
  	  	- lui x10, 0x87654 	# x10 = 0x87654000
  	  	- addi x10, x10, 0x321 	# x10 = 0x87654321

- **Corner Case**:
- How to set 0xDEADBEEF?
  	- lui x10, 0xDEADB 	# x10 = 0xDEADB000
  	- addi x10, x10,0xEEF 	# x10 = 0xDEADAEEF
- addi 12-bit immediate is always sign-extended!
  	- if top bit of the 12-bit immediate is a 1, it will subtract -1 from upper 20 bits

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/e841facc-a6cd-4c2c-96b3-ba0548bffe74)

-  **AUIPC**: Add upper immediate value to PC
  	- Adds upper immediate value to PC and places result in destination register
-  sed for PC-relative addressing
  	- **Label: auipc x10, 0**
   - Puts address of label into x10
      
### 5. <ins>SB-format</ins> (Branch)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/7f3718f4-fff8-40b4-9bfe-e9f8234ec1d4)

   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/bc390bf3-a21c-46ec-a20e-6fb71fbc55ee)

**<ins>Branching Instructions</ins>:**
   - *beq, bne,bge,blt*
   		- Need to specify an **address** to go to
   		- Also take **two registers** to compare
   		- Doesn’t write into a register (similar to stores)
   - How to encode label, i.e., where to branch to?
     
**<ins>Branching Instruction Usage</ins>:** 
   - Branches typically used for loops (if-else, while, for)
   		- Loops are generally small (< 50 instructions)
   - **Recall:** Instructions stored in a localized area of memory (Code/Text)
   		- Largest branch distance limited by size of code
     	- Address of current instruction stored in the program counter (PC)
         
**<ins>PC-Relative Addressing</ins>:**
   - PC-Relative Addressing: Use the immediate field as a two’s complement offset to PC
     	- Branches generally change the PC by a small amount
     	- Can specify ± 2<sup>11</sup> addresses from the PC
   - Why not use byte address offset from PC as the immediate?
     
**<ins>Branching Reach</ins>**:
   - Recall: RISCV uses 32-bit addresses, and memory is byte-addressed
   - Instructions are “word-aligned”: Address is always a multiple of 4 (in bytes).
   - PC ALWAYS points to an instruction
     	- PC is typed as a pointer to a word
     	- can do C-like pointer arithmetic
   - **Let immediate specify #words instead of #bytes**
     	- Instead of specifying ± 2<sup>11</sup> bytes from the PC, we will now specify ± 2<sup>11</sup> words = ± 2</sup>13</sup> byte addresses around PC
     	  
**<ins>Branch Calculation</ins>:**
   - If we **don’t** take the branch:  
     		  **PC = PC+4** = next instruction  	
   - If we **do** take the branch:  
     		  **PC = PC + (immediate*4)**
   - **Observations**:
     	- *immediate* is number of instructions to move (remember, specifies words) either forward (+) or backwards (–)
     	  
**<ins>RISC-V Feature, n×16-bit instructions</ins>:**  
   - Extensions to RISC-V base ISA support 16-bit compressed instructions and also variable-length instructions that are multiples of 16-bits in length
   - 16-bit = half-word
   - **To enable this, RISC-V scales the branch offset to be half-words even when there are no 16-bit instructions.**
   - Reduces branch reach by half and means that ½ of possible targets will be errors on RISC-V processors that only support 32-bit instructions (as used in this class)
   - RISC-V conditional branches can only reach ± 2<sup>10</sup> × 32-bit instructions either side of PC.
     
**<ins>RISC-V B-Format for Branches</ins>:**
   - B-format is mostly same as S-Format, with two register sources (rs1/rs2) and a 12-bit immediate
   - But now immediate represents values -2<sup>12</sup> to +2<sup>12</sup>-2 in 2-byte increments
   - The 12 immediate bits encode even 13-bit signed byte offsets (lowest bit of offset is always zero, so no need to store it)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/2ed148f6-3aba-4bd2-b384-6e5152568739)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/58b86c58-e531-4a5f-bac6-8f880358a8c1)
![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/1c391ffe-593c-43c0-a18f-3e56b46e73c4)
![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/d800f301-fcd4-43db-8dc3-020e3241c505)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c8da153d-2b58-4922-8a21-39fc516aa407)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/87cd9bb8-f71a-4bb6-8e5a-ad0515874f01)

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/a15e648f-8105-4db5-9263-d5da20825661)
     
### 6. <ins>UJ-format</ins> (jump)

**Subroutine calls, jumps (UJ), and branches (SB)**  

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/5e5a198d-04e2-477b-8005-00c977faffe1)

**<ins>UJ-Format Instructions</ins>:**
- For branches, we assumed that we won’t want to branch too far, so we can specify a **change** in the PC
- For general jumps (**jal**), we may jump to **anywhere** in code memory
  	- Ideally, we would specify a 32-bit memory address to jump to
  	- Unfortunately, we can’t fit both a 7-bit *opcode* and a 32-bit address into a single 32-bit word
  	- Also, when linking we must write to an **rd** register

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/a279fd9a-9b45-456d-9996-18a303f19713)

- jal saves **PC+4 in** register **rd** (the return address)
- Set PC = PC + offset (PC-relative jump)
- Target somewhere within ±2<sup>19</sup> locations, 2 bytes apart
- ±2<sup>18</sup> 32-bit instructions
- **Reminder:** “j” jump is a pseudo-instruction—the assembler will instead use jal but sets rd=x0 to discard return address.
- Immediate encoding optimized similarly to branch instruction to reduce hardware cost
- **j pseudo-instruction**
- j Label = jal x0, Label # Discard return address
- **Call function within 2<sup>18</sup> instructions of PC**
- **jal ra, FuncName**
- Why is the immediate so funky?
  	- Similar reasoning as for branch immediates
![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c1edc5bb-06e1-440c-b9a0-6b2d5a2a1f41)

**<ins>*jalr* Instruction (I-Format)</ins>:**

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/0d17ff61-6a8e-407f-95a0-8d9be7445433)
- **jalr rd, rs1, offset**
- Writes PC+4 to rd (return address)
- Sets PC = rs1 + offset
- Uses same immediates as arithmetic & loads
  	- **no** multiplication by 2 bytes
  	  
**<ins>Uses of *jalr*</ins>:**
 
 ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/1fc9cb5a-7f8f-4c08-bf1b-865ffa8df52b)
 
- ret and jr psuedo-instructions
  	- ret = jr ra = jalr x0, ra, 0  
- Call function at any 32-bit absolute address  
  	lui x1, <hi 20 bits>  
  	jalr ra, x1, <lo 12 bits>  
- Jump PC-relative with 32-bit offset  
  	auipc x1, <hi 20 bits>
  	jalr x0, x1, <lo 12 bits>

**Question: When combining two C files into one executable, we can compile them independently and then merge them together.**
![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/9f7dc1a5-09fa-45e1-b1c4-99365bfda2f0)
  

  ## RISC-V ISA cheatsheet

[RISC-V-cheatsheet-RV32I-4-3.pdf](https://github.com/nkrvlsi/VSDSquadron_Labs/files/15483063/RISC-V-cheatsheet-RV32I-4-3.pdf)

Another useful thing to point out about this sheet is the pseudo instructions. They are not real instructions supported by RISC-V processors. Instead, they are just convenient ways of writing other instructions. For instance, if I want to move the value of one register say x3 to x4 then it would be nice if RISC-V had a move instruction. MV x4, x3 would accomplish this, except it doesn’t really exist. Why? Because the same can be accomplished with:  
		- **ADDI x4, x3, 0  # x4 ← x3 + 0**  
 
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


Thats concludes TASK2.

# TASK3: rv32i processor
======================================================
## 1. Project: RISC-V RV32I
This project is to undertand few instructions in single cycle using RISC-V ISA. Here we use 32-bit RISC-V processor with 32-bit General Purpose Registers (GPR's) so all the instructions are 32-bit wide. 

## 2. BLOCK DIAGRAM OF RISC-V RV32I

<img width="527" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/4963f044-89eb-426d-a05d-dd212cb7ed00">

## 3. INSTRUCTION SET OF RISC-V RV32I

Instructions we use here are classified in 5 types.
1. <slt>Arithmetic type</slt>  
   - i) **ADD**-addition, 2) **SUB**-subtraction, 3) **AND**-logical and, 4) **OR** -logical or, 5) **XOR** - logical xor 6) **SLT** - set less than  
2. <slt>Immediate Type</slt>
   - i) **ADDI**-add immediate, 2) **SUBI**-subtract immediate , 3) **ANDI**-local and immediate, 4) **ORI**-logical or immediate 5) **XORI**-logical xor immediate         
3. <slt>Memory type</slt>
   - i) **LW**-load word  2) **SW**-store word  
4. <slt>Branch Type</slt>  
   - i) **BEQ**-Branch if quals to 2) **BNE**-Brnach if not equals to  
5. <slt>Shift type</slt>  
   - i) **SLL**-Shift left logic 2) **SRL**-Shift right logic
  
<img width="412" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/857af80f-7751-48ef-a793-61651805cd85">

## 4. Functional simulation using verilog files
### 4.1. Tools used:  iverilog and GTKwave
1. Verilog compiler   - **Icarus Verilog**             -> open source
2. waveform viewer    - **GTKWave Analyzer V3.3.86**   -> open source
    
### 4.2. setup
1. Installing iverilog and gtkwave - Ubuntu
      - open terminal and type following command
      - cmd: **sudo apt-get update**
      - cmd: **sudo apt-get install iverilog gtkwave**
2. Clone github repository
      - cmd: **git clone https://github.com/vinayrayapati/rv32i**
      - cmd: **cd rv32i**
### 4.3. Simulation
1. for compilation and simulation use below command
      - cmd: **iverilog iiitb_rv32i.v iiitb_rv32i_tb.v -o output_file**
           - it will create iiitb_rv32i.vcd dump file that can be used to check waveform
           - here -o is to tell output file (for now im not seeing any use case with this file)
### 4.4. output checking using GTKWave
1. Open gtkwave and load vcd file  
        - cmd: **gtkwave iiitb_rv32i.vcd &**  

2. Instructions performed in a 5-stage pipeline ISA.  
     1. Instruction **Fetch** (IF)    - Read instruction from instruction memory.
     2. Intrction **Decode** (ID)     - Read program registers & identify type of operation.
     3. **execute**                   - Compute value or address. performs type of opertation on data.
     4. **Memory** access (Mem Rd/Wr) - Read or write back data. Load(I) & Store(S) instructions transfer value between register <----> memory
     5. **Write Back** (WB)           - program registers. Result of the instruction will be written back to the register file.

3. Check waveform:
   1. Check clk running and reset release
      
      <img width="446" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/60d0613a-707e-4326-bfbe-dbc170b7b99c">


   2. instruction types (arithmetic_type=7'd0, memory_type=7'd1, branch_type=7'd2 & shift_type=7'd3) will be checked using ID_EX_IR[6:0] as below
      
     ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/7476e52e-d334-4194-ac54-e9fdbf9368a6)

     - here we can see clearly shift type (value should be 3) we are not excercized but rest all we excercized.
 
   3. check if condition ID_EX_IR[6:0]=0 (tells AR_Type) and ID_EX_IR[31:25]== 7'd1 (tells arithmetic and logical operation type).
         - ID_EX_IR[14:12] tells type of operation to perform: ADD=3'd0, SUB=3'd1, AND=3'd2, OR=3'd3, XOR=3'd4, SLT=3'd5
   4. check if condition ID_EX_IR[6:0]=0 (tells R_Type) and ID_EX_IR[31:25]== 7'd0 (tells I_type).
         - ID_EX_IR[14:12] tells type of operation to perform:   ADDI=3'd0, SUBI=3'd1, ANDI=3'd2, ORI=3'd3, XORI=3'd4
   5. check if condition ID_EX_IR[6:0]=1 (tells M_Type)
         - ID_EX_IR[14:12] tells type of operation to perform: LW=3'd0, SW=3'd1
   6. check if condition ID_EX_IR[6:0]=2 (tells BR_Type)
         - ID_EX_IR[14:12] tells type of operation to perform: BEQ=3'd0, BNE=3'd1
   7. check if condition ID_EX_IR[6:0]=3 (tells SH_Type)
          - ID_EX_IR[14:12] tells type of operation to perform: SLL=3'd0, SRL=3'd1
steps3,4,5,6,7 explaind in below image.
<img width="805" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/cdeac2c5-11b2-487b-b2d6-6bb7b4a2c6e7">

**<ins>R-Type:</ins>**  

   1. ADD:EX_MEM_ALUOUT <= ID_EX_A + ID_EX_B;  
   <img width="375" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/12ce1bad-9487-459c-b5de-5bdbcd9ff147">
   - ALU output is generated in the next posedge of clk

   2. SUB:EX_MEM_ALUOUT <= ID_EX_A - ID_EX_B;  
   <img width="317" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/7190dcec-0bb8-47ba-9293-476666723529">

   3. AND:EX_MEM_ALUOUT <= ID_EX_A & ID_EX_B;  
   <img width="350" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c4e37e3b-3a25-454b-b455-c207b41afbd9">

   4. OR :EX_MEM_ALUOUT <= ID_EX_A | ID_EX_B;  
   <img width="369" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/55f23a71-ddc0-4e52-b4ea-97928446826a">

   5. XOR:EX_MEM_ALUOUT <= ID_EX_A ^ ID_EX_B;
   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/cf7e0ffd-3be9-4e3b-9ff3-9abc0848a365)

   7. SLT:EX_MEM_ALUOUT <= (ID_EX_A < ID_EX_B) ? 32'd1 : 32'd0;  
      - not excercized
        
**<ins>I-type:</ins>**

   1. ADDI:EX_MEM_ALUOUT <= ID_EX_A + ID_EX_IMMEDIATE;  
   ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c72daea8-cb6c-42c8-871e-0e8e3762e1c1)

   2. SUBI:EX_MEM_ALUOUT <= ID_EX_A - ID_EX_IMMEDIATE;
      - not excercized
   3. ANDI:EX_MEM_ALUOUT <= ID_EX_A & ID_EX_B;
      - not excercized
   4. ORI:EX_MEM_ALUOUT  <= ID_EX_A | ID_EX_B;
      - not excercized
   5. XORI:EX_MEM_ALUOUT <= ID_EX_A ^ ID_EX_B;  
      - not excercized
        
**<ins>M-type:</ins>**

   1. LW  :EX_MEM_ALUOUT <= ID_EX_A + ID_EX_IMMEDIATE;
      ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/b0486ded-6ffb-46c2-ae61-a9599af11230)

   2. SW  :EX_MEM_ALUOUT <= ID_EX_IR[24:20] + ID_EX_IR[19:15];
      ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/12d43a99-05e8-4ca6-ac8a-93f3d5c4f2cd)

**<ins>BR-type:</ins>**
      
   1. BEQ: EX_MEM_ALUOUT <= ID_EX_NPC+ID_EX_IMMEDIATE;
           BR_EN <= 1'd1 ? (ID_EX_IR[19:15] == ID_EX_IR[11:7]) : 1'd0;
      ![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/1da029c1-0e55-4538-ad96-db56e48931f4)

   2. BNE: EX_MEM_ALUOUT <= ID_EX_NPC+ID_EX_IMMEDIATE;
           BR_EN <= (ID_EX_IR[19:15] != ID_EX_IR[11:7]) ? 1'd1 : 1'd0;
      - not excercized
        
**<ins>SH-type:</ins>**

   1. SLL:EX_MEM_ALUOUT <= ID_EX_A << ID_EX_B;
      - not excercized
   2. SRL:EX_MEM_ALUOUT <= ID_EX_A >> ID_EX_B;
      - not excercized

**<ins>Conclusion:</ins>** Look into below diagram:

![image](https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/826de386-563b-4720-9385-5837ea3ee481)

   - above diagram we can clearly see that PC is incremented by 1 each clock
   - we know from block diagram we had 3 types of memory here, 1. register 2. IMEM - instruction Memory 3. DMEM - Data Memory
   - **Instrcion code** is preloaded into  IMEM as below  
     <img width="230" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/03e2be7f-57e8-4123-9532-6b7f607cbbed">
     
   - **Decode Stage** we are reading register data and writing into r1,r2 register.
        - loading A,B, RD, IR, Immediate, PC etc from register memory.  
     <img width="260" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/5f4ab972-74e9-4b12-a9fb-3f87af3ac665">

   - **Execution stage** we are performing certain operation between registers like add, sub, sll etc and final output of the operand is written to  EX_MEM_ALUOUT[31:0].
   - **Memory stage** from RTL side: computed EX_MEM_ALUOUT is written to MEM_WB_ALUOUT.
        - Incase of Mem type:
             - load **LW:MEM_WB_LDM <= DM[EX_MEM_ALUOUT];**
             - Store **SW:DM[EX_MEM_ALUOUT]<=REG[EX_MEM_IR[11:7]];**
   -  Finally **Write back** stage -> ALU output is written to a register. i.e., ex: REG[MEM_WB_IR[11:7]] <= MEM_WB_ALUOUT.

all signlas snapshot:  
<img width="808" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/bd84c5df-83c6-409b-a8a5-5f897209c392">

## 5. Synthesis
### 5.1 Synthesis
Synthesis is the process of **converting RTL code**, typically written in hardware description languages like Verilog or VHDL, **into a gate-level netlist**. It involves mapping the functionality specified in the RTL code to a library of standard cells, such as NAND, NOR, XOR gates, etc., provided by the target technology. 
   - Inputs    : RTL, Technology libraries, Constraints (Environment, clocks, IO delays etc.)
   - Outputs   : Netlist , SDC, Reports etc.
     
Synthesis takes place in multiple steps:
   - Converting RTL into simple logic gates.
   - Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
   - Optimizing the mapped netlist keeping the constraints set by the designer intact.
     
### 5.2 Required Files for Synthesis:
To perform synthesis effectively, several files are essential:  

**<ins>1. RTL Code<ins>:** The RTL code serves as the input, written in hardware description languages like Verilog or VHDL, which captures the desired behavior of the design.  
**<ins>2. Technology Libraries<ins>:** These libraries provide a collection of standard cells, gates, and other components specific to the target technology.  
**<ins>3. Constraint File<ins>:** The constraint file guides the synthesis tool, providing additional information and specifications for optimizing the netlist generation. It includes details such as timing constraints, power targets, and area requirements.  

### 5.2 Synthesizer: tool: Yosys
It is a tool we use to convert out RTL design code to netlist.  
**<ins>Yosys:</ins>**  
Yosys is a Verilog RTL synthesis framework to perform logic **synthesis, elaboration**, and converting a subset of the Verilog Hardware Description Language (HDL) into a **BLIF netlist**.  

**<ins>installing Yosys:</ins>**
   - **cmd:** **sudo apt install yosys**

Now you need to create a yosys_run.sh file , which is the yosys script file used to run the synthesis. The contents of the yosys_run file are given below 

```
# read design

read_verilog iiitb_rv32i.v

# generic synthesis
synth -top iiitb_rv32i

# mapping to mycells.lib
dfflibmap -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
proc; opt
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime,{D};strash;dch,-f;map,-M,1,{D}
clean
flatten
# write synthesized design
write_verilog -noattr iiitb_rv32i_synth.v
```
<img width="392" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/2f656d09-1b94-43d5-ad98-bc3674080538">

**<ins>running Yosys:</ins>**

termianl type below commands
   - **cmd: yosys**
   - **cmd: script yosysrun.sh**
     
Now **synthesized netlist is generated in iiitb_rv32i_synth.v** file. 

<img width="664" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/6147abf5-8c29-4661-aee2-1d092438c784">

## 6. Gate level simulation - GLS

GLS is generating the simulation output by running test bench with netlist file generated from synthesis as DUT. Netlist is logically same as RTL code, therefore, same test bench can be used for it. We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.  

### 6.1 Folllowing are the commands to run the GLS simulation:  

   **cmd: iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 verilog_model/primitives.v verilog_model/sky130_fd_sc_hd.v iiitb_rv32i_synth.v iiitb_rv32i_tb.v**  
   
