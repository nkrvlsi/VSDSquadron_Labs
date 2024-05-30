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
     3. **execute**                   - Compute value or address. performs type of opertation on data.
     4. **Memory** access (Mem Rd/Wr) - Read or write back data. Load(I) & Store(S) instructions transfer value between register <----> memory
     5. **Write Back** (WB)           - program registers. Result of the instruction will be written back to the register file.

3. Check waveform:
   1. Check clk running and reset release
      
      <img width="335" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/b101b49a-a54a-48c6-ac6c-127a3f56156d">

   2. instruction types (arithmetic_type=7'd0, memory_type=7'd1, branch_type=7'd2 & shift_type=7'd3) will be checked using ID_EX_IR[6:0] as below
      
     <img width="353" alt="image" src="https://github.com/nkrvlsi/VSDSquadron_Labs/assets/170950241/c7f0a0a9-7e8e-46ed-9d47-c4cf43a020ef">
     
     - here we can see clearly shift type (value should be 3) we are not excercized but rest all we excercized.
 
   3. 

