
## 1传统的运筹优化

### 1.1框架工具：

- CVXPY：使用Python进行凸优化，可用于线性和非线性规划，半正定规划、混合整数规划等问题。（类似于casadi，本身是一个框架，可以调用其他求解器）。CVXPY 是一个纯粹的 python 优化工具接口，它本身并没有实现任何求解器，都是根据问题类型调用相应的外部求解器 [10](https://www.cvxpy.org/tutorial/advanced/index.html#choosing-a-solver)，然后解析结果。对于线性规划问题，可以调用外部的 CBC, GLPK, CPLEX, GUROBI, MOSEK 等。需要注意的是，当需要调用 CBC 时，必须先安装其官方 python 接口 CyLP；当需要调用 CPLEX 时也需要先安装其官方 python 接口。
  甚至cvxpy可以嗲用cvxopt的求解器。

- CVXOPT ：CVXOPT 实际上是一个凸优化工具包，并自带有线性规划求解器，但效率不是很高。但它可以调用外部求解器 [8](https://cvxopt.org/userguide/coneprog.html?highlight=lp#linear-programming)，包括 [GLPK](https://www.gnu.org/software/glpk/glpk.html), [MOSEK](https://www.mosek.com/) 和 [DSDP](https://www.mcs.anl.gov/hs/software/DSDP/)。

-  Pyomo：Python建模语言，可用于线性规划，非线性规划，混合整数规划等问题。
-  casadi
- PuLP：线性规划库，使用Python语言编写，简单易用。PulP 也可以调用 CBC, GLPK, CPLEX, GUROBI 等外部线性规划求解器。具体使用方法可以参考它的官方文档 [12](https://pythonhosted.org/PuLP/index.html)


### 1.2求解器

#### （1）开源

- osqp：二次规划库
- nlopt：非线性规划库
- scipy.optimize：SciPy中的优化模块，包括线性规划和非线性规划等算法。
- Optuna：用于超参数优化的库，可用于自动化机器学习和深度学习模型调参。
- SCIP：非商用优化软件，可用于解决线性规划、混合整数规划等问题。
- PySCIPOpt：Python界面的SCIP优化器，可用于混合整数规划、二次规划等问题。
- OR-Tools：Google出品的优化库，可用于线性规划、整数优化、约束优化、车辆路径规划等问题。
- DEAP：进化算法库，可用于优化问题，如遗传算法、粒子群算法等。
- Platypus：Python框架，用于多目标优化问题，可用于遗传算法、NSGA-II等。

#### （2）商用

- Gurobi：商用优化软件，提供Python API。
- CPLEX：商用优化软件，提供Python API，可用于线性规划、混合整数规划等问题。。
- MOSEK：商用优化软件，可用于线性规划、二次规划、混合整数规划等问题。

### 1.3 性能对比
- GLPK 对于变量规模很大的线性规划问题求解速度非常慢。
- CPLEX 求解大规模线性规划问题非常快，但是使用官方提供的方法建模时间非常长。实际测试，在变量规模为 65536 时，建模花费 20s 左右，求解花费不到 1s。当使用 CVXPY 调用 CPLEX 时，同样规模一共花费 5s 左右，推测主要时间也是花费在建模上面。
- 比较 CVXPY 调用默认求解器，CPLEX, GUROBI，发现速度差距不大，相对来说 GUROBI 可能会稍微好一点。


## 2进化算法（建议用scikit-opt）

- DEAP
- mealpy
- scikit-opt （国产良心）
- Geatpy2（国产用心）
- pygmo2
- pyswarms
- SciPy