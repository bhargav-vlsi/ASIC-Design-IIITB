# ASIC Design
[Day-0](#day-0) Installation of required Tools.
<details>
<summary>DAY-0</summary>
<br>

## DAY-0

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
