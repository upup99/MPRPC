# MPRPC

### 一、介绍

本项目是由C++开发的分布式网络通信框架(RPC)，为了分布式环境下服务器之间进行远程方法调用提供了一套接口，通过该框架可以快速的实现RPC方法调用。

本项目基于muduo网络库、protobuf、zookeeper开发，并使用该框架提供了一套框架代码使用范例，在example目录下。



### 二、技术栈

项目开发涉及到以下几个方面

- RPC远程过程调用原理以及实现
- Protobuf数据序列化和反序列化协议
- ZooKeeper分布式一致性协调服务应用以及编程
- 网络库编程
- conf配置文件读取
- 异步日志模块
- CMake构建项目集成编译环境
- github管理项目



### 三、项目代码工程目录



```
|-- bin 可执行文件，这里放的是example编译好的二进制文件
|-- build 项目编译文件
|-- example 框架代码使用范例
|-- lib 项目库文件
|-- src 源文件
|-- test 放的是一些util和net的测试代码
|-- CMakeLists.txt 顶层的cmake文件
|-- README.md 项目自述文件
|-- autobuild.sh 一键编译脚本
```

### 四、环境配置

本项目使用了部分第三方库及环境配置，需要提前部署安装。

#### 1. 实验环境

Linux版本：CentOS 7

#### 2. protobuf安装配置

##### protobuf介绍

protobuf（protocol buffer）是google 的一种数据交换的格式，它独立于平台语言。  google 提供了protobuf多种语言的实现：java、c#、c++、go 和 python，每一种实现都包含了相应语 言的编译器以及库文件。  

由于它是一种二进制的格式，比使用 xml（20倍） 、json（10倍）进行数据交换快许多。可以把它用 于分布式应用之间的数据通信或者异构环境下的数据交换。作为一种效率和兼容性都很优秀的二进制数 据传输格式，可以用于诸如网络传输、配置文件、数据存储等诸多领域。

##### CentOS下protobuf环境搭建

- 直接在github上下载源代码：[protocolbuffers/protobuf: Protocol Buffers - Google's data interchange format (github.com)](https://github.com/protocolbuffers/protobuf)
- 百度网盘：`链接：https://pan.baidu.com/s/1GHk-LaoXqAnWb40-VbHGBQ 提取码：nqg1` （版本为3.11.0）
  

安装过程如下：

```
1、解压压缩包：unzip protobuf-master.zip
2、进入解压后的文件夹：cd protobuf-master
3、安装所需工具：sudo yum install autoconf automake libtool curl make g++ unzip
4、自动生成confifigure配置文件：./autogen.sh
5、配置环境：./confifigure 
6、编译源代码(时间比较长)：make
7、安装：sudo make install
8、刷新动态库：sudo ldconfig
```

> notes：安装完之后可能依然会出现找不到动态库文件libprotobuf.so.22，这时可以使用find命令先找到该动态库文件在哪里，执行以下命令
>
> ```shell 
> echo 'export LD_LIBRARY_PATH=/path/to/your/library:$LD_LIBRARY_PATH' >> ~/.bashrc
> source ~/.bashrc
> ```

#### 3. Zookeeper安装配置

##### Zookeeper介绍

Zookeeper是在分布式环境中应用非常广泛，它的优秀功能很多，比如分布式环境中全局命名服务，服 务注册中心，全局分布式锁等等。 参考链接： https://www.cnblogs.com/xinyonghu/p/11031729.html

##### CentOS下Zookeeper环境搭建

- Zookeeper是用java写的，需要提供java环境，建议java11

- github源码链接：[Apache ZooKeeper](https://zookeeper.apache.org/releases.html)

- 百度网盘：`链接: https://pan.baidu.com/s/1yPl1aNGuC7OVabs5icw1Ag?pwd=92xe 提取码: 92xe`（版本为3.4.10）

安装过程如下：

```
1、下载zookeeper-3.4.10.tar.gz解压后进入zookeeper-3.4.10
2. cd zookeeper-3.4.10/conf
3. mv zoo_sample.cfg zoo.cfg
4. 进入bin目录，启动zkServer， ./zkServer.sh start
5. sudo netstat -tanp 查看zkServer的端口（默认2181）在bin目录启动zkClient.sh链接zkServer，熟悉zookeeper怎么组织节点
```

##### zk的原生开发API（c/c++接口）

- 进入上面解压目录src/c下面，zookeeper已经提供了原生的C/C++和Java API开发接口，需要通过源码编译生成，过程如下：

```
~/package/zookeeper-3.4.10/src/c$ sudo ./confifigure
~/package/zookeeper-3.4.10/src/c$ sudo make
~/package/zookeeper-3.4.10/src/c$ sudo make install
```

- 主要关注zookeeper怎么管理节点，zk-c API怎么创建节点，获取节点，删除节点以及watcher机制的 API编程。

> notes：在这里执行sudo make时，可能会报警告错误，这是因为在makefile文件中设置了-Werror选项，使得警告也被看做错误，进入makefile文件中找到-Werror，删除该选项即可

##### 4. muduo库安装配置

[C++ muduo网络库知识分享01 - Linux平台下muduo网络库源码编译安装-CSDN博客](https://blog.csdn.net/QIANGWEIYUAN/article/details/89023980)



