# LAMMPS WITTIN DOCKER

## Start from python-slim
`docker container run -ti --name pylammp python:3.7.3-slim bash`<br/>
## In this new container, install LAMMPS, dependencies, and tools
```
apt-get update
# libssl-dev is the openssl dev library (need it for cmake)
apt-get install -y wget build-essential ssh zlib1g-dev libfftw3-dev \
libopenblas-dev libopenmpi-dev libssl-dev
rm -rf /var/lib/apt/lists/*
```
### INSTALL CMAKE
```
wget https://github.com/Kitware/CMake/releases/download/v3.19.6/cmake-3.19.6.tar.gz
tar -zxvf cmake-3.19.6.tar.gz
cd cmake-3.19.6
./bootstrap
make
make install
```
### INSTALL LAMMPS
```
VERSION=lammps-10Feb21
wget -q https://lammps.sandia.gov/tars/$VERSION.tar.gz
tar -xf $VERSION.tar.gz
cd $VERSION
mkdir build
cd build
# configure LAMMPS compilation
cmake -C ../cmake/presets/minimal.cmake -D BUILD_SHARED_LIBS=on \
      -D LAMMPS_EXCEPTIONS=on -D PKG_PYTHON=on ../cmake
# compile LAMMPS
cmake --build .
# install LAMMPS into $HOME/.local
cmake --install .
```
### INSTALL vim
`apt-get install vim`
### ENSURE LD_LIBRARY_PATH IS INCLUDED IN THE PATH
```
vi ~/.bashrc
export LD_LIBRARY_PATH=$HOME/.local/lib:$LD_LIBRARY_PATH
export PATH=$HOME/.local/bin:$PATH
######
source ~/.bashrc
```
### CLEAN
```
cd ~/../data/
rm -rf $VERSION.tar.gz $VERSION cmake-3.19.6.tar.gz cmake-3.19.6
```
### Install numpy, matplotlib, jupyter and others
`pip3 install numpy scipy matplotlib jupyter pandas sympy nose`

### Problem with LD_LIBRARY_PATH env variable
jupyter reads LD_LIBRARY from `$HOME/.local/lib/pythonX.Y/site-packages/lammps`
 but with the cmake installation, that library is located in `
$HOME/.local/lib/ `. To solve this, I copied `liblammps.so` and `liblammps.so.0`
into `$HOME/.local/lib/pythonX.Y/site-packages/lammps`

## Runnig LAMMPS within jupyter
We can use this container with the Dockerfile:
```
FROM pylammp
LABEL maintainer="JBB"
LABEL version="0.1"
LABEL description="LAMMPS for Pyhton" 
EXPOSE 8888
CMD ["jupyter","notebook","--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```
### Buid image
`docker build -t pylammpsjup .`

### run and go to jupyter. Also share a volume with the host
`docker run -ti -v path/2/sharedir:/data -p 8888:8888 pylammpsjup`

## My image can be accessed from:
`docker push jabohorq/pylammpjup2:tagname`
