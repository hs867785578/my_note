ubuntu 18 安装ompl时不要安装1.6.0，要安装1.5.0
因为1.6.0需要cmake 3.16以上，而Ubuntu18最多支持3.10

参考方法2
https://blog.csdn.net/qq_18376583/article/details/127129778

[Index of /sites/distfiles.macports.org/ompl (mirrorservice.org)](http://www.mirrorservice.org/sites/distfiles.macports.org/ompl/?C=S;O=A)

下载[ompl-1.5.0.tar.gz](http://www.mirrorservice.org/sites/distfiles.macports.org/ompl/ompl-1.5.0.tar.gz)

*tar zxf*  [ompl-1.5.0.tar.gz](http://www.mirrorservice.org/sites/distfiles.macports.org/ompl/ompl-1.5.0.tar.gz)

安装依赖的库：

apt-get -y install g++ cmake libboost-serialization-dev libboost-filesystem-dev libboost-system-dev libboost-program-options-dev libboost-test-dev libeigen3-dev libode-devlibyaml-cpp-dev

install_python_binding_dependencies：

apt-get -y install python3-dev python3-pip libboost-python-dev libboost-numpy-dev python3-numpy

install_app_dependencies:

sudo apt-get -y install python3-pyqt5.qtopengl  freeglut3-dev libassimp-dev python3-opengl python3-flask python3-celery libccd-dev

sudo pip3 install -vU PyOpenGL-accelerate
sudo apt-get -y install libfcl-dev

mkdir -p build/Release
cd build/Release

cmake ../.. -DPYTHON_EXEC=/usr/bin/python${PYTHONV}

make -j 1
sudo make install