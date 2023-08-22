# RISC-V
# Table of Contents
 - [Day0 - Installation](#Installation)<br>
 - [Day1 - Introduction to RISC-V ISA and GNU Compiler ToolChain](#Introduction-to-RISC-V-ISA-and-GNU-Compiler-ToolChain)<br>
  - [Day2 - Introduction to ABI and basic verification flow](#Introduction-to-ABI-and-basic-verification-flow)<br>
  - [Day3 - Digital logic with TL-Verilog and Makerchip](#Digital-logic-with-TL-Verilog-and-Makerchip)<br>
  - [Day4 - Basic RISC-V CPU micro-architecture](#Basic-RISC-V-CPU-micro-architecture)<br>

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

![Sum C program](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/6d1d01f9-97a5-499c-94c9-0e0a649ec8bb)

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

**Lab for Instruction Decode(part1)**

![inst decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/b0768f80-cc8f-46a7-a1db-bdc09b9638e9)

**Makerchip Implementation**



**Lab to decode other field of instructions**

 ![inst field decode](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/49a5cef8-4502-496d-a35d-eebe0a69ebf7)

**Makerchip Implementation**

**Lab for Instruction Decode(part2)**

![inst decode 2](https://github.com/IswaryaIlanchezhiyan/Iswarya-RISC-V/assets/140998760/bfcb8842-0312-4060-a98f-7372b8091fde)

**Makerchip Implementation**



</details>




