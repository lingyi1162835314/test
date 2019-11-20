# wujian100_open介绍
    wujian100_open是一个基于MCU的SoC平台，支持通过EDA工具进行前端仿真和制作FPGA进行测试。同时我们也希望能有越来越多的开发者们可以参与进来，与T-head一同建立和发展开放的MCU ecosystem，让IC的设计和发展变得更加简单快速和可靠。
    
    项目目录结构
    |--Project               //开源项目工作目录 
      |--riscv_toolchain     //工具链安装目录，用户需要将工具链按照在该目录下。工具链可以在下载页面下载
      |--wujian100_open      //wujian100_open平台项目工程目录。wujian100_open 平台代码可以通过github下载获取
        |--case              //仿真使用的测试case
        |--doc               //wujian100_open平台的用户手册
        |--fpga              //FPGA制作相关脚本
        |--lib               //仿真编译使用的脚本及库文件
        |--regress           //回归测试的结果
        |--sdk               //软件开发套件
        |--soc               //SoC RTL源码
        |--tb                //test bench和monitor文件
        |--tools             //仿真脚本和环境变量设置文件
        |--workdir           //执行仿真的工作目录
        |--LICENSE
        |--README.md

# 预先准备
    1. 首先准备一个项目工作目录，如‘Project’
    2. 进入Project目录
    3. 从github获取平台代码：git clone https://github.com/T-head-Semi/wujian100_open.git 或 git clone git@github.com:T-head-Semi/wujian100_open.git

# 下载C/C++ Compiler
    1. 准备工具链安装目录‘riscv_toolchain’//使用c shell 命令，如‘mkdir  riscv_toolchain’
    2. 从下载页面下载工具链
    3. 解压工具链到riscv_toolchain目录下

# 准备开源EDA工具
    centos7/rhel7: sudo yum iverilog verilator gtwave
    ubuntu/debian: sudo apt-get iverilog verilator gtkwave

# 进行代码仿真
    1. 进入 wujian100_open/tools 目录下
    2. 打开setup.csh 文件，如果有vcs的license，请设置vcs相关的路径和license
    3. 执行source setup.csh命令设置环境变量 
    4. 环境变量设置完成后，进入wujian100_open/workdir 目录开始仿真
    5. 目前环境中支持iverilog和vcs 两种工具仿真。以timer 测试case为例：
       如果您想要使用iverilog 进行仿真，请在workdir目录下执行命令‘../tools/run_case -sim_tool iverilog ../case/timer/timer_test.c'
       如果您想要使用vcs 进行仿真，请在workdir目录下执行命令‘../tools/run_case -sim_tool vcs ../case/timer/timer_test.c' 
    6. 等待仿真结束，仿真结束会打印“Test Pass”表明本次仿真测试通过


# 如何制作FPGA bit文件
    1. 首先确保您的环境中有synplify和vivado工具的license
    2. 进入wujian100_open/fpga/synplify目录
    3. 打开synplify工具，同时加载wujian100_open_200t_3b.prj文件导入项目工程
    4. 在synplify console窗口输入命令‘sdc2fdc’，将sdc文件转换生成fdc文件
    5. 点击RUN 开始synplify综合，等待网表生成
    6. 在synplify 生成网表完成后，打开vivado工具 开始P&R和bit文件生成 
    7. 进入wujian100_open/fpga/vivado目录
    8. 在vivodo工具中导入'wujian100_open_200t_3b_prj.tcl'文件执行run tcl，等待bit文件生成
    9. 在bit文件生成后，将bit文件下载到FPGA开发板上
    10. 开始在FPGA上进行调试和软件应用开发
# 如何获取调试工具DebugServer
    从下载页面直接下载调试工具 
# 如何获取IDE开发工具CDK
    从下载页面直接下载IDE工具 

# SDK介绍 
    wujian100_open SDK是wujian100_open的软件开发工具包，软件遵循CSI 接口规范。通过该SDK用户可以快速对wujian100_open进行测试与评估，同时用户可以参考SDK 中集成的各种常用组件以及示例程序进行应用开发，快速形成产品方案。

SDK目录结构:
|--sdk
 |--csi_core 	//基于E902定义了CPU及与CPU紧耦合外设的标准接口规范以及实现
 |--csi_dirver  //定义外围设备驱动的标准接口规范，以及相关外围接口的实现
 |--csi_kernel  /定义了实时操作系统标准接口规范，以及Rhino、FreeRTOSv8.2.3、uCos-III等实时操作系统的对接示例代码。
 |--libs        //存放通用的库实现
 |--projects	//存放各种参考示例，包括benchmark 测试程序、驱动示例程序、rtos 示例程序等。同时包括了相关的工程项目文件。
 |--utilites	//存放项目配置文件
 |--VERSION

    1. 下载和安装CDK工具
    2. 使用CDK工具打开项目工程，如打开hello world项目工程：
       projects/examples/hello_world/CDK/wj100-open-hello_world.cdkproj
    3. 编译工程
    在工具栏窗口点击‘project’，选择‘build all’。等待编译完成后，您将看到如下打印:

Build target ' wujian100_open-hello_world BuildSet '
----------Building project:[ wujian100_open-hello_world - BuildSet ]----------
make[1]: Entering directory 'D:/release/Wujian100_open-V1.0.0/Wujian100_open-V1.0.0/projects/examples/hello_world/CDK'
make[1]: Leaving directory 'D:/release/Wujian100_open-V1.0.0/Wujian100_open-V1.0.0/projects/examples/hello_world/CDK'
make[1]: Entering directory 'D:/release/Wujian100_open-V1.0.0/Wujian100_open-V1.0.0/projects/examples/hello_world/CDK'
linking...
size of target:
   text	   data	    bss	    dec	    hex	filename
  22680	   1628	   6660	  30968	   78f8	D:/release/Wujian100_open-V1.0.0/Wujian100_open-V1.0.0/projects/examples/hello_world/CDK/Obj/wujian100_open-hello_world.elf
checksum value of target:  0xE2B2C769 (491,388)
make[1]: Leaving directory 'D:/release/Wujian100_open-V1.0.0/Wujian100_open-V1.0.0/projects/examples/hello_world/CDK'
Executing Post Build commands ...
Done
====0 errors, 0 warnings, total time : 20s263ms====

    4. 开始调试:
    在工具栏窗口点击‘Debug’，选择‘Start/Stop Debugger’

# 讨论

如果想要对wujian100_open工程进行讨论和建议，可以通过以下链接打开二维码，使用钉钉扫描进入我们的讨论群：
    ![讨论群](https://cop-image-prod.oss-cn-hangzhou.aliyuncs.com/mcu/q.jpg)
