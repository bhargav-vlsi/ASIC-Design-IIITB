# ASIC Design

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
Ideally, we expect gate level netlist to function similar as RTL design. But at times, it may not function as intended due to various reasons like missing signals in sensitivity list, improper use of blocking and non-blocking statements and improper RTl design.

### Blocking & Non-blocking statements
There are two important ways to write group of statements inside always block. 
Blocking statement refers to sequential execution of statements one at a time. ( = )
Non-blocking statement refers to parallel execution of statements( <= )
We prefer to use non-blocking statements in always block for sequential circuits. If we are using blocking statements, we should carefully place the statements proper order for logic to be executed properly.

### Sample GLS execution.
Consider the following mux code and its simulation in behaviourial modelling.
![ternary_mux_pre_code](./Images/ternary_mux_pre_code.png)

![ternary_mux_pre_sim](./Images/ternary_mux_pre_sim.png)

![ternary_mux_pre](./Images/ternary_mux_pre.png)


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

![blocking_mux](./Images/blocking_mux.png)

Now, after synthesis, we observe synthesis simulation mismatch. 
![blocking_post_sim](./Images/blocking_post_sim.png)


</details>

<details>
<summary>REFERENCES</summary>

    
https://steveicarus.github.io/iverilog/

https://yosyshq.net/yosys/

https://gtkwave.sourceforge.net/

https://ngspice.sourceforge.io/

https://github.com/The-OpenROAD-Project/OpenSTA

http://opencircuitdesign.com/magic/

https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

</details>
