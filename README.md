# RISC-V
# Table of Contents
 - [Day0 - Installation](#Installation)<br>
 - [Day0 - Introduction to RISC-V ISA and GNU Compiler ToolChain](#Introduction to RISC-V ISA and GNU Compiler ToolChain)<br>

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
  
 </summary>
</details>


   

