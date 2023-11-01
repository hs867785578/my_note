
## 1.casadi的源码安装
```
sudo apt install gcc g++ gfortran git cmake liblapack-dev pkg-config --install-recommends
sudo apt install ipython3 python3-dev python3-numpy python3-scipy python3-matplotlib --install-recommends
sudo apt install swig --install-recommends
git clone https://github.com/casadi/casadi.git -b main casadi
```

## 2.库的安装

### ipopt的安装
```
sudo apt install coinor-libipopt-dev
```

### osqp的安装(0.6.0版本,casadi只支持0.6)
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

### qpoases安装

```
git clone https://github.com/coin-or/qpOASES.git
cd qpOASES
git submodule update --init --recursive
mkdir build && cd build
cmake ..
make -j7
sudo make install
```

## 3 casadi编译

```
//casadi/build目录下
cmake -DWITH_OSQP=ON -DWITH_LAPACK=ON -DWITH_QPOASES=ON -DWITH_IPOPT=ON ..
make -j7
sudo make install
```
### qpoases
## 4.demo 代码（nlp、qp）
```cpp
# 声明要求的 cmake 最低版本
cmake_minimum_required(VERSION 3.0)
set(CMAKE_CXX_STANDARD 11)
project(TEST)

# 添加一个可执行程序
add_executable(test qp.cpp)
# 添加相关库文件链接到工程
target_link_libraries(test /usr/local/lib/libcasadi.so.3.7)
# 设置输出的可执行文件存放目录
set_target_properties(test PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
```

nlp问题
```cpp
#include<iostream>
#include<vector>
#include <casadi/casadi.hpp>
using namespace std;
using namespace casadi;
int main()
{
   cout << "casadi_test" << endl;
// This is another way to define a nonlinear solver. Opti is new
/*
  *    min  x1*x4*(x1 + x2 + x3) + x3
  *    s.t. x1*x2*x3*x4 >=25
            x1^2 + x2^2 + x3^2 + x4^2 = 40
            1 <= x1, x2, x3, x4 <= 5
*/
//   Optimization variables
  SX x = SX::sym("x", 4);
  std::cout << "x:" << x << std::endl;
  // Objective
  SX f = x(0)*x(3)*(x(0) + x(1) + x(2)) + x(2);
  // SX f = x(0) * x(0)*x(3) + x(0)*x(1)*x(3) + x(0)*x(2)*x(3)+ x(2);
  std::cout << "f:" << f << std::endl;
  // Constraints
  // SX g = vertcat(6 * x(0) + 3 * x(1) + 2 * x(2) - p(0), p(1) * x(0) + x(1) - x(2) - 1);
  SX g = vertcat(x(0)*x(1)*x(2)*x(3), pow(x(0),2) + pow(x(1),2) + pow(x(2),2) + pow(x(3),2));
  std::cout << "g:" << g << std::endl;
  // Initial guess and bounds for the optimization variables
  vector<double> x0 = { 0.0, 0.0, 0.0, 0.0 };
  vector<double> lbx = { 1, 1, 1, 1 };
  vector<double> ubx = {5, 5, 5, 5 };
  // Nonlinear bounds
  vector<double> lbg = { 25, 40 };
  vector<double> ubg = { inf, 40 };
  // NLP
  SXDict nlp = { { "x", x }, { "f", f }, { "g", g } };
  // Create NLP solver and buffers
  Dict opts;
  // opts["print_time"] = false;
  //关闭结果打印
  // opts["ipopt.print_level"] = 0;
  Function solver = nlpsol("solver", "ipopt", nlp, opts);
  std::map<std::string, DM> arg, res;
  // Solve the NLP
  arg["lbx"] = lbx;
  arg["ubx"] = ubx;
  arg["lbg"] = lbg;
  arg["ubg"] = ubg;
  arg["x0"] = x0;
  res = solver(arg);
  // Print the solution
  cout << "--------------------------------" << endl;
  // std::cout << res << std::endl;
  cout << "objective: " << res.at("f") << endl;
  cout << "solution: " << res.at("x") << endl;
  return 0;
}
```

qp问题
```cpp
#include <casadi/casadi.hpp>
int main() {
    // 创建问题变量
    casadi::SX x = casadi::SX::sym("x", 2);  // 2维变量x
    casadi::SX f = x(0)*x(0)+x(1)*x(1);  // 目标函数 f = x1^2 + x2^2
    // 创建约束条件
    casadi::SX g = x(0) + x(1) - 1;  // 约束条件 g = x1 + x2 - 1
    // 创建问题对象
    casadi::SXDict qp = { { "x", x }, { "f", f }, { "g", g } };
    // 创建求解器
    casadi::Dict opts;
    casadi::Function solver = casadi::qpsol("solver", "qpoases", qp, opts);
    // 设置求解器参数
    std::map<std::string, casadi::DM> args, res;
    args["lbg"] = 0.0;  // 约束下界
    args["ubg"] = 0.0;  // 约束上界
    // 求解问题
    res = solver(args);
    // 输出结果
    std::cout << "Optimal solution: " << res.at("x") << std::endl;
    std::cout << "Optimal cost: " << res.at("f") << std::endl;
    return 0;

}
```