# rv32i
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
### 1. Tools used:  iverilog and GTKwave
1. Verilog compiler   - **Icarus Verilog**             -> open source
2. waveform viewer    - **GTKWave Analyzer V3.3.86**   -> open source
    
### 2. setup
1. Installing iverilog and gtkwave - Ubuntu
      - open terminal and type following command
      - cmd: **sudo apt-get update**
      - cmd: **sudo apt-get install iverilog gtkwave**
2. Clone github repository
      - cmd: **git clone https://github.com/vinayrayapati/rv32i**
      - cmd: **cd rv32i**
### 3. Simulation
1. for compilation and simulation use below command
      - cmd: **iverilog iiitb_rv32i.v iiitb_rv32i_tb.v -o output_file**
           - it will create iiitb_rv32i.vcd dump file that can be used to check waveform
           - here -o is to tell output file (for now im not seeing any use case with this file)
### 4. output checking using GTKWave
1. Open gtkwave and load vcd file  
        - cmd: **gtkwave iiitb_rv32i.vcd &**  

2. Instructions performed in a 5-stage pipeline ISA.  
     1. Instruction **Fetch** (IF)    - Read instruction from instruction memory.
     2. Intrction **Decode** (ID)     - Read program registers & identify type of operation.
     3. **execute**                   - Compute value or address. performs type of opertation on data. finally ALU out is generated.
     4. **Memory** access (Mem Rd/Wr) - Read or write back data. Load(I) & Store(S) instructions transfer value between register <----> memory
     5. **Write Back** (WB)           - program registers. Result of the instruction will be written back to the register.

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
steps3,4,5,6,7 explaind in below image. **Note: RTL side PC is incremented by 1 only(suppose to be +4).**
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

