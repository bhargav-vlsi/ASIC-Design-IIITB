# iiitb_asic_class

<details>
<summary>DAY-1</summary>
<br>

## DAY-1

### Icarus Verilog Installation

**Steps to install Icarus Verilog**
```
sudo apt-get install iverilog
```


![iverilog](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/1fce1434-4d84-4f1e-b2d3-c2b06fe998d4)


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

![yosys](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/f7030fd6-fe4a-4c98-b873-1826deb7fcfa)


Yosys installed



### Gtkwave Installation

**Steps to install Gtkwave**
```
sudo apt update
sudo apt install gtkwave
```

![gtkwave](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/ee1bf47a-cf28-494f-b449-2dfa35d27cb9)

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
![ngspice](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/84cc0419-139d-4cc7-8e24-3df164950c9a)

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
![opensta](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/ce6d6daa-5bc7-469d-bb34-c497b56167fc)

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

![magic](https://github.com/bhargav-vlsi/ASIC-Design-IIITB/assets/141163376/d30d5299-d203-4ea3-8d12-0089c3ca8850)

Magic tool installed
</details>
