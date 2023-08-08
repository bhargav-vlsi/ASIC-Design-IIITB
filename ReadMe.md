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
<summary>REFERENCES</summary>

    
https://steveicarus.github.io/iverilog/

https://yosyshq.net/yosys/

https://gtkwave.sourceforge.net/

https://ngspice.sourceforge.io/

https://github.com/The-OpenROAD-Project/OpenSTA

http://opencircuitdesign.com/magic/

https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

</details>
