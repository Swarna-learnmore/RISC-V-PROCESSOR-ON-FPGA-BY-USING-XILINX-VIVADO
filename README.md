# RISC-V-PROCESSOR-ON-FPGA-BY-USING-XILINX-VIVADO
8 bit and 5 Stage (Fetch, Decode, Execution, Memory, Register writeback) single cycled without pipeline RISC-V processor. Performing Addition, Multiplication, Fibonacci operations.


RISC-V is an Instruction set architecture developed by the University of California, Berkeley. The concept of RISC was motivated by the truth that most of the processor instructions were not utilized by most computer programs. So, unnecessary decoding logic was being utilized within the designs of processors, consuming more power as well as area. RISC-V is an open-source ISA that is royalty-free and can be used for any purpose1.It offers innovative operational mechanisms and has a large number of CPU designs.

we have implemented basically 3 mathematical operations those are addition, multiplication, and fibonacci series by using RISC-V Processor.

**Architecture of RISC-V Processor**
![image](https://github.com/Swarna-learnmore/RISC-V-PROCESSOR-ON-FPGA-BY-USING-XILINX-VIVADO/assets/139202780/5772a99f-81ef-45c2-b010-fcbabca08bba)

**1.Fetch Stage**

The fetching stage in a RISC-V processor is the initial step in the instruction execution pipeline. It involves retrieving the next instruction from memory. The program counter (PC) holds the address of the next instruction to be fetched. The PC is incremented to point to the next instruction after the current one is fetched. The instruction is then passed to the decoding stage for further processing. The fetching stage plays a crucial role in the processor's performance, as it determines the rate at which instructions are fetched and processed, directly impacting the overall execution speed of the RISC-V processor.

**2.Decode Stage**

The decoding stage in a RISC-V processor is the second step in the instruction execution pipeline. Once an instruction is fetched, it enters the decoding stage where it is decoded and prepared for execution. During this stage, the instruction's opcode and operands are identified, and the necessary control signals are generated to facilitate the execution of the instruction. The decoding stage interprets the instruction and determines the sequence of operations that need to be performed. It also handles any data dependencies and resolves any resource conflicts that may arise. The decoded instruction is then passed on to the subsequent stages of the pipeline for execution.

**3.Execution Stage**

The execution stage in a RISC-V processor is the third step in the instruction execution pipeline. Once an instruction is decoded, it enters the execution stage where the actual computation or operation specified by the instruction is performed. This stage involves executing arithmetic and logical operations, data manipulation, memory accesses, and control flow operations. The execution stage utilizes the control signals generated during the decoding stage to carry out the required operation. The operands are fetched from registers or memory, and the result of the computation is calculated. The execution stage plays a critical role in the overall performance of the processor, as it directly affects the speed and efficiency of instruction execution.

**4.Memory stage**

The memory stage in a RISC-V processor is the fourth step in the instruction execution pipeline. This stage primarily deals with memory-related operations, such as data loads and stores. During the memory stage, the necessary memory addresses are calculated based on the instruction and the data being operated on. If the instruction is a load, the memory stage retrieves the data from the specified memory address and prepares it for the next stage. If the instruction is a store, the memory stage writes the data from the register to the specified memory address. The memory stage also handles any necessary memory access synchronization and ensures data consistency. It plays a crucial role in enabling the processor to interact with memory effectively and efficiently.

**5.Register Writeback stage**

The register writeback stage in a RISC-V processor is the final step in the instruction execution pipeline. In this stage, the results of the executed instruction are written back to the destination registers. The executed instruction produces a result that needs to be stored in a register. This stage updates the register file with the computed value. The register writeback stage ensures that the updated value is available for subsequent instructions that may require it as an operand. It finalizes the execution of the instruction and completes the data flow through the processor. This stage is crucial for maintaining the consistency and correctness of the register values throughout the program execution.

**General Purpose Registers**
R0 through R3 are  general purpose registers.

![image](https://github.com/Swarna-learnmore/RISC-V-PROCESSOR-ON-FPGA-BY-USING-XILINX-VIVADO/assets/139202780/ce238f9d-3b4c-4be5-8cd4-70c015ef1f92)

**Program Counter**
PC is the program counter, which always points to the currently running instruction or its argument. The PC is initialized at address 0x00, so all programs must start there.

**Stack Pointer**
SP is the Stack Pointer, which starts pointing to the end of the RAM memory (address 0xFF), as the stack grows towards lower memory locations.
The stack is used by four instructions: PUSH, POP, BSR, and RET. 
PUSH and POP are used for pushing and popping the general purpose registers to and from the stack, whereas BSR (Branch to SubRoutine) and RET (Return) push and pop the Program Counter respectively.

**Memory Model**
We implemented a  Harvard Architecture, meaning that it has two separate addressing spaces for a Program Memory (ROM) and a Data Memory (RAM), both able to store up to 256 bytes. That’s why the PC and SP are both 8-bits wide.
The Program Memory is Read-Only, and it’s implemented in a separate Verilog Module where programmers can write their code with a reasonably easy syntax. The Data Memory is accessible for reading and writing, and it’s implemented inside the program, so its bus is not accessible to the outside world. Following the RISC philosophy, access to this memory is limited to Load, Store, and Stack instructions (PUSH, POP, BSR, RET).  

**I/O Model**
It  has 4 8-bit input ports and 4 8-bit output ports, all with a dedicated strobe output signal. Each Register is tied to one input port and one output port. For example, the incoming data at input port 2 can only enter main program through register R2, and the outgoing data that must go to output port 0 can only be sent through register R0.
The strobe outputs signal the external hardware the instant when the data has moved. This signal is a low pulse, where the falling edge indicates the read or write instant. An input strobe informs the external hardware that the incoming data has been consumed, so the external hardware may queue that value and place a new one, or switch to an end-of-transmission code.
An output strobe informs the external hardware that the outgoing data has been sent, so the external hardware must consume the data at that instant using the strobe signal as a write input. After the low-pulse in the strobe signal, the data in the output port may change at any time. 

**Addressing Modes**
1.Register - For operations between 2 registers, for example ADD R1, R2

2.Immediate - For loading and comparing: LD R1, #43   CMP R3, #22

3.Direct  - For load and store: LD R3,102   ST R0,0x3F

4.Indirect - For load and store: LD R3,(R2)   ST R0,(R1)

5.Absolute - For branches, for example BGT 201

6.Implicit - No operands: NOP and RET 


**Operators**

{}   Concatenation of registers.

()   Indirection of register for indirect addressing mode. 

+   Addition
  
-   Subtraction
  
*   Multiplication

← Is loaded with. This is the assign operator.

&  Bitwise AND

|   Bitwise OR

~   Bitwise NOT

**Operands**
Ra The first register operand for instructions with the register addressing mode.

Rb The second register operand for instructions with the register addressing mode.

K The immediate argument for instructions with the immediate addressing mode.

Addr Address in data memory instructions with the direct addressing mode.

Tgt Target address in program memory for instructions with the absolute addressing mode.

**Condition Flags**

V -Overflow Flag

N- Negative Flag

Z - Zero Flag

C - Carry Bit

                        **Executed operations**
                        
**1.Addition**

Operation:
Ra ← Ra + Rb

Addressing Mode:
Register.

Source Form:
ADD Ra, Rb

Ra is one of the addition operands as well as the target register. It may be any of the CPU registers: R0, R1, R2, R3.

Rb is the other addition operand. It may be any of the CPU registers: R0, R1, R2, R3.

**2.Fibonacci**

Description:
a series of numbers in which each number (Fibonacci number) is the sum of the two preceding numbers. The simplest is the series 1, 1, 2, 3, 5, 8, etc.

Operation:

Let  R1,R2,R3,R4 are 4 variable numbers

R1=R1,  R2=R1,  R3=R1+R2, R4=R2+R3

Addressing Mode:
Register

**3.Multiplication**

Description:
Multiplies two CPU registers and produces a 16-bit result if the registers are distinct, or an 8-bit result if the square of a single register is wanted. Both operands are modified.

Operation:

{Rb, Ra} ← Ra * Rb     if Ra and Rb are distinct

or

Ra ← low_byte(Ra * Rb) if Ra and Rb are the same register

Addressing Mode:
Register.

Source Form:
MUL Ra, Rb

Ra is one of the multiplication operands and always the target for the low byte of the 16-bit product. It may be any of the CPU registers: R0, R1, R2, R3.

Rb is the other multiplication operand, and optionally the target for the high byte of the 16-bit result if it is not the same register as Ra. It may be any of the CPU registers: R0, R1, R2, R3.

If you have any queries mail to the Author.


**Author Details:**

swarnalathadigumarthi@rguktn.ac.in


