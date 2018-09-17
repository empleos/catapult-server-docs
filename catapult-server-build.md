# Setting up Catapult network on nodes running Ubuntu 16.04

Set up a node on any hosting provider  
Recommended minimum 4GB RAM and 80GB of disk space  
Login to your node as root user  

## Add user

We don't recommend to use 'catapult' username for security reasons. Choose different usernames on each of your nodes.  
```
useradd -s /bin/bash -m catapult  
# set password for the user
passwd catapult
gpasswd -a catapult sudo
su -l catapult
```

## Install dependencies
```
sudo apt update
sudo apt upgrade
sudo apt install -y build-essential git cmake automake autoconf libtool pkg-config software-properties-common
```


## Install gcc 7
```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt install -y gcc-7 g++-7
sudo apt clean 
sudo rm -rf /var/lib/apt/lists/* 
sudo rm /usr/bin/gcc /usr/bin/g++
sudo ln -s /usr/bin/gcc-7 /usr/bin/gcc
sudo ln -s /usr/bin/g++-7 /usr/bin/g++
```

## Create directories
```
C_HOME=$HOME/catapult
mkdir $C_HOME
C_LIBS=$C_HOME/libs
mkdir $C_LIBS
```


## Install rocksdb library
```
cd $C_LIBS
wget -O rocksdb-5.13.1.tar.gz https://github.com/facebook/rocksdb/archive/v5.13.1.tar.gz
tar zxvf rocksdb-5.13.1.tar.gz
cd rocksdb-5.13.1
make
sudo make install
```


## Install boost library
```
cd $C_LIBS
wget https://dl.bintray.com/boostorg/release/1.65.1/source/boost_1_65_1.tar.gz
tar -xzvf boost_1_65_1.tar.gz
cd boost_1_65_1
./bootstrap.sh
sudo ./b2 install -j2
```

## Install googletest library
```
cd $C_LIBS
wget -O googletest-release-1.8.0.tar.gz https://github.com/google/googletest/archive/release-1.8.0.tar.gz
tar zxvf googletest-release-1.8.0.tar.gz
cd googletest-release-1.8.0/googletest
mkdir _build
cd _build
cmake ..
make
sudo make install
```

## Install libzmq library
```
cd $C_LIBS
wget -O libzmq-4.2.3.tar.gz https://github.com/zeromq/libzmq/archive/v4.2.3.tar.gz
tar zxvf libzmq-4.2.3.tar.gz
cd libzmq-4.2.3
mkdir _build
cd _build
cmake ..
make
sudo make install
```

## Install cppzmq
```
cd $C_LIBS
wget -O cppzmq-4.2.3.tar.gz https://github.com/zeromq/cppzmq/archive/v4.2.3.tar.gz
tar zxvf cppzmq-4.2.3.tar.gz
cd cppzmq-4.2.3
mkdir _build
cd _build
cmake ..
make
sudo make install
```

## Install mongo-c-driver
```
cd $C_LIBS
wget https://github.com/mongodb/mongo-c-driver/releases/download/1.4.2/mongo-c-driver-1.4.2.tar.gz
tar zxvf mongo-c-driver-1.4.2.tar.gz
cd mongo-c-driver-1.4.2
./configure --disable-automatic-init-and-cleanup
make
sudo make install
```

## Install mongo-cxx-driver
```
cd $C_LIBS
wget -O mongo-cxx-driver-r3.0.2.tar.gz https://github.com/mongodb/mongo-cxx-driver/archive/r3.0.2.tar.gz
tar zxvf mongo-cxx-driver-r3.0.2.tar.gz
cd mongo-cxx-driver-r3.0.2
mkdir _build
cd _build
cmake -DBSONCXX_POLY_USE_BOOST=1 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..
make
sudo make install
```

## Build catapult-server
```
cd $C_HOME
git clone https://github.com/nemtech/catapult-server
cd catapult-server/
mkdir _build
cd _build
export PYTHON_EXECUTABLE=/usr/bin/python3
export BOOST_ROOT=/usr/local/include/boost
export GTEST_ROOT=/usr/local/include/gtest
export LIBMONGOCXX_DIR=/usr/local/lib/cmake/libmongocxx-3.3.0-rc0-pre
export LIBBSONCXX_DIR=/usr/local/lib/cmake/libbsoncxx-3.3.0-rc0-pre
export cppzmq_DIR=/usr/local/share/cmake/cppzmq
export ROCKSDB_ROOT_DIR=/usr/local/include/rocksdb
cmake -DCMAKE_BUILD_TYPE=RelWithDebugInfo \
-DCMAKE_C_FLAGS="-lpthread" \
-DCMAKE_MODULE_LINKER_FLAGS="-lpthread" \
-DCMAKE_SHARED_LINKER_FLAGS="-lpthread" \
-DBSONCXX_LIB:FILEPATH=/usr/local/lib/libbsoncxx.so \
-MONGOCXX_LIB:FILEPATH=/usr/local/lib/libmongocxx.so \
..
make publish
make
```
