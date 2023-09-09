# RISC-V
# Table of Contents 
 - [Day0 - Installation](#Installation)<br>
 - [Day1 - Introduction to RISC-V ISA and GNU Compiler ToolChain](#Introduction-to-RISC-V-ISA-and-GNU-Compiler-ToolChain)<br>
  - [Day2 - Introduction to ABI and basic verification flow](#Introduction-to-ABI-and-basic-verification-flow)<br>
  - [Day3 - Digital logic with TL-Verilog and Makerchip](#Digital-logic-with-TL-Verilog-and-Makerchip)<br>
  - [Day4 - Basic RISC-V CPU micro-architecture](#Basic-RISC-V-CPU-micro-architecture)<br>
   - [Day5 - Complete pipelined RISC-V CPU micro-architecture](#Complete-pipelined-RISC-V-CPU-micro-architecture)<br>
   - [References](#References)<br>
     

 # Day 0

 # Installation
 <details>
   <summary>
     RISC Tools
   </summary>
   The following commands are used to install RISC tools:
   
```

sudo apt install libboost-all-dev
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh

```

After run type the following commands:

```

cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install

```

Set Path variable in .bashrc using the following commands:

```

gedit .bashrc
export PATH="/home/iswarya/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" #Type at last line # close the bashrc and type
source .bashrc

```
</details>

# Day 1

# Introduction to RISC-V ISA and GNU Compiler ToolChain

<details>
 <summary>
    Introduction to RISC-V basic keywords
 </summary>

**RISC-V**

 RISC-V (“risk-five”) is an instruction set architecture (ISA) rooted in reduced instruction set computer (RISC) principles. RISC-V is unique, even revolutionary, because it is a common, free, open-source ISA to which software can be ported, hardware can be developed, and processors can be built to support it.

**ISA**

An Instruction Set Architecture (ISA) is part of the abstract model of a computer that defines how the CPU is controlled by the software. The ISA acts as an interface between the hardware and the software, specifying both what the processor is capable of doing as well as how it gets done.The ISA provides the only way through which a user is able to interact with the hardware.

ISA also known as **Abstract Interface** and **Architecture of Computer**.

 
</details>

<details>
 <summary>
  From Apps to Hardware
 </summary>

 **Diagrammatic Representation**

![Diagrammatic Representation](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/04cf63c8-a085-45c8-9879-791dbaae9c32)

**System Software**

System Software is the interface between Hardware and User Applications.System Software includes
+ Operating Systems
+ Compiler
+ Assembler

**Operating Systems**

It converts apps into their respective assembly language program and then into binary code for the hardware to understand it.
It also 
+ handle IO operations
+ allocate memory
+ low level system functions

**Compiler**

It is a special program that translates a programming language's source code into Instruction sets(.exe file).

Instruction sets depends on the hardware that we are going to use.

**Assembler**

It converts Instruction sets into binary code(logic 1 & logic 0).

**Instruction Sets**

Initially, we get specifications from ISA and write a HDL (Hardware Description Language) code which get synthesized into a gate level design .Gate Level Design is then converted into respective layout(Hardware).

**Instruction Set Architecture** has
+ Pseudo Instructions
+ Base Integer Instructions(RV64I)
+ Multiply Extension(RV64M)
+ Single & Double precision floating point extension (RV64F & RV64D)
+ Application Binary Interface
+ Memory Allocation & Stack Pointer
</details>

<details>
 <summary>
  Labwork for RISC-V software toolchain
 </summary>

 Leafpad is a simple GTK+ based text editor with user interface similar to Notepad.
 
 Install Leafpad using the commands

 ```

sudo apt-get update
sudo apt-get -y install leafpad

```

**Write a C program which sum numbers from 1 to n:**

```
leafpad sum1ton.c

```

![Sum C program](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6d1d01f9-97a5-499c-94c9-0e0a649ec8bb.

**Output**

![C program output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/1308fb9e-56f8-48f9-a437-44b0b3b1d4ec)

**Compiling the Code and Deassemble using O1**

```

riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o Sum1ton.o Sum1ton.c
riscv64-unknown-elf-objdump -d Sum1ton.o | less

```

![Deassemble using O1](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/9095cca6-6531-4c52-868f-50ccd55fe776)

**Compiling the Code and Deassemble using Ofast**

```

riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o Sum1ton.o Sum1ton.c
riscv64-unknown-elf-objdump -d Sum1ton.o | less

```

 ![Deassemble using Ofast](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/ba5824ea-e9b8-4430-801f-c5c43a54d3cd)


**:q** -> for quit

**Spike Simulation and Debug**

Use this command to print output on the terminal

```

spike pk Sum1ton.o

```

**Debugging**

```

spike -d pk Sum1ton.o

```

**(spike) until pc 0 100b0** -----> makes the program counter to run from 0 to 100b0(memory address) after that we run manually to debug it.

![debug](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/ba75d0ed-ef53-43f6-a792-8036559fe886)

**lui**

The load upper immediate instruction (lui) loads the highest 16 bits of a register with a constant, and clears the lowest 16 bits to 0s.

![liu](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e473bb02-53b6-470f-93ed-29fee69853aa)

**addi**

The addi instruction performs an addition on both the source register's contents and the immediate data,
and stores the result in the destination register.

![addi](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/4953f1b3-8edb-4b3f-9a70-816e7b10b207)
</details>

<details>
 <summary>
  Number system for Unsigned and Signed numbers
  </summary>

 **Unsigned Numbers**

 Unsigned Integers are just like integers (whole numbers) but have the property that they don't have a + or - sign associated with them. Thus they are always non-negative (zero or positive).

 An n-bit unsigned number represents all numbers in the range 0 to (2^n − 1).
 
 ![unsigned integer](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/034e9fde-f70e-4ecb-a980-8a2b7e6d1165)

 **Bit**
 
A binary digit (bit) is the minimum unit of binary information stored in a computer system. A bit can have only two states, on or off, which are commonly represented as ones and zeros. The combination of ones and zeros determines which information is entered into and processed by the computer.

**Byte**

A byte is a unit of data that is eight binary digits(8 bits) long. A byte is the unit most computers use to represent a character such as a letter, number or typographic symbol.

**Word** -----> a word is 2 bytes (16 bits).

**Double Word** -----> a single unit of data expressing two adjacent words (64 bits).

**Signed Numbers**

The signed numbers have a sign bit so that it can differentiate positive and negative integer numbers.The signed binary number technique has both the sign bit and the magnitude of the number.Sign bit is the Most Significant Bit(MSB).

+ **0 as sign bit** ---> represents **positive** number

+ **1 as sign bit** ---> represents **negative** number

 An n-bit signed number represents all numbers in the range – (2^(n-1)-1) to + (2^(n-1)-1).

 **Example**

 + +108(10) = 01101100(2)
 + −108(10) = 11101100(2)

**1's Complement**

1’s complement of a binary number is another binary number obtained by toggling all bits in it, i.e., transforming the 0 bit to 1 and the 1 bit to 0.

Range of 1's complement is   – (2^(n-1) – 1) to + (2^(n -1) – 1).

**Example**

1's complement of "0111" is "1000"

**2's Complement**

The 2’s complement of a binary number is obtained by adding one to the 1’s complement of signed binary number.

Range of 2's complement is – (2^(n-1) ) to + (2^(n-1) – 1).

**Example**

![2's comp rep](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/7735bcbe-f35c-4d0b-b4e7-7f070a69f05b)
</details>
<details>
 <summary>
  Lab for Signed and Unsigned Numbers
 </summary>

 ![Datatypes](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/8196edc1-6a34-4bde-832c-f2fc17245de7)

 **Code for Unsigned number**
 
 ![unsigned c code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b068083a-ab9f-4a8b-a9ca-cd20a86b3e00)

 **Output**
 
![unsigned lab](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/34b4c11b-5eae-4320-8809-944473d1b217)

 **Code for Signed number**

![signed c code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/55f529a6-292a-4f0c-87a2-3ec4e48df90a)

 
 **Output**

![signed lab](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/be071507-7a33-43d5-93e5-929d6fe38049)
</details>

# Day 2

# Introduction to ABI and basic verification flow
<details>
 <summary>
   Application Binary Interface
 </summary>

 An application binary interface (ABI) is an interface between two binary program modules. Often, one of these modules is a library or operating system facility, and the other is a program that is being run by a user. ABI defines how your code is stored inside the library file, so that any program using your library can locate the desired function and execute it.

RISC-V belongs to little-endian memory addressing system.

Size of each Instruction set is 32 bit for risc64.

![ABI](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/02a5019d-ac56-42cc-9b81-243e2cd3524e)

![Registers name](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b99ccf6a-28c3-4018-ad44-c455f533da45)

**R-type** --->  instructions for register-register operations 

**I-type** --->  instructions for immediate and load operations

**S-type** --->  instructions for store operations 
</details>

<details>
 <summary>
   Lab using ABI function calls
 </summary>

 **Flowchart**

![flowchart](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b6b4ece5-d9a8-481b-9d24-e21c3d4752ea)

 **Code using ASM language**

![1to9_custom code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/8b9a19d4-9206-4e1e-882a-816be49ae863)

![load s](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/13646592-5031-40c0-b5c7-8a643ca9f617)


**Output**

![1to9 out](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/a67b4473-300e-484d-bcc3-0b6021da9c60)
</details>

<details>
 <summary>
  Basic verification flow using iverilog
 </summary>

 ![RISC-V CPU iverilog](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/9b108c14-7a70-4af5-824b-d4f9080335ac)

 **Code**
 
![riscv cpu iverilog code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e6c6f6c6-25fa-4e5b-8cd2-5c85c518b059)

**Contents of rm32im.sh**

![riscv cpu commands](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/1e6d238c-ac38-40d5-9b56-881afe39704a)
</details>

# Day 3

# Digital logic with TL-Verilog and Makerchip

<details>
 <summary>
  Combinational Logic
 </summary>
 
 **Logic Gates**

 A logic gate is a device that acts as a building block for digital circuits. They perform basic logical functions that are fundamental to digital circuits.

![Logic gates ](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/ea6a7f57-9c77-4a90-9453-17e9a056ecce)

**Combinational Circuits**

 A combinational circuit is a circuit in which the output depends on the present combination of inputs. Combinational circuits are made up of logic gates.

 ![comb circuits](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/01c154ad-1e6d-4edc-a06a-1470c3146cb0)

 **Boolean Operators**

 Boolean is a set of commands that can be used in almost every search engine, database, or online catalogue.  The most popular Boolean commands are AND, OR, and NOT. 

 ![boolean](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/bfa6b17e-8380-4a43-9712-d8a4ac83af09)

 **Multiplexer**
 
 A MUX functions as a multiple-input, single-output switch that allows multiple analog and digital input signals and to be routed through a single output line. It is also called as Data Selector.

 **2x1 MUX Representation**
 
![mux](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e05e3215-1a91-498d-9654-1b525da79d95)

**Verilog code for 2x1 MUX**

```
assign f = s ? x1 : x2;

```

**Chaining Ternary Operator**

![mux ternary ](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/7bac2096-cfa4-4f4d-8065-431154a24e82)

**Makerchip**

1.Go to makerchip.com

2.Click LAUNCH MAKERCHIP IDE

For example ,we choose fpga multiplier from tutorials examples

**Code**

![fpga mul code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/4e28a5e4-2f87-4fa0-9c47-47bacf678774)

**Diagram**

![fpga mul diagram](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/41fc367e-5cf3-4012-9c5f-522eed500c37)

**Waveform**

![fpga mul waveform](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/4ba783b0-6b0e-464e-8f57-16c29c28252f)

**Lab Makerchip Platform**

![lab makerchip](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/df88c8de-3a3c-4cfd-8cdc-ddc8215d6119)

**Lab Inverter**

![lab inverter](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/7347f0ce-e5e4-4dc3-b339-4a63391f0eb3)

**Lab And**

![lab and](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e8d3dca4-f83e-4f8e-8110-ac4ae237421a)

**Lab Or**

![lab or](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/830687b5-26df-43c2-8175-50943e0c3015)

**Lab Xor**

![lab xor](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e04c82a2-98a7-4e62-80ef-ec9280af0c95)

**Lab Vectors**

![lab vectors](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/65abe500-9fd1-436b-9f4e-62962f0dc003)

**Lab Mux**

![lab mux](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/2f468d92-9c51-4614-9319-d6e500c4f1b0)

**Lab Modified Mux**

![lab mux vectors](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d98dc25a-0f74-4a02-8a43-d9578a85f35c)


**Lab Combinational Calculator**

![lab comb calculator](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/28ebe75b-098c-4a3f-9d02-eccacb28996e)
</details>

<details>
 <summary>
  Sequential Logic
 </summary>

 **Sequential Circuits**

 The sequential circuit is a special type of circuit that has a series of inputs and outputs. The outputs of the sequential circuits depend on both the combination of present inputs and previous outputs. The previous output is treated as the present state. So, the sequential circuit contains the combinational circuit and its memory storage elements. A sequential circuit doesn't need to always contain a combinational circuit. So, the sequential circuit can contain only the memory element.

![sequential-circuits](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/4c507948-2983-4093-a28d-7520cca281ea)

Sequential Logic is sequenced by a clock signal.

![clock](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/acf11022-e12c-49b9-8c4f-56cd41fe1681)

**Values in Verilog**

Verilog provides 4 basic values,

a) 0 — logic zero or false condition

b) 1 — logic one, or true condition

c) x — unknown/undefined logic value. Only for physical data types.

d) z — high-impedance/floating state. Only for physical data types.

Constants in Verilog are expressed in the following format:

1. width 'radix value
2. width — Expressed in decimal integer. Optional, default is inferred from value.
3. 'radix — Binary(b), octal(o), decimal(d), or hexadecimal(h). Optional, default is decimal.
value — Any combination of the 4 basic values can be digits for radix octal, decimal or
hexadecimal.

+ 4'b1011 // 4-bit binary of value 1011
+ 234 // 3-digit decimal of value 234
+ 2'h5a // 2-digit (8-bit) hexadecimal of value 5A
+ 3'o671 // 3-digit (9-bit) octal of value 671
+ 4b'1x0z // 4-bit binary. 2nd MSB is unknown. LSB is Hi-Z.
+ 3.14 // Floating point
+ 1.28e5 // Scientific notation

**Sequential Logic Fibonacci Series**

![lab fibonacci](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/081091bd-a11d-4076-9259-e982800f8fb2)

**Sequential Logic Counter**

![lab counter](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/871c0a4a-f704-43ad-97b4-1c2d062e75f2)

**Sequential Logic Calculator**

![sequential calc output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e9bb969f-af23-40f7-b06a-7a821ce697fc)

</details>

<details>
 <summary>
  Pipelined Logic
 </summary>

 Pipelining is the process of storing and prioritizing computer instructions that the processor executes. The pipeline is a "logical pipeline" that lets the processor perform an instruction in multiple steps. The processing happens in a continuous, orderly, somewhat overlapped manner.

 Let's compute using pythagoras theorem.

 ![Pythagoras dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6df41352-1097-4f77-b25c-b72555ffcda3)

 **Diagrammatic representation of hardware**

 ![pythagoras hardware structure](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/15deba0a-91d3-4f6d-8f5f-aa5ad0989cd9)

 **RTL**

 ![RTL](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/2a66c3e4-b850-4693-b989-9f8cce6933e4)

 **Timing Abstract**

 ![Timing Abstract](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/60db3c21-6af5-490e-a02f-60484e4df412)

 + Stage 1: Square the inputs a and b.
 + Stage 2: Add the squared inputs
 + Stage 3: Take squareroot of the determined output.

**TL-Verilog code**

![pyth tl verilog](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/29dec9c6-e423-4a0a-95c1-70e2e828abab)

**System Verilog code**

![system verilog](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d0d6b857-be3c-494e-9cc8-9b4f3f88fa9d)

**Diagram and Waveform on Makerchip**

![pytha mc](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6ba38bd4-4c90-46e4-812a-461ed0ad4ca0)

**Retiming**

Retiming is the technique of moving the structural location of latches or registers in a digital circuit to improve its performance, area, and/or power characteristics in such a way that preserves its functional behavior at its outputs.

**Modified TL-Verilog code**

![modified tl verilog](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/74b68fe4-bc3a-4cae-bae0-41b39d89d519)

**Modified Timing Abstract**

![modified timing abstract](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/192bf2aa-6925-4533-8d87-a4b83d8917fe)

The pipelining allows us run at high clock frequency ,so that we get high throughput for our circuit.

![high clk freq](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/7898090c-c741-4f82-8a72-d5bf0d5f07f6)

**Identifiers and its types**

![Identifiers](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b979b608-ae26-4b1d-8686-cff160fdfe80)

**Implementation of Fibonacci using Pipeline**

![fib pipe](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/f160687c-97e2-4296-a499-2194fb60732b)

**Lab:Pipeline**

![lab pipeline](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6d512315-979b-4aee-a153-a4197ba55565)

In the above implementation, we can observe errors in the pipeline:

![error dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/2a830e98-d927-48c3-8ed0-8a707011e6df)

**Lab:Counter and Calculator**

**Block Diagram:**

![counter   calculator](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/164950b0-7c75-42d5-9e83-0eab925eebf8)

**Output**

![counter   calc output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/495bb758-f318-4ac5-8630-ca10ce3d1e95)

**Lab:Cycle Calculator**

**Block Diagram:**

![cycle calc](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/ab1a8ed5-5860-4324-ad16-3f3a448106ad)

**Output**

![cycle cal output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/1ad68ba3-c5ff-4625-8d6c-4561396a2cb6)
</details>

<details>
 <summary>
  Validity
 </summary>

 Validity provides:
 + Easier debug
 + Cleaner design
 + Better error checking
 + Automated clock gating

**Pythagoras theorem with validity**

![pytha validity](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/30b42516-d923-43ef-a9ee-1533b83b6fad)

**Clock Gating**

Clock gating saves power by pruning the clock tree, at the cost of adding more logic to a circuit.

![clock gating](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b14cd3c7-a576-481e-a83a-c89a97b9e014)

**Lab for Distance Accumulator**

**Block Diagram**

![total distance dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/477e3501-f851-4003-8f2b-08127e571075)

**Output**

![dis acc output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/728fcc32-01fd-4e74-8aef-42ec8e99daa5)

**Lab for Cycle Calculator with validity**

**Block Diagram**

![cycle calc val dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/0fc26fba-b7a3-4403-b906-1f1195c9babd)

**Output**

![cycle calc output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/44ac666d-2b5c-4460-be9d-c72302df1085)

**Lab for Calculator with Single Value Memory**

**Block Diagram**

![svm dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/caadc36c-9b9b-4864-9e77-7efc491135fd)

**Output**

![svm output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/4c8d3ffd-4897-4282-a02e-d35690a01eaf)

</details>

<details>
 <summary>
  Wrap Up
 </summary>

 We are going to learn about **Hierarchy** using Conway's Game of Life.

 **Lab: Conway's Game of Life**

 ![conway game of life](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d18bb902-6966-4f3f-b286-596d7beb16aa)

 **Lab: Hierarchy**

 ![hierarchy](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b22cce0e-f610-4076-9f16-9f2c23bba39b)

</details>

# Day 4

# Basic RISC-V CPU micro-architecture

<details>
 <summary>
  Introduction to simple RISC-V CPU micro architecture
 </summary>

**RISC-V Block Diagram**
 
![risc-v block diagram](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/086f5383-bcae-42a7-adde-a06705666230)

+ **Program Counter** --->A program counter is a register in a computer processor that contains the address (location) of the instruction being executed at the current time. As each instruction gets fetched, the program counter increases its stored value by 1.

+ **Instruction Memory** --->The instruction is read from part of memory; load and store instructions then read or write data from another part of memory.

+ **Register File** --->A register file is an array of processor registers in a central processing unit (CPU).

</details>

<details>
 <summary>
  Fetch and Decode
 </summary>

 **Fetch** ---> involves receiving the instruction from memory. 
 
 **Decode** --->  The fetched instruction is now decoded for conveying to execution.

 **Lab for Program Counter**
 
 **Block Diagram**

 ![lab pc](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/cb322fe8-0f54-450f-a06b-adb909ec25b3)

 **Makerchip Implementation**

 ![lab pc output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/86078564-3feb-40d6-bbab-61375a6cb18d)

**Lab for Fetch**

 **Block Diagram**

![lab fetch1](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/fe08bcee-454d-4c8f-a1f3-7b9503c6f9db)

 ![lab fetch 2](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e16fcbb5-6df7-4a12-9700-268d21beee2e)

**Makerchip Implementation**
 
![lab fetch output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d0975b79-531d-4efa-88f7-b8e8f411ca30)

**Lab for Decode Logic**

 **Block Diagram**

![lab decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/471f9d82-f0c4-4219-8b3b-0eb9909061dd)

**Makerchip Implementation**

![lab decode output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/fe0609b5-994c-405e-b48f-aa7f12276c43)

**Lab for Immediate Decode Logic**

![inst immediate decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/53d603c1-9567-4a00-bdfb-5a0440165592)

**Makerchip Implementation**

![imme decode output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/5864d71d-13db-4a18-af87-f0b2c3272cce)

**Lab for Instruction Decode(part1)**

![inst decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b0768f80-cc8f-46a7-a1db-bdc09b9638e9)

**Makerchip Implementation**

![inst decode 1 output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6519e0b3-32a7-4309-aa8c-4854e8708a0e)

**Lab to decode other field of instructions**

 ![inst field decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/49a5cef8-4502-496d-a35d-eebe0a69ebf7)

**Makerchip Implementation**

![inst field decode output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/97014a10-5b12-4f97-8cf3-ed892745b150)

**Lab for Instruction Decode(part2)**

![inst decode 2](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/bfcb8842-0312-4060-a98f-7372b8091fde)

**Makerchip Implementation**

![inst decode 2 output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/daf644ad-9a34-4cb6-b658-27c4dd1cfea1)


</details>
<details>
 <summary>
  RISC-V Control Logic
 </summary>
 
**Lab for Register File Read**
 
![reg file read](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/06f98796-9193-4a85-9fd0-656ce75bdc33)

![reg file read code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/66ff2ca9-6aaa-4337-b3a5-61cd8294d6f6)

![reg file read 2](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/0b623d7a-2e79-4c7c-bc25-a2baf3952dc6)

**Makerchip Implementation**

![reg file read output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/2b74d618-e263-4478-bc20-5779b9062174)

**Lab ALU**

**Block Diagram**

![lab ALU](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/a9e0852d-8a25-4b02-aada-fdab8c5d1ec0)

**Makerchip Implementation**

![lab aluoutput](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d5cf78f7-403d-4ab2-a6f5-385270f28018)

**Lab for Register File Write**

![reg file write dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/75bef632-14bf-476e-9f52-8ec266923b82)

![reg file write code](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/23e14ae9-d82b-44a7-8877-b26a71673b7a)

**Makerchip Implementation**

![reg file write output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6ff9de4c-91e4-48f7-ab30-460e32e66183)

**Concept of Arrays and Register File Details**

![arrays](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b5900227-9afe-440a-bc97-5e3ec0d6f6e1)

![reg file details ](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/e79644ee-226f-4622-bf83-2fa090ffd8eb)

**Lab for Branches**

![lab branches](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/974ce0dd-8b2d-4048-955b-b97e31cab78c)

**Makerchip Implementation**

![branches output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/9dd67fa6-c9c3-45b0-8d4e-b70b22dd2892)

**Lab for Testbench**

![lab testbench](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/11a6ae93-0e2b-4b48-9a6a-459f88b2dd55)

</details>

# Day 5

# Complete pipelined RISC-V CPU micro-architecture

<details>
 <summary>
   Pipelining the CPU
 </summary>

 **Introduction to control flow hazard and read after write hazard**

**Block Diagram for Pipelining your RISC-V**

![pipeline risc-v](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/58d73dd1-d901-4c9d-ac15-4e5d2fa4b93e)

**Block Diagram for Waterfall Logic Diagram**

![waterfall logic dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/26d15b1c-89f3-490f-b5ee-fe737b93496f)
 
**Block Diagram for Waterfall Diagram and Hazards**

 ![waterfall hazards](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/fa3a85c2-f42c-4ce9-85c1-75c7db68145b)

**Makerchip Implementation of 3-cycle valid signal**

![cycle valid inst](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/9a40c822-dbc5-403f-aafc-d9f0092c4fe9)

</details>

<details>
 <summary>
  Solution to pipeline hazards
 </summary>

**Register File Bypass**

 The ALU is able to bypass from any of these stages to dependent instructions in the Register Read stage.

 ![reg file bypass](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/d4e566f1-3ae3-411c-b227-af1394463e63)

**Makerchip Implementation**

 ![REg File bypass output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6400a433-b797-4a56-8eab-da7400d11790)

 **Branches**

![cpu branch dia](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/c0a35833-884b-4238-af6f-a5789ab44d66)

**Makerchip Implementation**

![cpu branch output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/5a5c20e9-a9d3-4960-99bd-85991553d30a)

**Complete Instruction Decode**

![complete inst decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/dbcdf17b-b151-4e0e-ab71-a0f01dabd119)

**Makerchip Implementation**

![complete inst decode output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/654f73c8-3210-49ef-8cce-0e7195d3f5d0)

**Complete ALU**

![complete alu](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/f3f6f7bd-a9b6-461e-8a4b-65b2978f8cef)

**Makerchip Implementation**

![complete alu output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b3df3b04-6269-44b7-b207-edc59b9c8313)

</details>

<details>
 <summary>
  Load/Store Instructions
 </summary>

 **LOADS/STORES**

 ![load](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/a4323e14-1943-4cce-9801-8915aa430466)

![load 2](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6fbec0d3-cd79-4b2e-95d5-31c1b6feb46f)


**Makerchip Implementation**

![load output](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/9e89d1c9-5b61-4a29-bdcc-3ee966713c77)

**JUMPS**

![jump](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/fe2ddd90-51a0-4b11-a64c-e28fdbadcd41)

**Complete code for RISC-V CPU**

```
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/blob/main/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   //
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store the final result value to byte address 16
   m4_asm(LW, r15, r0, 10000)           // Load the final result value from adress 16 to x17
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)


   |cpu
      @0
         $reset = *reset;
         
         //MODIFIED NEXT PC LOGIC FOR INCLUDING BRANCH INSTRCUTIONS
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>3$valid_taken_branch ? >>3$br_target_pc :
                     >>3$valid_load ? >>3$inc_pc :
                     >>3$valid_jump && >>3$is_jal ? >>3$br_target_pc :
                     >>3$valid_jump && >>3$is_jalr ? >>3$jalr_target_pc :
                     >>1$inc_pc ;
         //START LOGIC TO PROVIDE FIRST VALID LOGIC
         //$start = (>>1$reset && $reset == 0) ? 1'b1 : 1'b0;
         //$valid = $reset ? 1'b0 :
                  //$start ? 1'b1 : >>3$valid;
     
      @1  
         //INSTRUCTION FETCH
         $inc_pc[31:0] = $pc + 32'd4;
         
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         $instr[31:0] = $imem_rd_data[31:0];
         
         //INSTRUCTION TYPES DECODE        
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                                            32'b0;
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
           
         $rs1_valid = $is_r_instr  || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr  || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
           
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
           
         $rd_valid = $is_r_instr  || $is_u_instr || $is_j_instr || $is_i_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
      @2
         //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_load = $opcode == 7'b0000011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0100011;
         $is_xori = $dec_bits ==? 11'bx_100_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0100011;
         $is_andi = $dec_bits ==? 11'bx_111_0100011;
         $is_slli = $dec_bits ==? 11'b0_001_0100011;
         $is_srli = $dec_bits ==? 11'b0_101_0100011;
         $is_srai = $dec_bits ==? 11'b1_101_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         
         $jalr_target_pc[31:0] = $src1_value +$imm ;
      @3
         $is_jump = $is_jal || $is_jalr ;   
         `BOGUS_USE($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add
                    $is_lui $is_auipc $is_jal $is_jalr $is_load $is_sb $is_sh $is_sw $is_slti
                    $is_sltiu $is_xori $is_ori $is_andi $is_slli $is_srli $is_srai $is_sub $is_sll
                    $is_slt $is_sltu $is_xor $is_srl $is_sra $is_or $is_and)
         
      @2  
         //REGISTER FILE READ
         //$rf_wr_en = 1'b0;
         //$rf_wr_index[4:0] = 5'b0;
         //$rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = >>1$rf_wr_en && (>>1$rf_wr_index == $rf_rd_index1) ? >>1$result : $rf_rd_data1;
         $src2_value[31:0] = >>1$rf_wr_en && (>>1$rf_wr_index == $rf_rd_index2) ? >>1$result : $rf_rd_data2;
         $br_target_pc[31:0] = $pc +$imm;
         
      @3  
         //ARITHMETIC AND LOGIC UNIT (ALU)
         
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         $is_andi ? $src1_value & $imm :
                         $is_ori ? $src1_value | $imm :
                         $is_xori ? $src1_value ^ $imm :
                         $is_slli ? $src1_value << $imm[5:0] :
                         ($is_addi || $is_load || $is_s_instr) ? $src1_value + $imm :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_and ? $src1_value & $src2_value :
                         $is_or ? $src1_value | $src2_value :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_sub ? $src1_value - $src2_value :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_srl ? $src1_value >> $src2_value[4:0] :
                         $is_sltu ? $sltu_rslt :
                         $is_sltiu ? $sltiu_rslt :
                         $is_lui ? {$imm[31:12],12'b0} :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 :
                         $is_srai ? { {32{$src1_value[31]}},$src1_value} >> $imm[4:0] :
                         $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0,$src1_value[31]} :
                         $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0,$src1_value[31]} :
                         $is_sra ? { {32{$src1_value[31]}},$src1_value} >> $src2_value[4:0] :
                         32'bx;
         
         
         //REGISTER FILE WRITE
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         
         
         //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                         1'b0;
          //CYCLE VALID INSTRUCTIONS
         $valid = !(>>1$valid_taken_branch || >>2$valid_taken_branch ||
                    >>1$valid_load || >>2$valid_load) ;
         
         $valid_load = $valid && $is_load ;
         //$valid = !(>>1$valid_taken_branch || >>2$valid_taken_branch);
         $valid_taken_branch = $valid && $taken_branch;
         $valid_jump = $is_jump && $valid ;
         `BOGUS_USE($taken_branch)
      @4
         //MINI 1-R/W MEMORY
         $dmem_wr_en = $is_s_instr && $valid ;
         $dmem_addr[3:0] = $result[5:2] ;
         $dmem_wr_data[31:0] = $src2_value ;
         $dmem_rd_en = $is_load ;
         
      @5
         //LOAD DATA
         $ld_data[31:0] = $dmem_rd_data ;   
         
         
         

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //*passed = *cyc_cnt > 40;
   *passed = |cpu/xreg[15]>>5$value == (1+2+3+4+5+6+7+8+9) ;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
   //@4 would work for all lab
\SV
   endmodule

```

**Diagram**

![complete risc-v cpu diagram](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/266c62d8-497e-4077-9b9a-36f77c31b26e)

</details>

# References

+ https://makerchip.com/sandbox/
+ https://github.com/stevehoover/RISC-V_MYTH_Workshop
+ https://github.com/RISCV-MYTH-WORKSHOP/RISC-V-CPU-Core-using-TL-Verilog


