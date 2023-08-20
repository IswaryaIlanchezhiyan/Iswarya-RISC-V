# RISC-V
# Table of Contents
 - [Day0 - Installation](#Installation)<br>
 - [Day0 - Introduction to RISC-V ISA and GNU Compiler ToolChain](#Introduction-to-RISC-V-ISA-and-GNU-Compiler-ToolChain)<br>

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





