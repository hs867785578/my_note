见
https://github.com/mayataka/hpipm-cpp

## 安装库

### eigen3
```
sudo apt install libeigen3-dev
```
### [blasfeo](https://github.com/giaf/blasfeo) and [hpipm](https://github.com/giaf/hpipm)
```
git clone https://github.com/giaf/blasfeo
git clone https://github.com/giaf/hpipm
cd blasfeo && mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON -DBLASFEO_EXAMPLES=OFF 
make -j8
sudo make install -j
cd ../../hpipm && mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON -DHPIPM_TESTING=OFF 
make -j8
sudo make install -j
```
### 设置动态库路径
```
export LD_LIBRARY_PATH=/opt/blasfeo/lib:/opt/hpipm/lib:$LD_LIBRARY_PATH
```

## 安装hpipm-cpp

```
git clone https://github.com/mayataka/hpipm-cpp
cd hpipm-cpp
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release 
make -j8
sudo make install -j
```

## 使用

见hpipm-cpp/example