```
System： ubuntu16.04
libbson： 1.9.5.1
mongo-c-driver : 1.14.0
mongo-cxx-driver : 3.4.0    
```
---
### 1. libbson install
```sh
wget https://github.com/mongodb/libbson/releases/download/1.9.5/libbson-1.9.5.tar.gz

tar -xzf libbson-1.9.5.tar.gz

cd libbson-1.9.5.tar.gz/

./autogen.sh

make -j4

sudo make install
```

### 2. mongo-c-driver install
```sh
wget https://github.com/mongodb/mongo-c-driver/releases/download/1.14.0/mongo-c-driver-1.14.0.tar.gz

tar xzf mongo-c-driver-1.14.0.tar.gz

cd mongo-c-driver-1.14.0.tar.gz/build

cmake ..

make -j4

sudo make install
```
### 3. mongo-cxx-driver install
```sh
git clone https://github.com/mongodb/mongo-cxx-driver.git --branch releases/stable --depth 1

cd mongo-cxx-driver/build

cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..

sudo make EP_mnmlstc_core

make -j4

sudo make install

```
