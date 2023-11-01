## 库安装

### eigen3
```
sudo apt install libeigen3-dev
```
### osqp
```
git clone https://github.com/osqp/osqp.git
cd osqp
git submodule update --init --recursive
git checkout -b 0.6.0 v0.6.0
mkdir build && cd build
cmake ..
make -j7
sudo make install
```

## 源码安装

```
git clone https://github.com/robotology/osqp-eigen.git
cd osqp-eigen
mkdir build
cd build
cmake ..
make
make install
```

## 使用

见osqp-eigen/example（mpc）