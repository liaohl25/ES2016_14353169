## cartograper 安装与测试

###安装说明

　　整个安装步骤主要是参考了ROS官网，以及以下两篇博客：
	
* ROS官网的ROS安装说明：<http://wiki.ros.org/jade/Installation/Ubuntu>
* hitcm 的博客： <http://www.cnblogs.com/hitcm/p/5939507.html>
* wen_hust 的博客： <http://www.cnblogs.com/wenhust/p/5961017.html>

###安装步骤

**1. 安装 ROS**

**1.1 配置 Ubuntu 软件仓库**

　　配置Ubuntu 软件仓库(repositories) 以允许"restricted"、"universe" 和 "multiverse"这三种安装模式。配置的方法参考[Ubuntu软件仓库配置指南](https://help.ubuntu.com/community/Repositories/Ubuntu)。
　　

**1.2 添加 sourses.list**

　　配置系统使其能够安装来自 packages.ros.org 的软件包：
<pre>
　　sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
</pre>
　　注意语句中的网址是镜像的链接。这里是[各地区的镜像文件和对应的指令](http://wiki.ros.org/ROS/Installation/UbuntuMirrors)，建议使用国内的或者新加坡的镜像源，安装下载的速度快。

**1.3 添加 keys**

<pre>
　　sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
</pre>

**1.4 安装**

　　首先更新 Debian软件包。
<pre>
　　sudo apt-get update
</pre>
　　然后安装ROS， ROS中有很多函数库和工具，这里有4种默认安装方式，也可以单独安装某个特定的软件包。推荐安装下面第一个桌面完整版的：

* 桌面完整版： 包含ROS、rqt、rviz、通用机器人函数库、2D/3D仿真器、导航以及2D/3D感知功能。
<pre>
　　sudo apt-get install ros-jade-desktop-full
</pre>

* 桌面版： 包含ROS、rqt、rviz以及通用机器人函数库。
<pre>
　　sudo apt-get install ros-jade-desktop
</pre>

* 基础版： 包含ROS核心软件包、构建工具以及通信相关的程序库，无GUI工具。
<pre>
　　sudo apt-get install ros-jade-ros-base
</pre>

* 单个软件包安装： 安装某个指定的ROS软件包（packagename为软件包名字）：
<pre>
　　sudo apt-get install ros-jade-packagename
</pre>

**1.5 初始化 rosdep**

　　使用ROS前需要初始化rosdep。rosdep可以方便在需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。
<pre>
　sudo rosdep init
  rosdep update
</pre>

**1.6 环境配置**

　　下面语句使得每次打开新的终端时，ROS环境变量都能够自动配置好：
<pre>
echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc
</pre>
　　若安装有多个ROS版本, ~/.bashrc 必须只能 source 当前使用版本所对应的 setup.bash。想要改变当前终端下的环境变量时，可以执行以下命令：
<pre>
source /opt/ros/jade/setup.bash
</pre>

**1.７ 安装 rosinstall**

　　rosinstall 是ROS中一个独立分开的常用命令行工具，它可以方便让你通过一条命令就可以给某个ROS软件包下载很多源码树。
<pre>
sudo apt-get install python-rosinstall
</pre>
　

**2. 安装依赖项**

　　指令比较长，输入的时候要小心。
<pre>
sudo apt-get update
sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev
</pre>
　

**3. 安装ceres solver**

　　首先安装ceres solver，选择的版本是1.11,路径随意，可以选择下载安装在主目录下。这里使用 hitcm 博主的 github 地址：
<pre>
git clone https://github.com/hitcm/ceres-solver-1.11.0.git
cd ceres-solver-1.11.0
mkdir build
cd build
cmake .. -G Ninja
ninja
ninja test
sudo ninja install
</pre>
　

**4. 安装cartographer**

　　安装 cartographer，路径随意，可以选择下载安装在主目录下。这里使用 hitcm 博主的 github 地址：
<pre>
git clone https://github.com/hitcm/cartographer.git
cd cartographer
mkdir build
cd build
cmake .. -G Ninja
ninja
ninja test
sudo ninja install
</pre>
　

**5. 安装cartographer_ros**

　　谷歌官方提供的安装方法比较繁琐，而且官方教程需要FQ下载一些文件，因此容易失败，hitcm 博主对原来的文件进行了少许的修改，核心代码不变，只是修改了编译文件。直接从hitcm 博主的 github下载：
<pre>
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build

# Create a new workspace in 'catkin_ws'.
mkdir catkin_ws
cd catkin_ws
wstool init src

# download to catkin_ws/src
cd src
git clone https://github.com/hitcm/cartographer_ros.git

# run catkin_make to install
cd
cd catkin_ws
catkin_make
source ./devel/setup.bash
</pre>
　　 下面语句使得每次打开新的终端时，环境变量都能够自动配置好：
<pre>
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
</pre>
　　若要改变当前终端下的环境变量，可执行命令：
<pre>
source source ~/catkin_ws/devel/setup.bash
</pre>
　

**6. 运行demo**

**6.1 下载数据**

　　下载2d、3d数据，可以选择使用迅雷下载，然后复制进Ubuntu，或者直接在终端下载，或者其他方便的方法。数据比较大，2d数据约500M，3d数据约3G。这里使用终端下载到Downloads文件夹：
<pre>
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/cartographer_3d_deutsches_museum.bag
</pre>

**6.2 运行文件**

<pre>
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/cartographer_3d_deutsches_museum.bag
</pre>

**6.3 运行结果**

　　2d图像：
<img src="" width="300" alt="configure"/>

　　3d图像：
<img src="" width="300" alt="configure"/>



