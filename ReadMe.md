# ASIC Design

## RTL design using Verilog
<details>
<summary>DAY-0</summary>
<br>



### Icarus Verilog Installation

**Steps to install Icarus Verilog**
```
sudo apt-get install iverilog
```


![iverilog](./Images/Iverilog.png)


iverilog tool installed

### Yosys Installation

**Steps to install Yosys**

```
git clone https://github.com/YosysHQ/yosys.git
cd yosys 
sudo apt install make (If make is not installed please install it) 
sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make 
sudo make install
```

![yosys](./Images/Yosys.png)

Yosys installed



### Gtkwave Installation

**Steps to install Gtkwave**
```
sudo apt update
sudo apt install gtkwave
```

![gtkwave](./Images/Gtkwave.png)

gtkwave installed

### Ngspice installation
**Steps to install ngspice**
```
wget https://sourceforge.net/projects/ngspice/files/ngspice-40.tar.gz
tar -zxvf ngspice-40.tar.gz
cd ngspice-40
mkdir release
cd release
sudo apt install automake libtool libxaw7-dev flex bison libncurses5-dev
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install
```
![ngspice](./Images/Ngspice.png)

ngspice installed

### OpenSTA Installtion
**Steps to install OpenSTA**
```
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
sudo apt-get install cmake clang gcctcl swig bison flex
cmake ..
make
```
![opensta](./Images/OpenSTA.png)

Note: Additional step of storing path of openSTA executable file in environment variables is done for easy access in terminal

Open STA installed

### Magic tool installation
**steps to install magic layout tool**
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic-master
./configure
make
make install
sudo apt install magic
```

![magic](./Images/Magic.png)

Magic tool installed
</details>

<details>
<summary>DAY-1</summary>
<br>

### Overview
This session is about steps followed to compile and simulate verilog design and testbench codes using iverilog tool. This section also deals with graphical waveform viewer tool called gtkwave and synthesis tool called yosys and its steps to produce netlist from design file.

### Sample Verilog simulation
This session takes an example of 2x1 multiplexer (verilog design and test bench) to demonstrate iverilog compilation and gtkwave waveform viewer. 

The verilog codes are taken from github repository: https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git



Following is syntax for compilation and execution of verilog codes to generation outputs.
```
iverilog designfile.v testbench.v
./aout
gtkwave vcdfile.vcd
```


Below represents sample design verilog codes.

![verilogcode](./Images/Verilogcode.png)



Below represent simulation output of 2x1 multiplexer design.

![simulation](./Images/Simulation.png)



### Yosys synthesis process
This section explains the concept of yosys library cells and process of generating netlist using yosys tool. The library contains variety of cells with various operating speeds for different applications and avoid violations. 

Following represents various commands used to generate netlist for given design.

```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> abc -liberty <path to lib file>
yosys> show
yosys> write_verilog <file_name_netlist.v>
yosys> write_verilog -noattr <file_name_netlist.v>
```


Below represents schematic represented by yosys tool for given design.
![Schematic](./Images/Schematic.png)



Below represents netlist represented by yosys tool for given design.
![Netlist](./Images/Netlist.png)






</details>



<details>
<summary>DAY-2</summary>

### Overview
This section describes basic understanding of lib technology file and its important aspects. This section also explains hierarchy and flat synthesis implementation of multiple modules.

### Verilog modules
The verilog codes are taken from github repository: https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

The verilog codes considered are multiple_moudules.v

### Sample Synthesis of multiple modules
This section explains the sample synthesis process involved in multiple modules rather than single module. The previously discussed yosys commands are used to execute synthesis process with respective parameters for two types of designs. They are hierarchy and flat designs. 

```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> abc -liberty <path to lib file>
yosys> flatten
yosys> show
yosys> write_verilog -noattr <file_name_netlist.v>
```

Following represents schematic netlist of hierarchy design.
![Multiple_modules](./Images/Multiple_modules.png)
 
Following represents schematic netlist of flat design.
![Multiple_modules_flat](./Images/Multiple_modules_flat.png)


### Sample synthesis of submodules
In this section, we learn about synthesis process of submodules and generating corrspnding netlist.

We follow the similar steps as decribed previously with small change in synthesis command i.e we specify the subodule name we are interested and apply steps similar to we have seen previosuly till netlist generation.
```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
synth -top <submodule_name>
yosys> abc -liberty <path to lib file>
yosys> show
yosys> write_verilog -noattr <file_name_netlist.v>
```

Following represents synthesis schematic of two submodules defined in main design file.
![Submodule1](./Images/Submodule1.png)

![Submodule2](./Images/Submodule2.png)

We prefer to synthesize submodules separately due to various reasons such as inefficient synthesis carried out if done with entire module, if design contains replica of sub modules, we would like to synthesize once and combine together in main module.

### Coding styles
This section explains about various coding styles. 

Usually, in a digital circuit, we encounter an issue called as glitch. This is due to propagation delay associated with gates in circuit. To resolve this issue, we use D flip flop in between combinational circuits to hold the stable value and prevent it from disturbing next stage until positive edge of clock occurs. Hence, we want a particular value to occur initially. This is achieved through synchronous or asynchronous reset signals.

Following represents asynchronous reset signal in action.
![Dff_async_res](./Images/Dff_async_res.png)

Following represents asynchronous set signal in action.
![Dff_async_set](./Images/Dff_async_set.png)
 
Following represents synchronous reset signal in action.
![Dff_sync_res](./Images/Dff_sync_res.png)

### Synthesis of Flop circuits
This section explains steps to be followed in synthesis of circuits containing flop modules and its commands.

These include an additional step specifically for FF designs to pick library cells specific to them. 
```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> dfflibmap -liberty <path to lib file>
yosys> abc -liberty <path to lib file>
yosys> show
yosys> write_verilog <file_name_netlist.v>
yosys> write_verilog -noattr <file_name_netlist.v>
```

Following represents asynchronous reset schematic representation.
![Dff_async_res](./Images/Dff_async_res_schematic.png)

Following represents asynchronous set schematic representation.
![Dff_async_set](./Images/Dff_async_set_schematic.png)
 
Following represents synchronous reset schematic representation.
![Dff_sync_res](./Images/Dff_sync_res_schematic.png)

### Interesting optimization exhibited by yosys
This section describes about glimpse of optimization executed by yosys.

Lets take example of hardware to that multiplies 2 to input and produces output. We think that we require some kind of gates to achieve that execution, but at end of day its all playing with wires. The tool does not map hardware to any standard cell in library due to optimization.

Following represents example of circuit to just multiply input to 2 and produce output.
![mul2](./Images/mul2.png)



</details>

<details>
<summary>DAY-3</summary>

### Introduction to optimization
Optimization plays an important while we design any hardware. It reduces number of components which inturn reduces size and improves performance. Many times, we come across an expression which by simplification reduces to a either simple variable or a constant value through reduction of unsued variables.

For example, if a DFF having D=0 and reset=1 will always have Q=0 for all clk values.
On the other side, if a DFF having D=0 and set signal will either Q=1 or Q=0 depending on set signal. If set=0 then Q =0 always. If set=1 Q=1 always.

### Combinational logic optimization

We have considered few examples to demonstrate optimization exhibited by yosys tool with additional step. The command removes unused cells and nets if any.
```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> opt_clean -purge
yosys> abc -liberty <path to lib file>
yosys> show
yosys> write_verilog <file_name_netlist.v>
yosys> write_verilog -noattr <file_name_netlist.v>
```
Following represents simplified circuit for y=a?b:0
![opt_check](./Images/opt_check.png)

Following represents simplified circuit for y=a?1:b
![opt_check2](./Images/opt_check2.png)

Following represents simplified circuit for y=a?(c?b:0):0
![opt_check3](./Images/opt_check3.png)

Below is a small lab exercise to understand optimization done by yosys.

Following represents simplified circuit for y = a?(b?(a & c ):c):(!c)
![opt_check4](./Images/opt_check4.png)

Following represents sample multiple module verilog code and its simplified schematic diagram after optimization.
![multiple_module_opt_code](./Images/multiple_module_opt_code.png)

![multiple_module_opt](./Images/multiple_module_opt.png)

Following represents another sample multiple module  verilog code and its simplified schematic diagram after optimization.
![multiple_module_opt_code2](./Images/multiple_module_opt_code2.png)

![multiple_module_opt2](./Images/multiple_module_opt2.png)

### Sequential logic optimization

We have used few examples of sequential circuits to demonstrate optimization done by yosys.

```
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> dfflibmap -liberty <path to lib file>
yosys> opt_clean -purge
yosys> abc -liberty <path to lib file>
yosys> show
yosys> write_verilog -noattr <file_name_netlist.v>
```

Following represents dff_const1 verilog code and its simplified schematic diagram after optimization.
![dff_const1_code](./Images/dff_const1_code.png)

![dff_const1](./Images/dff_const1.png)

Following represents dff_const2 verilog code and its simplified schematic diagram after optimization.
![dff_const2_code](./Images/dff_const2_code.png)

![dff_const2](./Images/dff_const2.png)

Following represents dff_const3 verilog code and its simplified schematic diagram after optimization.
![dff_const3_code](./Images/dff_const3_code.png)

![dff_const3](./Images/dff_const3.png)

Following represents dff_const4 verilog code and its simplified schematic diagram after optimization.
![dff_const4_code](./Images/dff_const4_code.png)

![dff_const4](./Images/dff_const4.png)

Following represents dff_const5 verilog code and its simplified schematic diagram after optimization.
![dff_const5_code](./Images/dff_const5_code.png)

![dff_const5](./Images/dff_const5.png)

### Unsued output optimization in sequential circuits

The yosys tool removes unused logic not connected to outputs. Also, it removes outputs and its associated logic not required in output which is illustrated in below examples.

Following represents code and schematic of sample counter_opt design. Though the code contains 3bit register, but still yosys will retain logic only visible in required final output.
![counter_opt_code](./Images/counter_opt_code.png)

![counter_opt](./Images/counter_opt.png)

Following retention in logic required in output compared to previous one.
![counter_opt2_code](./Images/counter_opt2_code.png)

![counter_opt2](./Images/counter_opt2.png)


</details>

<details>
<summary>DAY-4</summary>

### Overview


### Gate level Simulation (GLS) & Synthesis
Gate level netlist is a verilog code generated by yosys to map it to standard cell library later. This code is functionally similar to RTL design code written by us. Simulation of netlist is done to ensure retention of functionality after synthesis. The netlist is build using gate models which is either timing aware or functional aware or both. Timing awareness refers to delay information associated with gates.


### Importance of GLS
Ideally, we expect gate level netlist to function similar as RTL design. But at times, it may not function as intended due to various reasons like missing signals in sensitivity list, improper use of blocking and non-blocking statements and improper RTL design.

### Blocking & Non-blocking statements
There are two important ways to write group of statements inside always block. 
Blocking statement refers to sequential execution of statements one at a time. ( = )
Non-blocking statement refers to parallel execution of statements( <= )
We prefer to use non-blocking statements in always block for sequential circuits. If we are using blocking statements, we should carefully place the statements proper order for logic to be executed properly.

### Sample GLS execution.
Consider the following mux code and its simulation in behaviourial modelling.
![ternary_mux_pre_code](./Images/ternary_mux_pre_code.png)

![ternary_mux_pre_sim](./Images/ternary_mux_pre_sim.png)

![ternary_mux](./Images/ternary_mux.png)


Now, after the synthesis of netlist of RTL design,  we verify the post synthesis simulation by using following command to use verilog models for GLS.
```
iverilog ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v netlist.v tb.v
```
![ternary_mux_post_sim](./Images/ternary_mux_post_sim.png)

### Sample synthesis simulation mismatch due to missing sensitivity list
Here we consider a badly designed multiplexer which is sensitive to only select line changes and not input lines.
Following is RTL design, simulation and schematic representation for bad mux. We clearly observe in simulation that design is not working like a multiplexer.
![bad_mux_pre_code](./Images/bad_mux_pre_code.png)

![bad_mux_pre_sim](./Images/bad_mux_pre_sim.png)

![bad_mux](./Images/bad_mux.png)

Now, after synthesis, we observe synthesis simulation mismatch. 
![bad_mux_post_sim](./Images/bad_mux_post_sim.png)

### Sample synthesis simulation mismatch due to blocking statement
Here we consider a sample blocking statement example to understand importance of right use of blocking statements.
Following is sample RTL design, simulation and synthesis of blocking caveat code. We clearly observe mismatch in logic we intended to acheive and we currently observe.

![blocking_pre_code](./Images/blocking_pre_code.png)

![blocking_pre_sim](./Images/blocking_pre_sim.png)

![blocking](./Images/blocking.png)

Now, after synthesis, we observe synthesis simulation mismatch. 
![blocking_post_sim](./Images/blocking_post_sim.png)


</details>

<details>
<summary>DAY-5</summary>

### Overview
This section explains about simulation and synthesis of if, case, generate and loops statements

### If statements and associated caveat
If statements in verilog are mapped to a inter-linked multiplexer. The first condition will be given highest priority among others and else (if present) the lowest. But, due to bad style of coding we miss else statement, yosys will infer a latch to hold previous output. This problem mainly observed in combinational circuits due to presense of inferred latches. We observe these issues in following examples.

Here is sample incomplete if statement code, simulation and schematic representation.
![incomplete_if_code](./Images/incomplete_if_code.png)

![incomplete_if_sim](./Images/incomplete_if_sim.png)

![incomplete_if](./Images/incomplete_if.png)

Here is sample incomplete if statement2 code, simulation and schematic representation.
![incomplete_if2_code](./Images/incomplete_if2_code.png)

![incomplete_if2_sim](./Images/incomplete_if2_sim.png)

![incomplete_if2](./Images/incomplete_if2.png)

### Case statements and associated caveat
Even case statements in verilog are mapped to multiplexer. The code associated with particular condition is executed. If no condition is met, it will execute default statement (if present) otherwise it will infer latch and produce previous output. If a condition is i.e. case 2'b1?, then we may multiple statements matching and the one with highest priority will be executed. This is also an issue and should be taken care. In some scenario, we may have all conditions met, but we may have not assigned all outputs (if present). We observe these issues in following examples.

Here is sample incomplete case statement code, simulation and schematic representation.
![incomplete_case_code](./Images/incomplete_case_code.png)

![incomplete_case_sim](./Images/incomplete_case_sim.png)

![incomplete_case](./Images/incomplete_case.png)

Here is sample complete case statement code, simulation and schematic representation.
![complete_case_code](./Images/complete_case_code.png)

![complete_case_sim](./Images/complete_case_sim.png)

![complete_case](./Images/complete_case.png)

Here is sample partial case statement code, simulation and schematic representation.
![partial_case_code](./Images/partial_case_code.png)

![partial_case_sim](./Images/partial_case_sim.png)

![partial_case](./Images/partial_case.png)


Here is sample bad case statement code, simulation and schematic representation.
![bad_case_code](./Images/bad_case_code.png)

![bad_case_sim](./Images/bad_case_sim.png)

![bad_case](./Images/bad_case.png)

Now performing GLS, we find that the output stuck at some value is not seen here. It is functioaning as proper multiplixer.
![bad_case_post_sim](./Images/bad_case_post_sim.png)

### Looping constructs
Looping constructs are mainly used to evaluate repeated task efficiently. We have two types: For & Generate-For loop.

For loop is written inside always block and used to evaluate statements executed repeatedly for a certain number of times. A good example is a 128:1 multiplexer.

Following represents a sample for loop RTL code, simulation, schematic and synthesized simulation for multiplexer.
![mux_for_code](./Images/mux_for_code.png)

![mux_for_sim](./Images/mux_for_sim.png)

![mux_for](./Images/mux_for.png)

So even after synthesis, functionality is retained.
![mux_for_post_sim](./Images/mux_for_post_sim.png)

Generate for loop is written outside always block and used to instantiate a certain hardware multiple times. A good example is a 64 bit Full adder.

Following represents a sample generate loop RTL code, simulation, schematic and synthesized simulation for ripple carry adder.
![rca_code](./Images/rca_code.png)

![fa_code](./Images/fa_code.png)

![rca_sim](./Images/rca_sim.png)

![rca](./Images/rca.png)

So even after synthesis, functionality is retained.
![rca_post_sim](./Images/rca_post_sim.png)

</details>

## RISCV based Myth

<details>
<summary>DAY-0</summary>

This section describes steps to install and configure RISCV tool chain

```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
sudo apt install libboost-regex-dev
cd riscv_workshop_collaterals
chmod 755 run.sh
./run.sh
```

The above commands usually creates a folder called "riscv_toolchain" in home folder. Follow the next commands to access the tool chain from anywhere in terminal. Otherwise, path to bin folder of toolchain has to be provided to execute respective commands.

```
gedit .bashrc
```

At last line of .bashrc

```
export PATH=/home/<username>/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH
```

Save and close the .bashrc file. Then give following command to apply the changes of .bashrc file.

```
source .bashrc
```

![riscv_toolchain](./Images2/riscv_toolchain.png)

Riscv toolchain installed

</details>

<details>
<summary>DAY-1</summary>

### Overview
This section explains about development of applications on custom hardware architecture and RISCV toolchain.

### Introduction
RISC- Reduced Instruction Set Architecture Computer
We have hardware resources and software codes running on these resources. Compiler is a tool that convert high level code to assembly level code. Assembler is a tool that converts assembly level code to machine level code. The assembly level code is specific to type of architecture used. We will also look into several instructions and concepts such as psuedo instructions, integer RV64I, multiply extenstion RV64M, single & double precision floating point extension, application binary interface, memory allocation and stack pointer.


### GCC compiler and sample usage
Gcc compiler is used to convert C code into machine code for computer to execute. Here is sample commands to compile and execute sample c code.
```
gcc -o code.out code.c
./code.out
```

Following is sample c code for sum of 'n' numbers.
```
#include<stdio.h>
int main()
{
	int n,sum=0;
	printf("Enter n: ");
	scanf("%d",&n);
	for(int i=1;i<=n;i++) {
	sum=sum+i; }
	printf("Sum of %d numbers is %d\n",n,sum);
	return 1;
}
```

![sum1ton](./Images2/sum1ton.png)

### RISCV gcc compilation and assembly code
Folowing command describes the way to compile c code in riscv gcc compiler.
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o <output>.o <inputfile>.c
```

Following represents way to observe object(compiled assembly code)
```
riscv64-unknown-elf-objdump -d <output>.o
```

If we want a bit optimized version of assembly code we use following option and use same command to observe object file.
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o <output>.o <inputfile>.c
```
### Execution of output file in RISCV tool chain
We use following command to execute object file using riscv tool chain.
```
spike pk <output>.o
```

![spike_pk_sum1ton](./Images2/spike_pk_sum1ton.png)

We use following command to debug the output
```
spike -d pk <output>.o
```
![spike_pk_debug](./Images2/spike_pk_debug.png)

We have few commands to execute and observe specific variables or registers during debugging session.
--To run code until a specific location
```
until pc 0 <memory_location>
```

--To observe contents of register in specific core
```
reg <core> <register>
```

Press "Enter" to execute line by line in assembly code.

Press "q" to to quit debugging session.

### Integer floating point representation
Human beings are accustomed to use decimal number system and computers are designed for binary number system. Hence, there is a requirement for conversion of decimal to binary system. Present day computers are designed to handle 64 bit numbers where we usually divide 64 bits into two 32 bits group, each 32 bit group is divided into four 8 bit group, each 8 bit group is divided into either 2 nibbles or simply considered doubleword.

The number of patterns for any 'n' bits is 2^(n).

Signed binary numbers are represented using 2's complement numbers. MSB of a binary number is 0 for positive number and 1 for negative number in any representation.

For unsigned numbers of n bit, range -> 0 to 2^(n)-1.
For signed numbers of n bit, range -> -(2^(n-1)) to (2^(n-1)-1).
Here is sample C code to understand floating representation and highest & lowest value possible in RISCV.

```
#include<stdio.h>
#include<math.h>

int main()
{
	unsigned long long int max=(unsigned long long int) (pow(2,64)-1);
	printf("Highest num represented by unsigned long long integer for 64 bit is %llu\n",max);
	
	max=(unsigned long long int) (pow(2,10)-1);
	printf("Highest num represented by unsigned long long integer for 10 bit is %llu\n",max);
	
	max=(unsigned long long int) (pow(2,127)-1);
	printf("Highest num represented by unsigned long long integer for 127 bit is %llu\n",max);
	
	unsigned long long int min=(unsigned long long int) (pow(2,64)*-1);
	printf("Lowest num represented by unsigned long long integer for 64 bit is %llu\n",min);
	
	long long int max2=(long long int) (pow(2,63)-1);// bug was here type long long int instead of just int in video
	printf("Highest num represented by signed long long integer for 64 bit is %lld\n",max2);
	
	long long int min2=(long long int) (pow(2,63)*-1);// bug was here type long long int instead of just int in video
	printf("Lowest num represented by signed long long integer for 64 bit is %lld\n",min2);
	return 1;
}
```

Following output represents the output for above code.

![unsigned_signed_output](./Images2/unsigned_signed_output.png)

</details>

<details>
<summary>DAY-2</summary>

### Application Binary Interface

Interface simply refers to appearance & functionality of a system without deeper understanding architecture & implementation.
Ex: Users need to know mainly about appearance of a building rather than construction of the same.

Ex: Programmer need to know strcuture or syntax of a application library rather than its internal implementation.

Application Binary interface uses registers to access hardware resources. 

RISC uses little endian architecture for storing data where most significant byte is in highest memory location.

RISC uses little endian architecture and stores 1 byte in each location. Instructions are 32 bit but data is 64 bit in 64 bit architeciture. It is a byte addressable memory. We specify -march=rv64i as architecture and hence set of integer base instructions.

We have 32 registers in RISCV architecture with x00, x01 and so on as representation.

![register_riscv](./Images2/register_riscv.png)

We have a sample for ABI system call. Following are two codes (C & ASM)

C code

```
#include <stdio.h>

extern int load(int x, int y); 

int main() {
	int result = 0;
       	int count = 3;
    	result = load(0x0, count+1);
    	printf("Sum of number from 1 to %d is %d\n", count, result); 
}
```

load.S

```
.section .text
.global load
.type load, @function

load:
	add 	a4, a0, zero //Initialize sum register a4 with 0x0
	add 	a2, a0, a1   // store count of 10 in register a2. Register a1 is loaded with 0xa (decimal 10) from main program
	add	a3, a0, zero // initialize intermediate sum register a3 by 0
loop:	add 	a4, a3, a4   // Incremental addition
	addi 	a3, a3, 1    // Increment intermediate register by 1	
	blt 	a3, a2, loop // If a3 is less than a2, branch to label named <loop>
	add	a0, a4, zero // Store final result to register a0 so that it can be read by main program
	ret
```

Following is output after compilation & execution of above codes.

![c_asm_output](./Images2/c_asm_output.png)


### Execution of C code on RISCV CPU Verilog

We have a RISCV design written in verilog. We convert our c code into hex code and simulate and execute it on RISCV CPU code and get back output on terminal. We have all necessary codes in labs folder.

![c_riscv_execution](./Images2/c_riscv_execution.png)

</details>

<details>
<summary>DAY-3</summary>

### Overview
This section describes about TL-verilog and Makerchip platform & its examples.

### Makerchip platform
Makerchip platform is a cloud based web application that takes design input in form of TL-verilog code and provides logical diagram & waveform as output without any testbench provided externally.

### Pythagorean example
We take pythagorean example from platform and execute it to understand its flow. We compile and observe the output on different windows as shown below.

![maker_chip_pythagorean](./Images2/maker_chip_pythagorean.png)

### Few other sample exercise in Maker chip platform (combinational circuits0
Here we consider simple logic gates as examples.

![maker_chip_gates](./Images2/maker_chip_gates.png)

Here we consider example of Full adder to understand use of vectors.

![maker_chip_adder](./Images2/maker_chip_adder.png)

Here we consider example of multiplexer.

![maker_chip_mux](./Images2/maker_chip_mux.png)

Here is example for calculator.

![maker_chip_calc1](./Images2/maker_chip_calc1.png)

### Sequential circuits in Maker chip platform

Here is example for free counter.

![maker_chip_counter](./Images2/maker_chip_counter.png)

Here is sequential calculator that remembers previous results for next calculation.

![maker_chip_calc2](./Images2/maker_chip_calc2.png)


</details>

<details>
<summary>REFERENCES</summary>

    
https://steveicarus.github.io/iverilog/

https://yosyshq.net/yosys/

https://gtkwave.sourceforge.net/

https://ngspice.sourceforge.io/

https://github.com/The-OpenROAD-Project/OpenSTA

http://opencircuitdesign.com/magic/

https://github.com/kunalg123/

https://github.com/stevehoover/RISC-V_MYTH_Workshop

</details>
