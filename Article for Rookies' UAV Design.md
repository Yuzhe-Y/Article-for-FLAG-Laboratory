# 从零开始的无人机制作过程


## 0. 写在开头

这篇文章主要记录从24年9月到24年11月，参加2024年201所“狭路先锋”比赛过程中无人机组从小白开始的无人机制作以及调试过程。本文主要分为以下部分：无人机选型、设计及安装，ubuntu系统基本配置，第一次起飞的准备，代码讲解。由于是第一次进行无人机系统组装及调试，文章中难免会出现一些错误或漏洞，如有发现或有任何问题请与作者进行沟通及反馈，万分感谢！  

## 1. 无人机选型、设计及安装

该段落主要介绍本次参赛的无人机具体结构及相关选型（包括所有用具），无人机设计改进方法（轴距、桨距、电机及螺旋桨选型、布局及重量分配），以及整体组装过程中需要注意的事情。

### 1.1 具体元件选型

- __机架__：自制450mm轴距无人机碳板机架（带有桨保，起落架）
- __NUC（主机）__：intel NUC（12代、13代，16+512）  
- **激光雷达**：LIVOX MID360激光雷达（带有金属固定片，向前倾斜角度约**14度**）
- **电机**：老虎电机T-Motor Cine66 KV925/1155
- **螺旋桨**：老虎电机T-Motor C9.5寸
- **飞控**：pixhawk 6c
- **电调**：老虎电机T-Motor P60A V2
- **遥控器**：乐迪 AT9S PRO
- **遥控接收器**：R12DSM 2.4G 12通道
- **视觉相机**：INTEL D435i
- **电池**：格式电池
- **降压模块**：淘宝商家定制
- **分电板**：FLAG实验室自制

此外，除了以上的重要元件，还有许多元件需要进行准备：

电池充电器、12AMG等规格供电线、xt30立式（侧式）公母插头、热熔胶及热熔胶枪、扎带、魔术贴、电工胶、m3规格铝柱及螺丝、typec转usb线、电调与飞控通信线（10pin转8pin，10pin线端口GH1.25，8pin线端口SH1.0）、焊锡、焊台、焊油、八爪鱼、热缩管、剥线钳、万用表、螺丝刀、拐棒、**螺旋桨专用套杆**、激光雷达及相机专用线材、键盘、鼠标、电源适配器、显示屏

以上为无人机制作过程中所需要的大部分所需用具，如有遗漏；欢迎补充。

家旭同学为我们提供了两套无人机机体的设计方案，链接如下，具体提取码请联系石家旭同学：

> UAV0517(350mm轴距)：
> 链接：https://pan.baidu.com/s/18DhfVMhEfOZPhQM07YBK6w

> 加长新版设计图(450mm轴距，可搭载9、9.5寸螺旋桨)：
> 链接：https://pan.baidu.com/s/1SGTQN3qnIQvK69OQ-XM08g

### 1.2 无人机具体结构的设计原理

在整体无人机设计的过程中，我们遇到了许多设计上相关的问题，比如在续航方面的选择。由于我们采用的是学长比赛时候的飞行器，其设计不适用于长距离、长时间飞行，因此在我们进行测试分析后，认为我们需要对无人机的具体结构上进行再设计以满足我们的需求。

无人机结构上可以通过改进来进行提升的地方主要有：飞行器整体结构的设计，电机及螺旋桨的选型、飞行器整体布局的选择等方面。

#### 1.2.1 飞行器整体结构的设计

在飞行器整体结构设计上，我们主要进行改进的地方有：**桨距的改变，轴距的改变**。准确来说，桨距的改变这个作用，我也才是第一次知道，在进行结构改进的过程中，改变桨距只是为了能够让9.5寸的桨能够顺利放进飞行器中且不打到主体结构，但是根据gpt以及文心一言的介绍，我们得到以下结论：

> 桨距较小，提供较高的推力，适合起飞和悬停等需要大升力的场景。但高速飞行时效率较低，会导致更多能量损耗。
>
> 桨距较大，提供更高的空气推进效率，适合巡航飞行或低载荷场景。但起飞时可能导致电机负载过大，增加耗电量。

因此，我们有以下结论：**对于长时间悬停任务，选择中小桨距以减少能量消耗，对于远距离巡航任务，选择大桨距以提升气动效率。**

在轴距上我们也做出了一定的调整，采用更长的轴距设定也能增加其续航，以下为gpt的回答：

> 螺旋桨之间距离更远，下洗气流相互干扰减少，提升气动效率。每个螺旋桨可以以较低的转速提供相同的升力，从而减少能耗。适当增加轴距可以提高气动效率，但轴距过大会增加机架重量。
>
> 大轴距惯性矩增大，姿态调整的幅度更精确且稳定。在巡航中，飞行器的动态平衡更容易维持，从而减少调整所需的能量。但响应速度稍慢，适合长时间飞行而非快速机动。
>
> 电机分布更广，推力分布更加均匀。在长时间巡航时，可以更好地保持飞行稳定性，减少能耗。

轴距的增大也会导致重量的增加，我们需确保增加的轴距所带来的气动效率和稳定性提升，能弥补因重量增加导致的额外功耗。

#### 1.2.2 电机及螺旋桨的选型

在原来的比赛中，飞行器选用的是kv值为925的电机以及8寸的螺旋桨，但是经过询问德龙学长以及子辰学长后，我们了解到采用**尺寸更大的螺旋桨及kv值更低的电机**有利于能耗的降低。通过上网查阅资料，我们能够知道：

> 选择低kv值及高效的电机：KV表示每伏电压下的电机空载转速，低KV值电机意味着较低转速和较高扭矩。低KV电机适合搭配大桨距或大直径螺旋桨，以低转速提供更高推力。更高效率的电机将减少发热和能量浪费，延长续航时间。
>
> 螺旋桨尺寸的增大：螺旋桨直径增加，能推动更大的空气量，从而在较低转速下产生足够的升力。较低的转速减少湍流和摩擦损失，提高气动效率。
>
> 选择直径较大的螺旋桨时，应配备低KV值电机以维持合理转速。

此外，我们可以通过电机官网，查阅电机相关文档来进行选型。以老虎电机为例，对于不同电机，官网文档中都会给出不同螺旋桨尺寸下不同推力下功率、拉力及其力效，通过对比力效及其他方面的参数，也能为我们的挑选提供参考，以下为一个参考案例：

<img src="http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751566.png" alt="电机参数图" style="zoom: 50%;" />

以上图片中给出的参数都可以为我们的选择提供一些帮助。

#### 1.2.3 飞行器整体布局的改进

该部分主要以飞行器上的元件布局为主要介绍对象。

首先最重要的一点是：**注意飞控与电机旋转轴平面的关系，在实践中飞控应与电机输出轴最底端的绿色线圈在同一水平面上**。这个安装要求来自于德龙学长的提醒。其实根据网上查阅的资料我们得到飞控安装在飞行器重心处是最好的安装地点，因为接近重心的位置能减少姿态变化对传感器读数的干扰，提供更准确的运动信息。但我们在进行设计时一般难以估计飞行器重心的位置，因此我们将飞行器电机输出轴最底端的平面认为是重心所在平面。

其次，是飞行器各元件的布局。四旋翼飞行器要求我们做到重量分布对称，因此我们在进行设计的时候确保整体结构在大体上是对称的，如果存在无法与机架进行紧固的部分，我们也需要将其置于飞行器中心位置，减少因不对称产生的额外功率浪费。同时在上下重量的分布上我们也应做好设计，要求重心所在平面的上下部分重量分布大致一致，否则飞行器的控制难度将会增加。

### 1.3 整体组装过程中需要注意的事项

完成飞行器选型、设计及优化后，我们将进行飞行器的组装，接下来将会用列表的形式列出在安装过程中我们需要注意的地方。

1. 注意各个元件的安装顺序，不合理的安装顺序会大大延长装机时间，进行更换时也会更难操作，最好在进行设计时就考虑安装过程。

2. 注意元件安装的合理性，不合适的安装位置或其他不合理的地方容易导致安装时间的延长，操作难度增大。

3. 电调与电机的焊接需注意何时进行，过早焊接或过晚焊接都会使难度加大。此外，**12根电机三相线可以不按照一定的顺序进行焊接，只需要每三根电机线焊接在对应电调的三个连续的焊盘上即可**，顺序可以随后通过软件进行调整。

4. 电调焊接的时候供电线上的电容可以不进行焊接（也可以进行焊接来减少尖峰电流的冲击）。

5. 激光雷达一分三的线，其中有一根备用线不需要使用，另外两根根为网线和供电线，供电线在焊接的时候需要注意供电线正负极及屏蔽线的区分。

6. 激光雷达在安装的时候需要进行加固处理，减少激光雷达的抖动，并固定激光雷达的角度，方便其初始化。

7. 对电机进行固定的时候使用的螺丝不应过长，防止螺丝与电机线圈接触产生电火花，引发安全事故。

8. 螺旋桨安装的时候要求紧固，能够随电机转动而转动，防松螺母也需要紧固。

9. 注意螺旋桨安装的正反。螺旋桨分正螺旋桨与反螺旋桨，安装时根据电机转动方向进行不同安装。

10. 飞控与电调的通信线应按照飞控与电调的说明手册严格进行焊接（对于本机来说，采用10转8通信线，总共有5根线需要进行焊接，分别为地线，4根pwm控制线，飞控上总计10个接口，确定4个pwm控制线分别哪根线对应哪个电调上的1号端口，多余的线剪掉）。

   例：[pixhawk 6c 引脚示意文档](https://docs.holybro.com/autopilot/pixhawk-6c/pixhawk-6c-ports)    

   <img src="http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751568.png" alt="2024-11-20 17-06-27屏幕截图" style="zoom:50%;" />

   <img src="http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751569.png" alt="2024-11-20 17-07-22 的屏幕截图" style="zoom:50%;" />

   通过以上两张图进行介绍，从电调图中我们可以看见每三个焊盘对应一个编号，这也就意味着每个电机对应一个编号（**注意，这里的编号与后面我们介绍的控制中的电机编号不同，在这里仅针对电调焊接时使用**），通过查阅飞控与电调的端口号，我们需要进行这样的连接：飞控的ch1端口与电调上标号1的接口进行连接，以此类推。（**严格按照1对1，2对2的方式进行接线**）

11. 遥控与飞控的通信线为5转3线，购买遥控时需要一起进行购买，接在飞控 SBUS RC 接口处。

12. 注意激光雷达安装方向，连接一分三线的边为雷达的后端。

13. 电脑（NUC）以及其他的控制设备做好一定的防护，包括电气防护以及物理防护，可以增加3D打印保护壳或其他方式进行防护，减少铝柱与nuc接触的次数，采用绝缘的物体进行nuc安装，留出电脑风扇出风口。

14. 坠机后电脑如果开不了机，80%为内存或固态的问题，检查后更换即可，否则是电脑供电问题，需及时检修。

15. 降压模块做好绝缘，降压模块输入端与输出端最好不要采用压线的方法而是采用焊接的方法进行连线。

16. 分电板焊接采用侧插插头，注意焊接时的操作，做好绝缘，注意焊接插头的选择。

17. 整机电路一定做好绝缘，线缆安装时注意防止被螺旋桨打到。

18. 整机结构设计时尽可能压缩，即塔式结构不要堆叠太高，会使重心过高，难以控制。

以上为进行整机拼装及焊接时候需要注意的地方，如有补充，欢迎与作者联系。

## 2. UBUNTU系统基础配置

本部分主要介绍ubuntu系统的安装（双系统配置），ubuntu环境基础配置（VSCODE，QGC，向日葵，科学上网，git，网卡配置），程序相关环境配置（ROS系统，MAVROS，CasADI，激光雷达）三部分

### 2.1 UBUNTU系统安装

在网上有许多关于虚拟机或双系统进行ubuntu系统安装的文章，在这里我就不做过多介绍，下面会列出一些相关文章共参考。作者采用的是双系统方式，相比于虚拟机，双系统能够很好的利用电脑的资源，但是与虚拟机相比，两个系统间无法直接进行文件的传输是最大的痛点，因此仁者见仁智者见智，大家可以上网搜索两者的对比，选择自己喜欢的方式进行安装。

在安装双系统的时候，大家大概率会出现这样的一个问题：明明我的系统盘已经制作好了，但是插入系统盘，选择u盘启动后，能听见ubuntu系统的声音，却看不到画面。大家不用担心，这个是正常的事情，是因为NVIDIA显卡驱动不兼容的问题导致的。大家可以选用两种方法进行解决，第一种是进入到windows系统中或bios中，选择核显进行启动，另一种是按照如下文章的方法利用指令进行核显启动。

此外，飞行器上的nuc由于采用ubuntu单系统，其安装步骤比双系统或虚拟机简单的多，因此在这里不做过多赘述。

**双系统安装**：

> [windows10 和 Ubuntu双系统安装](https://blog.csdn.net/LIWEI940638093/article/details/113726447)
>
> [U盘配置Ubuntu系统 ——（Win10+Ubuntu20.04）双系统安装教程](https://blog.csdn.net/WS_Change/article/details/141126131)
>
> [Ubuntu双系统安装](https://blog.csdn.net/jy15246781299/article/details/133667186)
>
> [Ubuntu18.04完整新手安装教程和分区设置](https://blog.csdn.net/chencaw/article/details/101106073)

**双系统安装时黑屏问题解决**：

> [ubuntu安装中黑屏卡住](https://www.zcbm580.com/pos/o7o3eve7g.html)
>
> [笔记本Ubuntu系统的显示问题：安装前黑屏、安装后黑屏、外接显示器黑屏](https://blog.csdn.net/weixin_45291614/article/details/135106749)

### 2.2 UBUNTU环境基础配置

完成ubuntu系统的安装，接下来进行对应的ubuntu基础环境配置。针对这个比赛，我们主要所需的环境配置为：基础软件如vscode等的安装，基础配件如clash verge的安装等

对于本次比赛，我们所需的基础软件主要有：VSCODE（CLION），QGC（QGroundControl）,NoMachine（向日葵）

这些软件的安装教程网上一抓一大把，可以根据网上的教程完成相关的配置，不过多在这里进行阐述

除开以上的基础软件，我们还需要对我们电脑的一些基础环境进行对应配置，最主要的就是科学上网与代码管理。

科学上网，在ubuntu系统中我们采用clash verge。大家可以通过上网搜索，马上找到对应clash verge的最新版本，但大部分人经过尝试后都发现，clash verge v1.3.8能够进行安装，但是双击后会发现没有图形化界面出现，通过与学长交流后，他们也被这个问题困扰了许久，大部分人都是通过在终端中键入命令来进行科学上网的。因此，有一个方便的图形化界面十分重要。

经过我在网络上的不断搜索，终于在一个帖子中找到了一个可能的解决办法。在这个帖子中，提问人也是受到没有图形化界面的困扰，clash verge的上传作者最终推荐使用clash verge v1.3.8的debug版本，最终成功解决该问题。我也使用同样的方法，最终成功实现了clash verge的图形化，下面贴上网址连接：[Clash Verge Alpha](https://github.com/zzzgydi/clash-verge/releases/tag/alpha)

经作者测试，对于ubuntu20.04系统，该版本能够正常显示clash verge图形化界面，大大降低终端键入成本。

网络上同时也提供了使用更低的版本如v1.3.5的方法进行解决，但作者还没有进行测试，感兴趣的可以测试一下。当然，这个版本的clash verge还是存在一些小问题比如当开启时间过长会导致cpu占用率升高（风扇酷酷转），不过还是在能接受的范围内的。（241125补充：安装nvidia显卡驱动后问题消失，可以完全放心使用clash verge alpha v1.3.8）

另外一个必备的，就是git。git作为代码管理工具，对于这种需要多人协作的中型工作场景十分有用。网络上针对git进行配置的教程还是比较多的，大家可以上网自行搜索安装。安装完git后，也可以进行ssh免密连接github或gitee的操作：[Ubuntu系统SSH免密连接Github配置方法](https://blog.csdn.net/jks212454/article/details/140372251)

补充：对于我们的笔记本或台式机，一般不会出现系统配置完后无法联网的情况，但是对于我们使用过的nuc，我们发现在安装完系统后，系统右上角并没有出现WIFI图标，并且在设置中我们也没有找到WIFI相关配置。通过上网查询后发现是网卡驱动缺失。解决办法也很简单，通过阅读下面给出的文章即可解决问题：[Ubuntu20.04上无法识别英特尔WiFi6EAX211无线网卡](https://blog.csdn.net/u011391361/article/details/137016824)

### 2.3 程序相关环境配置

本部分主要介绍程序相关的环境配置。环境配置主要有：ROS系统，MAVROS，CasADI，激光雷达四部分。

#### 2.3.1 ROS系统配置

对于第一次使用ubuntu系统或对ubuntu系统还是不是很熟悉的读者可以按照下面列出的文章中的安装方式进行一键安装，简单省时省事省力，奶奶来了都能装上，就是会臃肿一点。如果认为自己对ubuntu很熟悉、想节省空间或想探索的读者也可以自己上网搜索安装方法。注意安装版本为 **ROS1 ROS Noetic（20.04对应）**

> [鱼香ROS网站上线|一行代码安装ROS/ROS2/解决rosdep问题|小鱼脚本](https://blog.csdn.net/qq_27865227/article/details/120277420)

#### 2.3.2 MAVROS安装

这部分主要是针对px4系统及mavros的安装。这部分花费的时间比较长，但是网络上的参考文档还是比较多的，在这里面主要推荐三个文档，其中第一个文档为px4官方提供的安装文档，可以着重参考这个，其他两个也可以参考。都看看没有坏处。

> [ROS (1) with MAVROS Installation Guide](https://docs.px4.io/main/en/ros/mavros_installation.html)
>
> [Ubuntu20.04+ROS1+PX4+Gazebo仿真（三）环境配置](https://www.bilibili.com/opus/934654041226477571?plat_id=35&share_from=article&share_medium=android&share_plat=android&share_source=WEIXIN&share_tag=s_i&spmid=read.column-detail.0.0&timestamp=1732106115&unique_k=MLoAwwz)
>
> [Ubuntu20.04+MAVROS+PX4+Gazebo保姆级安装教程](https://www.bilibili.com/opus/782824589525778437?plat_id=35&share_from=article&share_medium=android&share_plat=android&share_source=WEIXIN&share_tag=s_i&spmid=read.column-detail.0.0&timestamp=1732106126&unique_k=sAvZvPG)

其中有一点需要注意的是安装完px4后利用后两个教程的指导，是能够正常让无人机起飞及降落的，大家注意一下。

此外，在安装mavros的时候有两种方式，分别为二进制安装以及源码安装，这两种安装方式均可，作者采用的是第一种方法，经过询问后第二种方法也能成功。

#### 2.3.3 CasADI安装

CasADI是一个开源的计算机代数系统，专注于提供用于自动微分和非线性优化的功能。程序中使用他主要用来进行NMPC中非线性部分的求解。针对这一部分的安装，德龙学长留下了一篇安装文档供我们参考：[Ipopt和CasADi安装配置](https://github.com/Hydrogen2000/Installation-of-Ipopt-and-CasADi)

读者可以自行根据文章中的内容和提供的参考资料进行安装，德龙学长已经将安装内容写的很详细了。

**！！！注意！！！**

**按照以上步骤完成Ipopt和CasADI安装后，大部分情况下都是能够正常运行的，但是在我们组装的无人机NUC上，运行代码后会产生报错，通过阅读报错原因显示为Ipopt没有安装，我们猜测可能是部分环境没有进行配置**，因此上网搜索安装教程，根据该教程：[Ubuntu 20.04源码安装Ipopt](https://blog.csdn.net/tianjunc/article/details/143319954)，我们最终发现在这篇文章的最后一步中，除了针对

```bash
sudo cp coin-or coin -r
```

的修改外，文章中还对环境进行了如下的更改：

```bash
sudo ln -s /usr/local/lib/libcoinmumps.so.3 /usr/lib/libcoinmumps.so.3
sudo ln -s /usr/local/lib/libcoinhsl.so.2 /usr/lib/libcoinhsl.so.2
sudo ln -s /usr/local/lib/libipopt.so.3 /usr/lib/libipopt.so.3
```

增加以上环境变量配置后成功运行程序。

#### 2.3.4 激光雷达环境配置

无人机上采用的激光雷达为Livox MID360，因此我们在配置环境的时候SDK采用Livox SDK2，雷达驱动采用livox_ros_driver2。网络上有许多相关的配置文档，在这里我列出几个供大家参考：

> [Livox-Mid-360激光雷达配置](https://blog.csdn.net/m0_49384824/article/details/142483862)
>
> [Livox-mid-360激光雷达ip配置](https://blog.csdn.net/Hahalim/article/details/129414327?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%A4%9Amid360%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E6%A0%87%E5%AE%9A&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-6-129414327.142%5Ev100%5Epc_search_result_base2&spm=1018.2226.3001.4187)
>
> [Livox mid360 激光雷达运行 fast-lio2 详细教程](https://blog.csdn.net/m0_55117804/article/details/142644882)

在这里面大家需要注意一点，在进行livox_ros_driver2的安装时，最后一步

```bash
./build.sh ROS1
```

需要在 **../src/livox_ros_driver2** 这个文件夹下进行

**！！！注意！！！**

**不论是使用我们程序中淏宇学长修改的 ra-lio SLAM算法，还是开源的SLAM算法例如 fast-lio2，都应在对应存放SLAM算法的工作空间中将文件夹 ws_livox/src/ 中的 livox_ros_driver 文件夹复制过去（作者使用的这两个SLAM算法均为这种情况，其他SLAM算法按照对应网页介绍或相关教程处理）**

## 3. 第一次起飞的准备

完成上面所有的无人机组装工作与NUC环境配置工作后，我们正式开始让无人机起飞啦。

首先我们需要针对QGC以及PX4飞控进行一定的文档了解，[ PX4自动驾驶用户指南(v1.13)](http://docs.px4.io/v1.13/zh/)是我们进行初步了解最重要的指南手册。通过该指南手册，我们可以快速上手PX4飞控以及QGC地面站。

作者推荐阅读该文档中的：

1. Introduction（简介）
2. Getting Started（入门指南） 中除  Payloads and Cameras（载荷&相机） 外的全部内容
3. Basic Assembly（基本装配） 中的 Mounting the Flight Controller（安装飞控）, Vibration Isolation（震动隔离）, Holybro Pixhawk 6C Wiring Quick Start
4. Basic Configuration（基本配置） 中的所有内容
5. Flying（飞行） 中的 First Flight Guidelines（首次飞行指南） 和 Flying 101（飞行基础指南）
6. Flight Log Analysis（飞行日志分析）
7. Advanced Configuration（高级配置） 中的 Finding/Updating Parameters（查找/更新参数） 和 Multicopter Configuration（多旋翼配置/调试）
8. Hardware (Drones & Drone Parts)（硬件） 中有关 Holybro Pixhawk 6C 的部分
9. Development（开发） 中的 Concepts（概念） - PX4 Flight Stack Architecture（PX4飞行器堆栈结构） 和 Controller Diagrams（控制器框图）
10. Drone Apps & APIs（无人机应用程序） 中的 Offboard Control from Linux 和 ROS - ROS (1) with MAVROS - MAVROS *Offboard* control example (C++)

该文档中的其他内容也非常值得阅读，如果有时间的话就把整个文档都读一遍吧。

### 3.1 遥控器配对

遥控器配对比较简单，首先确保周围仅存在一个即将配对的遥控器，随后长按遥控接收器侧面的小按钮，等待遥控接收器上面的指示灯闪烁，松开，观察遥控及遥控接收器，如果遥控接收器上的指示灯等待几秒变为长亮且遥控器上显示出类似于信号满格的图像后代表配对成功。

### 3.2 地面站校准飞控

打开QGC，点击左上角QGC标志+"Vehicle Setup"进入设置界面，设置界面中有许多需要调整的选项。

首先查看飞控的概况。对于一个全新的 Holybro Pixhawk 6C 飞控， 其固件版本应该是 v1.13.3 （或v1.13的其他版本），若不是，参照下方的烧录固件文档重新进行固件烧录。

![2024-11-23 21-05-38屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751570.png)

第二步，进行机架调整。在左侧选择“机架”，找到众多选择中的“Quadrotor X（四旋翼无人机X型）”，点击下拉列表中的 “Generic Quadcopter“

![2024-11-23 21-14-12 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751571.png)

第三步，进行传感器校正。对于四旋翼飞行器，总共需要进行校准的传感器有：**罗盘、陀螺仪、加速度计**，在进行校准前，首先要进行飞控方向的选择。如下图所示，飞控方向可以进行选择，其中包括三轴角度的偏置。在飞行器设计的时候，飞控三轴并没有进行旋转等操作，因此此选项选择 “ROTATION_NONE”，按确定，开始进行校准。按照画面中显示的飞机（飞控）放置方向进行传感器的校准，注意QGC的提示是旋转机身还是静止放置。当框体全部变绿即校准完成。

![2024-11-23 22-39-48 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751572.png)

![2024-11-23 22-35-30 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751573.png)

**！！！注意！！！**

1. **进行传感器校准的时候请断开电池电源，电流磁效应可能会影响传感器的校准。**
2. **当传感器校准需要旋转机身的时候，请尽可能保持机身角度稳定，并进行柔和的左右旋转。**
3. **若调整或取下飞控再重新安装，或来到不同城市后，均应重新对飞控传感器进行校准。**

第三步，完成校准后，我们进行遥控器对频及摇杆拨杆通道测试。无人机遥控器分为两种设计，油门摇杆位于左侧（日本手）和油门摇杆位于右侧（美国手）。在我们大部分应用场景中，我们的遥控都是油门摇杆在左，因此当我们进入下面第一个界面的时候，首先查看右侧选项是否为日本手，若不是，则改为日本手。随后，我们推动遥控左右摇杆，观察通道是否正确且是否反向。

![2024-11-23 22-59-36 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751574.png)

**！！！注意！！！**

**无人机遥控摇杆通道分别为：左摇杆上下：油门控制（z轴控制，采用HOLD方式），左摇杆左右：方向控制（yaw角控制），右摇杆上下：前进后退控制（pitch角控制），右摇杆左右：左偏右偏控制（roll角控制）。**

随后针对遥控上的拨杆，我们也要进行调整。我们采用的遥控正面上方总共有4个拨杆（两个摇杆上方），我们利用的为右侧3个拨杆（最左侧的不用）。点击QGC左侧的“飞行模式”，来到下图所示页面，分配从左到右第三个拨杆（第五通道，channel 5）为模式切换拨杆，下方的六个飞行模式按图上所示设置，各飞行模式功能在PX4文档中都有详细介绍，这里不再过多叙述。随后，将右侧 “Emergency Kill switch channel” 分配到从左到右第四个拨杆（第十一通道，channel 11）。该拨杆的作用是紧急停止动力，只要拨杆拨至下方，无人机螺旋桨将不再输出动力**（此拨杆优先级最高）**。将右侧 “Offboardl switch channel” 分配到从左到右第二个拨杆（第九通道，channel 9）。该拨杆的作用是手动进行offboard模式的切换，正常情况下该拨杆应一直处于拨上位置。

![2024-11-23 23-04-08 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751575.png)

第四步，进行电机测试及方向校正。**在此阶段，请务必确保飞机上的螺旋桨已经被取下。**对于我们的四旋翼无人机来说，我们对电机的控制采用PWM波的形式，通过飞控向电调发送控制指令，电调向电机提供三相PWM电压来进行控制，因此，电机转向既可以通过硬件方式（重新焊接）来解决，也可以通过软件修改来解决。首先确保螺旋桨已经卸下且电调与电机的三相线，电调与飞控的通信线连接良好，随后点击QGC左侧的“电机”，来到下图所示页面，将最下面的小滑块拉至右侧高亮，此时就会看到上面五个滑动条均可以拉动。通过拉动每个编号的滑动条，可以让电机转动，从而检测电机是否能够正常转动且转动方向是否正确。若电机转向不正确，利用下面的语句，即可完成转向：点击左上角QGC标志+"Analyze Tools"进入分析工具界面，点击“Mavlink控制台“，输入：

```bash
dshot reverse -m 2 #2代表2号电机，可根据不同电机将2改为1或其他数字
dshot save -m 2
```

即可完成电机转向。

![2024-11-23 23-23-58 的屏幕截图](http://cdn.jsdelivr.net/gh/Yuzhe-Y/Markdown-Images@main/images/202411241751576.png)

**！！！注意！！！**

**无人机电机顺序为（飞控正方向朝向）：1号电机：右上电机，逆转（CCW）；2号电机：左下电机，逆转（CCW）；3号电机：左上电机，正转（CW）；4号电机：右下电机，正转（CW）。**

**无人机螺旋桨顺序为：1号电机、2号电机：正桨（标记为P或CCW，逆时针旋转）；3号电机、4号电机：反桨（标记为R或CW，顺时针旋转）。**

第五步，更改参数。我们需要对以下的参数进行修改。

点击左侧“参数”，在搜索栏中搜索“dshot”，随后点击“DSHOT_CONFIG”，将“DISABLE”改为“Dshot600”。搜索“sys”，随后点击“SYS_USE_IO”，将"IO enable(RC & PWM)"改为"IO disable(RC only)"。搜索“aid”，点击“EKF2_AID_MASK”，将“1”改为“24”，下方只勾选“vision position fusion”和“vision yaw fusion”。搜索“hgt”，点击“EKF2_HGT_MODE”，将选项改为“Vision”。搜索“delay”，你将会看到许多形式为“EKF2\_...\_DELAY”的参数（总计应为8个），将“EKF2_EV_DELAY”改为“50.0ms”，其余均改为“0.0ms”。

### 3.3 遥控器设置

当我们设置完QGC之后，我们将会对遥控器进行最终设定。

我们使用的是 乐迪 AT9S PRO 遥控器。针对我们使用的四旋翼无人机，我们需要进行以下设定：长按遥控器下方“MODE”按键进入到遥控器设定界面，拨动右侧圆盘找到“机型选择”，点击右侧“确定”后找到”机型“选项，选择“多旋翼模型”。随后返回到设置界面，找到“功能设置”，进入后分别将“通道选择”改为“12CH”以及将“OUT”改为“SBUS”，即完成所有遥控器的设置。

### 3.4 飞控固件烧录与更改传感器通信频率

本部分内容主要针对飞控与上位机通信频率的修改以及烧录飞控固件的有关操作。

如果我们没有修改过飞控固件中的任何参数，Holybro Pixhawk 6C 将会以60hz的频率进行imu数据的采样，根据采样定理，能够完整复现imu数据且进行卡尔曼滤波处理的最大工作频率为30hz。根据采样定理，此时我们整个状态机及其他利用到这些数据的程序，其最大工作频率将只有15hz，因此，状态机中nmpc进行控制的程序，其最大工作频率将只有15hz，这对于我们来说是不可接受的。因此，我们需要修改通信频率，使通信频率达到100hz，这样我们的状态机将会以50hz的频率进行运行，imu的采样频率也将会来到最高200hz（准确来说，Holybro Pixhawk 6C 上的imu最高采样频率为190hz）

首先进行固件下载，打开下面的网址：[PX4_v1.13.3_固件](https://github.com/PX4/PX4-Autopilot/releases/tag/v1.13.3) 或 [PX4固件github库](https://github.com/PX4/PX4-Autopilot)，将固件克隆到本地。

```bash
git clone -b v1.13.3 --recursive https://github.com/PX4/PX4-Autopilot.git  #也可以是其他v1.13版本，语句可能有不同
```

随后直接在文件夹中打开 mavlink_main.cpp，进行全局搜索，搜索 “30.0f”，你将会看到总共有8个搜索项，全部与 “LOCAL_POSITION_NED” 和 “ODOMETRY” 有关。将8个所有的 “30.0f” 改为 “100.0f” 并保存。

保存后进行程序编译。我们使用的飞控是 Holybro Pixhawk 6C，因此我们采用如下语句进行编译并进行烧录：

```bash
make px4_fmu-v6c_default #编译文件
make px4_fmu-v6c_default upload #烧录固件
```

此外，完成编译后我们也可以在QGC中进行固件烧录，点击左侧“固件”，重新插拔飞控与电脑的USB，随后就会产生提示框“固件设置”，点击下方的“高级设置”，选择“自定义固件文件”并点击上方的确定，即可自主选择要烧录的固件。找到编译好的 .px4 文件并进行烧录即可。

下方也将列出一些有关烧录方面的文章供大家参考：

[PX4开源飞控--开发环境搭建 编译仿真及烧录](https://blog.csdn.net/zhoubiaodi/article/details/138303475)

### 3.5 飞行日志读取与分析

飞行日志是我们了解无人机飞行状况的一大利器，通过阅读飞行日志，我们能够了解到无人机整个飞行过程中的状态以及存在的问题。

点击左上角QGC标志+"Analyze Tools"进入分析工具界面，出现的就是“日志下载”界面。点击右侧的“刷新”，即可看到全部的飞行日志。由于飞行日志上显示的时间并不是我们现在的时间，我们可以通过时间顺序来查看最近的日志，或者可以点击“擦除全部”，从而记录下一次的飞行日志。如果想要下载飞行日志，则点击要下载的日志，随后点击右侧的“下载”，即可将日志下载至本地。

我们可以利用该网站进行飞行日志查看[PX4 Flight Review](https://logs.px4.io/)，将下载的飞行日志上传，并填写油箱（随便一个），点击“upload”，即可完成飞行日志上传。

在这里我同时介绍一下悬停推力测量的方法：利用mavros中提供的位置控制指令，固定其飞行位置，并记录本次飞行日志，进行查看后直接拉至含有“Thrust”的图表，观察其悬停推力即可。

## 4. 代码讲解

本部分为本次比赛采用代码的对应原理讲解部分。本次比赛代码总共分为路径规划，运动控制与状态机，SLAM（同步定位与建图），视觉识别四部分，分别由：左远航（路径规划），杨宇哲（运动控制与状态机，SLAM），杨正南（视觉识别）三位同学负责。有任何有关代码上的问题请与这三位学长进行联系。此外，代码的构建还离不开实验室各位师兄师姐的贡献，是他们为我们提供了本次比赛的代码基础平台，大大减少了我们的工作量。如果存在我们无法解释的问题，还可以联系下列学长进行解答：路径规划：吴德龙学长，运动控制与状态机：华骏扬学长，施扬熹学长，SLAM：葛奕谷学长、杨淏宇学长，视觉识别：李昀皞同学。

### 4.1.1 Ego Planner原理

Ego Planner最大的优点是不需要ESDF地图，在轨迹优化时不需要使用ESDF去构建避障的cost项，ESDF是一种栅格距离场，每个格子都存放着距离该格子最近的障碍物的距离，从下图右下角的柱状图可以看出EWOK、Fast-Planner算法都需要较长的时间去构建这个ESDF地图，如果不构建ESDF可以使得规划时间更短，如Ego Planner算法

![image](https://github.com/user-attachments/assets/fb768165-e0f4-41c3-9393-58bfe4b6c37e)

除了构建ESDF地图需要耗费较长时间之外，在建图的时候只能看见障碍物的表面，如下图中的右图所示，看不见障碍物的后面/内部，在优化的过程中会使用ESDF去产生一个排斥轨迹远离障碍物，在下图所示的例子中，上面的障碍物将轨迹往下推，下面的障碍物将轨迹往上推，会出现轨迹卡在障碍物中的情况。所以ESDF存在构建时间长、对避障cost构建的缺点等问题
![image](https://github.com/user-attachments/assets/4c957b37-2f60-4bec-b0d7-6b6c26066cec)

在不使用ESDF地图的前提下，Ego Planner是如何实现将轨迹推到无障碍物区域呢？其核心思想是，轨迹在优化的过程中，如果轨迹与障碍物发生碰撞，我们就基于这个碰撞对轨迹产生一个反方向的力来把轨迹推出来，远离刚刚碰到的障碍物

![d698c7e1b3dfa94386f56d238f7e3592](https://github.com/user-attachments/assets/1eb72b01-82ef-4b74-947b-22a34b8ababe)

前端：Fast Planner的前端使用的是Hybrid A * 算法，而Ego Planner并不需要一个与障碍物不发生碰撞的初值轨迹，所以它的前端不需要Hybrid A * 这种比较复杂的算法，它只需要一条不考虑障碍物，满足给定起点和目标点状态的轨迹即可，然后在后端优化中再把轨迹优化成无碰撞的

   后端轨迹优化：相比于Fast Planner，Ego Planner在后端优化中在每次迭代中对优化后的轨迹需要增加检查轨迹是否发送碰撞的流程，如下图的check collision所示，如果没有发生碰撞，则继续迭代优化轨迹，如果发生了碰撞，就需要针对这个碰撞，加一个新的推力的cost，此时优化问题发生了变化，不能再针对原问题继续优化下去，需要针对新问题重新开始优化。即基于当前优化的结果再加上新的cost，再去进行优化。

   后端时间重分配：由于Ego Planner的后端优化也是基于优化的控制点，采用均匀B样条来生成轨迹，所以也需要跟Fast Planner一样，进行时间重分配，所不同的是Ego Planner的时间重分配与Fast Planner的时间重分配所使用的方法思路是完全不一样的。
接下来，我们来看一下轨迹与障碍物发生碰撞后，与碰撞反方向的推力是如何产生的

   首先，我们要检查轨迹上的所有控制点，找出所有在障碍物里面的控制点，以及与该位于障碍物内部的控制点邻近的控制点，如下图中的左图所示，然后我们需要以这两个不在障碍物内部的邻近的控制点为起点和终点使用传统的A * 算法 搜索得到两点之间的一条可行路径，如下图中中间图的蓝色路径所示，然后计算在障碍物内部的点Q的速度方向，然后以这个速度方向为法向量，找到一个垂直于这个速度方向且过点Q的平面，如下图中右图的绿色平面所示，然后找这个平面与障碍物表面的交点，如下图中右图的p点所示，然后取向量v为由Q点指向p点方向上的单位向量，即
   
   ![image](https://github.com/user-attachments/assets/4843a6ab-7d32-4b63-9339-eba9b41bf99d)

 这样就可以得到一个针对Q点的{p，v}对
 
![image](https://github.com/user-attachments/assets/f276c240-f964-4905-8354-83835580bb39)

![image](https://github.com/user-attachments/assets/07d68a6c-07ec-4f68-9036-22c7c6b6493f)

 Fast-Planner将轨迹参数化为非均匀B样条，检查井将超过速度、加速度限制的控制点所影响的时间区间采用迭代的方式进行扩大。·如果在轨迹初状态的周围调整时间，会导致轨迹的初状态的高阶量发生变化，从而导致轨迹在飞行的时候会有一些抖动。在Ego Planner中，会根据安全轨迹重新生成合理时间分配的均匀B样条轨迹。它的目标是使得时间分配更合理的B样条在位置上更加贴合之前优化出来的轨迹，并尽可能的满足动力学要求和平滑要求。
 
 ![image](https://github.com/user-attachments/assets/d70a1530-ea28-4601-8475-c3e8ed859f58)
 
 最后，再来总结一下Ego Planner框架的流程

   首先，我们可以通过非常简单天真的方法（不考虑障碍物），得到一条比较光滑的从起点到目标点的B样条曲线的初值，然后使用均匀的B样条去做轨迹优化，轨迹优化过程中，如果发现控制点碰到障碍物，就将该碰撞产生的新cost项，加入到优化的目标函数中，重新进行轨迹优化，直至收敛得到无碰撞的满足需求的解，然后再进行时间的重分配，依然使用均匀B样条，先计算出新的时间间隔△t，然后用最小二乘计算出一个新轨迹控制点初值，然后进行优化，使得新轨迹在新的时间与原轨迹在对应旧时间的位置上尽可能贴合，并满足光滑，动力学约束等，这个过程更像是一个曲线拟合问题，最终得到我们需要的轨迹。
   1、各功能包作用

   Ego Planner的开源代码中，uav_simulator文件夹中存放的是一些仿真相关的程序，planner文件夹中存放的是Ego Planner算法相关的程序。

   在planner文件夹中包含多个功能包，各功能包的作用如下：

   （1）bspline_opt ：均匀B样条，用于轨迹优化

   （2）path_searching：dynamic A * 算法，但在Ego Planner的规划过程中并没有用到

   （3）plan_env：用于地图生成

   （4）plan_manage：主文件夹，主节点所在文件夹

   （5）traj_utils：可视化及多项式轨迹类的相关内容
 2、Ego Planner程序框架/流程

   我们从主函数所在的ego_planner_node.cpp文件开始，该文件中初始化Ego Planner的节点以后，通过rebo_replan.init函数,来调用Ego Planner算法相关的程序，rebo_replan.init函数的具体程序在ego_replan_fsm.cpp文件中
```
rebo_replan.init(nh);

void EGOReplanFSM::init(ros::NodeHandle &nh)
```
   ego_replan_fsm.cpp文件中execFSMCallback函数是状态基，用于处理在机器人运行过程中遇到各种情况下，是选择急停、还是重新规划、还是继续执行等。

   我们前面介绍的Ego Planner算法的具体程序都在ego_replan_fsm.cpp 文件中 callReboundReplan函数中调用的reboundReplan函数中。
```
 bool EGOReplanFSM::callReboundReplan(bool flag_use_poly_init, bool flag_randomPolyTraj)

 bool plan_success =
        planner_manager_->reboundReplan(start_pt_, start_vel_, start_acc_, local_target_pt_, local_target_vel_, (have_new_target_ || flag_use_poly_init), flag_randomPolyTraj);


   reboundReplan函数的具体程序在planner_manager.cpp文件中

bool EGOPlannerManager::reboundReplan(Eigen::Vector3d start_pt, Eigen::Vector3d start_vel,
                                        Eigen::Vector3d start_acc, Eigen::Vector3d local_target_pt,
                                        Eigen::Vector3d local_target_vel, bool flag_polyInit, bool flag_randomPolyTraj)

```

   接下来，我们就顺着reboundReplan函数来看一下Ego Planner算法的流程

   首先是不考虑障碍物的情况下，生成从起点到目标点的光滑轨迹，如果之前已经有轨迹了，则初始轨迹就基于之前的轨迹来生成，这样保证在重规划时不会出现一会儿让无人机往左飞，一会儿让无人机往右飞，在几条轨迹间来回横跳的现象。
```
  double ts = (start_pt - local_target_pt).norm() > 0.1 ? pp_.ctrl_pt_dist / pp_.max_vel_ * 1.2 : pp_.ctrl_pt_dist / pp_.max_vel_ * 5; // pp_.ctrl_pt_dist / pp_.max_vel_ is too tense, and will surely exceed the acc/vel limits
    vector<Eigen::Vector3d> point_set, start_end_derivatives;
    static bool flag_first_call = true, flag_force_polynomial = false;
    bool flag_regenerate = false;
    do
    {
      point_set.clear();
      start_end_derivatives.clear();
      flag_regenerate = false;

      if (flag_first_call || flag_polyInit || flag_force_polynomial /*|| ( start_pt - local_target_pt ).norm() < 1.0*/) // Initial path generated from a min-snap traj by order.
      {

		 ...略...

       }
       else // Initial path generated from previous trajectory.
      {
      
		 ...略...

      }
     } while (flag_regenerate);
```
   上面生成的轨迹并不是B样条轨迹，接下来需要将其变成B样条轨迹（通过求解最小二乘问题获得B样条的控制带你来进行转换），该部分内容是通过调用parameterizeToBspline函数完成的，该函数的具体程序位于uniform_bspline.cpp文件中，该函数的输入参数中point_set是轨迹上的点，start_end_derivatives是起点和终点的高阶约束
```
UniformBspline::parameterizeToBspline(ts, point_set, start_end_derivatives, ctrl_pts);

  void UniformBspline::parameterizeToBspline(const double &ts, const vector<Eigen::Vector3d> &point_set,
                                             const vector<Eigen::Vector3d> &start_end_derivative,
                                             Eigen::MatrixXd &ctrl_pts)

```

   转换完成后，我们就得到了后端优化的初值，然后执行轨迹优化，该部分内容是通过调用BsplineOptimizeTrajRebound函数来完成的，该函数的具体程序在bspline_optimizer.cpp中
```
 /*** STEP 2: OPTIMIZE ***/
 
    bool flag_step_1_success = bspline_optimizer_rebound_->BsplineOptimizeTrajRebound(ctrl_pts, ts);


  bool BsplineOptimizer::BsplineOptimizeTrajRebound(Eigen::MatrixXd &optimal_points, double ts)
```
   优化结束后，开始执行时间重分配，该部分内容通过调用refineTrajAlgo函数来完成，该函数的具体程序位于planner_manager.cpp文件中
```
flag_step_2_success = refineTrajAlgo(pos, start_end_derivatives, ratio, ts, optimal_control_points);

bool EGOPlannerManager::refineTrajAlgo(UniformBspline &traj, vector<Eigen::Vector3d> &start_end_derivative, double ratio, double &ts, Eigen::MatrixXd &optimal_control_points)
```
   时间重分配流程结束后，就得到了最终的轨迹，将其发布出去

   3、重要函数的展开介绍（BsplineOptimizeTrajRebound和refineTrajAlgo）

   我们先来看，完成轨迹优化的BsplineOptimizeTrajRebound函数

   该函数的具体程序在bspline_optimizer.cpp中，首先将时间间隔ts传给B样条曲线，然后调用rebound_optimize函数执行轨迹优化
```
  bool BsplineOptimizer::BsplineOptimizeTrajRebound(Eigen::MatrixXd &optimal_points, double ts)
  {
    setBsplineInterval(ts);

    bool flag_success = rebound_optimize();

    optimal_points = cps_.points;

    return flag_success;
  }
```
   在rebound_optimize函数中，调用了LBFGS优化器进行优化
```
 int result = lbfgs::lbfgs_optimize(variable_num_, q, &final_cost, BsplineOptimizer::costFunctionRebound, NULL, BsplineOptimizer::earlyExit, this, &lbfgs_params);
```
   其核心是目标函数costFunctionRebound如何编写
```
  double BsplineOptimizer::costFunctionRebound(void *func_data, const double *x, double *grad, const int n)
  {
    BsplineOptimizer *opt = reinterpret_cast<BsplineOptimizer *>(func_data);

    double cost;
    opt->combineCostRebound(x, grad, cost, n);

    opt->iter_num_ += 1;
    return cost;
  }
```
   目标函数的具体表达式在costFunctionRebound函数调用的combineCostRebound函数中，该函数中又调用了calcSmoothnessCost、calcDistanceCostRebound、calcFeasibilityCost这三个函数，分别对应着目标函数中的平滑项、避障约束项、动力学约束项，其中calcDistanceCostRebound在计算避障约束项之前会先检测是否碰撞了障碍物，若是则会停止优化
```
 void BsplineOptimizer::combineCostRebound(const double *x, double *grad, double &f_combine, const int n)
  {

    memcpy(cps_.points.data() + 3 * order_, x, n * sizeof(x[0]));

    /* ---------- evaluate cost and gradient ---------- */
    double f_smoothness, f_distance, f_feasibility;

    Eigen::MatrixXd g_smoothness = Eigen::MatrixXd::Zero(3, cps_.size);
    Eigen::MatrixXd g_distance = Eigen::MatrixXd::Zero(3, cps_.size);
    Eigen::MatrixXd g_feasibility = Eigen::MatrixXd::Zero(3, cps_.size);

    calcSmoothnessCost(cps_.points, f_smoothness, g_smoothness);
    calcDistanceCostRebound(cps_.points, f_distance, g_distance, iter_num_, f_smoothness);
    calcFeasibilityCost(cps_.points, f_feasibility, g_feasibility);

    f_combine = lambda1_ * f_smoothness + new_lambda2_ * f_distance + lambda3_ * f_feasibility;
    //printf("origin %f %f %f %f\n", f_smoothness, f_distance, f_feasibility, f_combine);

    Eigen::MatrixXd grad_3D = lambda1_ * g_smoothness + new_lambda2_ * g_distance + lambda3_ * g_feasibility;
    memcpy(grad, grad_3D.data() + 3 * order_, n * sizeof(grad[0]));
  }
```
   我们再来看一下，用于时间重分配的refineTrajAlgo函数

   首先要计算出新的合适的时间间隔，该部分内容在reparamBspline函数中完成，然后基于新的时间间隔计算新轨迹的初值，该部分内容主要在UniformBspline函数中完成，然后，再对新的轨迹进行优化，该部分内容在BsplineOptimizeTrajRefine中完成
```
bool EGOPlannerManager::refineTrajAlgo(UniformBspline &traj, vector<Eigen::Vector3d> &start_end_derivative, double ratio, double &ts, Eigen::MatrixXd &optimal_control_points)
  {
    double t_inc;

    Eigen::MatrixXd ctrl_pts; // = traj.getControlPoint()

    // std::cout << "ratio: " << ratio << std::endl;
    reparamBspline(traj, start_end_derivative, ratio, ctrl_pts, ts, t_inc);

    traj = UniformBspline(ctrl_pts, 3, ts);

    double t_step = traj.getTimeSum() / (ctrl_pts.cols() - 3);
    bspline_optimizer_rebound_->ref_pts_.clear();
    for (double t = 0; t < traj.getTimeSum() + 1e-4; t += t_step)
      bspline_optimizer_rebound_->ref_pts_.push_back(traj.evaluateDeBoorT(t));

    bool success = bspline_optimizer_rebound_->BsplineOptimizeTrajRefine(ctrl_pts, ts, optimal_control_points);

    return success;
  }
```
   在BsplineOptimizeTrajRefine函数中，调用了refine_optimize函数来执行具体的优化过程。
```
  bool BsplineOptimizer::BsplineOptimizeTrajRefine(const Eigen::MatrixXd &init_points, const double ts, Eigen::MatrixXd &optimal_points)
  {

    setControlPoints(init_points);
    setBsplineInterval(ts);

    bool flag_success = refine_optimize();

    optimal_points = cps_.points;

    return flag_success;
  }
```
   refine_optimize函数中同样是调用了LBFGS求解器来就行优化过程，其核心依然是目标函数costFunctionRefine的给定

```  bool BsplineOptimizer::refine_optimize()

int result = lbfgs::lbfgs_optimize(variable_num_, q, &final_cost, BsplineOptimizer::costFunctionRefine, NULL, NULL, this, &lbfgs_params);

   costFunctionRefine函数中调用了combineCostRefine函数来计算目标函数值

  double BsplineOptimizer::costFunctionRefine(void *func_data, const double *x, double *grad, const int n)
  {
    BsplineOptimizer *opt = reinterpret_cast<BsplineOptimizer *>(func_data);

    double cost;
    opt->combineCostRefine(x, grad, cost, n);

    opt->iter_num_ += 1;
    return cost;
  }
```
   combineCostRefine函数中又调用了calcSmoothnessCost函数、calcFitnessCost函数、calcFeasibilityCost函数来分别计算平滑项、曲线拟合项、动力学约束项

```  void BsplineOptimizer::combineCostRefine(const double *x, double *grad, double &f_combine, const int n)
  {

    memcpy(cps_.points.data() + 3 * order_, x, n * sizeof(x[0]));

    /* ---------- evaluate cost and gradient ---------- */
    double f_smoothness, f_fitness, f_feasibility;

    Eigen::MatrixXd g_smoothness = Eigen::MatrixXd::Zero(3, cps_.points.cols());
    Eigen::MatrixXd g_fitness = Eigen::MatrixXd::Zero(3, cps_.points.cols());
    Eigen::MatrixXd g_feasibility = Eigen::MatrixXd::Zero(3, cps_.points.cols());

    //time_satrt = ros::Time::now();

    calcSmoothnessCost(cps_.points, f_smoothness, g_smoothness);
    calcFitnessCost(cps_.points, f_fitness, g_fitness);
    calcFeasibilityCost(cps_.points, f_feasibility, g_feasibility);

    /* ---------- convert to solver format...---------- */
    f_combine = lambda1_ * f_smoothness + lambda4_ * f_fitness + lambda3_ * f_feasibility;
    // printf("origin %f %f %f %f\n", f_smoothness, f_fitness, f_feasibility, f_combine);

    Eigen::MatrixXd grad_3D = lambda1_ * g_smoothness + lambda4_ * g_fitness + lambda3_ * g_feasibility;
    memcpy(grad, grad_3D.data() + 3 * order_, n * sizeof(grad[0]));
}
```
版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。                     
原文链接：https://blog.csdn.net/qq_44339029/article/details/132641867                   
原文链接：https://blog.csdn.net/qq_44339029/article/details/132655058

以上内容基本转载两篇Ego Planner规划算法（上）与（下）——Ego Planner程序框架，讲解代码逻辑清晰，在比赛中我们不需要了解Ego Planner具体代码细节，从原理上入手了解整个规划的过程即可。

在无人机比赛使用Ego Planner规划的时候，需要注意以下参数文件，进行相应的修改。

![image](https://github.com/user-attachments/assets/323b2eb0-49b6-4bf4-a863-ac76335c83a7)

![image](https://github.com/user-attachments/assets/15c6b26e-6e4c-4050-9251-b93ccbf3bdde)

此处虚拟天花板高度一般设置为洞穴比赛要求的2-3m，因为在实际飞行过程中无人机高度控制往往出现误差，相当于设置一个合适的限幅值，在仿真中会遇到类似的问题，由于虚拟天花板设置不得当（比如特别小，小于1），而实际飞行高度在仿真器中大于1m，会出现推离异常的情况，无法完成仿真规划。

![image](https://github.com/user-attachments/assets/0da2b050-c12e-49aa-bcd9-336cbc07287c)

在run_real.xml文件中设置最大规划出的运行速度的参数，在洞穴比赛中，不用实现高机动规划，可以适当设置慢速前进，进行充分的探索
1-1.5m为max_vel即可，其他两个参数不用动。

### 4.1.2 Ego Planner仿真

官方仿真：https://github.com/ZJU-FAST-Lab/EGO-Planner-v2.git

洞穴比赛搭建的仿真：这里我们手动搭建一个小仿真平台，Ego Planner仿真需要与状态及配合，single_offboard_fsm节点作为与轨迹规划通信的对象，可以在rviz中验证规划的有效性，规律以及状态机逻辑的有效性和规律。

我们需要启动以下几个节点
```
roscore
python3 obs.py
```
上述代码运行障碍物生成文件，通过人为生成类型为Pointcloud2的障碍物类型，充当避障的障碍物。
obs.py文件如下
```
#!/usr/bin/env python

import rospy
import std_msgs.msg
import sensor_msgs.msg
import numpy as np
def create_tunnel_and_obstacles():
    # 隧道的参数
    tunnel_length = 100.0  # 隧道长度
    tunnel_width = 10.0    # 隧道宽度
    tunnel_height = 5.0    # 隧道高度

    # 隧道表面点的计算：上下表面每5米一个点，左右墙面是连续的
    tunnel_points = []
    
    # 生成隧道的上、下表面和左右边界
    for i in range(0, int(tunnel_length) + 1, 5):  # 每5米一个点
        # 下边界
        tunnel_points.append((i, -tunnel_width / 2, 0))  # 左下角
        tunnel_points.append((i, tunnel_width / 2, 0))   # 右下角

        # 上边界
        tunnel_points.append((i, -tunnel_width / 2, tunnel_height))  # 左上角
        tunnel_points.append((i, tunnel_width / 2, tunnel_height))   # 右上角

    # 生成连续的左右侧墙面（在x轴方向密集）
    for i in np.linspace(0, tunnel_length+10, 1000):  # 沿x轴方向更密集生成点
        # 左侧墙：从地面到顶部
        for h in np.linspace(0, tunnel_height, 10):  # 高度方向均匀分布点
            tunnel_points.append((i, -tunnel_width / 2, h))  # 左墙
    for i in np.linspace(0, tunnel_length, 1000):
        # 右侧墙：从地面到顶部
        for h in np.linspace(0, tunnel_height, 10):
            tunnel_points.append((i, tunnel_width / 2, h))   # 右墙

    # 固定障碍物位置（位于隧道中间）
    obstacle_positions = []
    base_obstacles = [
        (5.0, 1.0),   # 障碍物1
        (10.0, -2.0), # 障碍物2
        (15.0, 0.0),  # 障碍物3
        (20.0, 2.0),  # 障碍物4
        (25.0, -1.5), # 障碍物5
        (30.0, 1.5),  # 障碍物6
        (35.0, -2.5), # 障碍物7
        (40.0, 0.5),  # 障碍物8
        (45.0, -1.0), # 障碍物9
        (50.0, 2.0),  # 障碍物10
        (55.0, -0.5), # 障碍物11
        (60.0, 1.0),  # 障碍物12
        (65.0, -1.2), # 障碍物13
        (70.0, 2.5),  # 障碍物14
        (75.0, 0.0),  # 障碍物15
        (80.0, -2.0), # 障碍物16
        (85.0, 1.5),  # 障碍物17
        (90.0, -1.0), # 障碍物18
        (95.0, 0.0),  # 障碍物19
        (100.0, -1.5),# 障碍物20
    ]
    
    # 为每个障碍物生成从 z=0 到 z=5 的多个点
    for x, y in base_obstacles:
        for z in np.linspace(0, 5.0, 20):  # 生成20个等间隔高度的点
            obstacle_positions.append((x, y, z))

    return np.array(tunnel_points), np.array(obstacle_positions)

def create_tunnel_and_obstacles1():
    # 隧道的参数
    tunnel_length = 100.0  # 隧道长度
    tunnel_width = 10.0    # 隧道宽度
    tunnel_height = 5.0    # 隧道高度

    # 隧道起始偏移量
    tunnel_start_x = 105.0
    tunnel_start_y = 5.0

    # 隧道表面点的计算：左右表面每5米一个点，上下墙面是连续的
    tunnel_points = []
    
    # 生成隧道的左、右表面和上下边界
    for i in range(0, int(tunnel_length) + 1, 5):  # 每5米一个点
        # 左边界
        tunnel_points.append((tunnel_start_x - tunnel_width / 2, tunnel_start_y + i, 0))  # 左下角
        tunnel_points.append((tunnel_start_x + tunnel_width / 2, tunnel_start_y + i, 0))  # 右下角

        # 右边界
        tunnel_points.append((tunnel_start_x - tunnel_width / 2, tunnel_start_y + i, tunnel_height))  # 左上角
        tunnel_points.append((tunnel_start_x + tunnel_width / 2, tunnel_start_y + i, tunnel_height))  # 右上角

    # 生成连续的上下侧墙面（在y轴方向密集） 
    for i in np.linspace(0, tunnel_length, 1000):  # 沿y轴方向更密集生成点
        # 下侧墙：从地面到顶部
        for h in np.linspace(0, tunnel_height, 10):  # 高度方向均匀分布点
            tunnel_points.append((tunnel_start_x - tunnel_width / 2, tunnel_start_y + i, h))  # 左墙
    for i in np.linspace(-10, tunnel_length, 1000):  # 沿y轴方向更密集生成点
        # 上侧墙：从地面到顶部
        for h in np.linspace(0, tunnel_height, 10):
            tunnel_points.append((tunnel_start_x + tunnel_width / 2, tunnel_start_y + i, h))   # 右墙

    # 固定障碍物位置（位于隧道中间）
    obstacle_positions = []
    base_obstacles = [
        (1.0, 5.0),   # 障碍物1
        (-2.0, 10.0), # 障碍物2
        (0.0, 15.0),  # 障碍物3
        (2.0, 20.0),  # 障碍物4
        (-1.5, 25.0), # 障碍物5
        (1.5, 30.0),  # 障碍物6
        (-2.5, 35.0), # 障碍物7
        (0.5, 40.0),  # 障碍物8
        (-1.0, 45.0), # 障碍物9
        (2.0, 50.0),  # 障碍物10
        (-0.5, 55.0), # 障碍物11
        (1.0, 60.0),  # 障碍物12
        (-1.2, 65.0), # 障碍物13
        (2.5, 70.0),  # 障碍物14
        (0.0, 75.0),  # 障碍物15
        (-2.0, 80.0), # 障碍物16
        (1.5, 85.0),  # 障碍物17
        (-1.0, 90.0), # 障碍物18
        (0.0, 95.0),  # 障碍物19
        (-1.5, 100.0),# 障碍物20
    ]
    
    # 为每个障碍物生成从 z=0 到 z=5 的多个点
    for x, y in base_obstacles:
        for z in np.linspace(0, 5.0, 20):  # 生成20个等间隔高度的点
            obstacle_positions.append((tunnel_start_x + x, tunnel_start_y + y, z))

    return np.array(tunnel_points), np.array(obstacle_positions)




def generate_random_points_around(point, num_points=100, range_size=1.0):
    """
    在一个点的周围生成随机点，形成填充。
    :param point: 原始点 (x, y, z)
    :param num_points: 在该点附近生成的随机点数量
    :param range_size: 随机点的范围大小
    :return: 随机点的数组
    """
    x, y, z = point
    random_points = np.random.uniform(-range_size / 2, range_size / 2, (num_points, 3))  # 在立方体范围内随机分布
    random_points += np.array([x, y, z])  # 平移到点的周围
    return random_points

def create_dense_pointcloud(points, num_points_per_point=100, range_size=1.0):
    """
    为每个输入的点生成大量随机点，形成稠密点云。
    :param points: 原始点列表
    :param num_points_per_point: 每个原始点生成的随机点数量
    :param range_size: 每个原始点生成点云的范围
    :return: 稠密点云
    """
    dense_points = []
    for point in points:
        random_points = generate_random_points_around(point, num_points=num_points_per_point, range_size=range_size)
        dense_points.append(random_points)
    return np.vstack(dense_points)

def create_pointcloud_msg(points, frame_id):
    # 创建 PointCloud2 消息
    header = std_msgs.msg.Header()
    header.stamp = rospy.Time.now()
    header.frame_id = frame_id  # 你可以根据需要更改框架ID

    # 将位置转换为 NumPy 数组并打包为 PointCloud2 消息
    points_flat = points.astype(np.float32).flatten()  # 展平数组
    cloud_msg = sensor_msgs.msg.PointCloud2()
    cloud_msg.header = header
    cloud_msg.height = 1
    cloud_msg.width = points.shape[0]
    cloud_msg.fields = [
        sensor_msgs.msg.PointField(name="x", offset=0, datatype=sensor_msgs.msg.PointField.FLOAT32, count=1),
        sensor_msgs.msg.PointField(name="y", offset=4, datatype=sensor_msgs.msg.PointField.FLOAT32, count=1),
        sensor_msgs.msg.PointField(name="z", offset=8, datatype=sensor_msgs.msg.PointField.FLOAT32, count=1),
    ]
    cloud_msg.is_bigendian = False
    cloud_msg.point_step = 12  # 每个点的字节大小
    cloud_msg.row_step = cloud_msg.point_step * points.shape[0]
    cloud_msg.data = points_flat.tobytes()
    cloud_msg.is_dense = True

    return cloud_msg

def obstacle_publisher():
    rospy.init_node('obstacle_publisher', anonymous=True)
    
    # 创建隧道和障碍物
    tunnel_points1, obstacle_positions1 = create_tunnel_and_obstacles1()
    tunnel_points, obstacle_positions = create_tunnel_and_obstacles()
    # 在每个点周围生成稠密点云
    dense_tunnel_points = create_dense_pointcloud(tunnel_points, num_points_per_point=1, range_size=1.0)
    dense_obstacle_points = create_dense_pointcloud(obstacle_positions, num_points_per_point=50, range_size=1.0)

    dense_tunnel_points1 = create_dense_pointcloud(tunnel_points1, num_points_per_point=1, range_size=1.0)
    dense_obstacle_points1 = create_dense_pointcloud(obstacle_positions1, num_points_per_point=50, range_size=1.0)
    # 合并隧道和障碍物的点云
    combined_points = np.vstack((dense_tunnel_points, dense_obstacle_points,dense_tunnel_points1, dense_obstacle_points1))

    pub_cloud = rospy.Publisher('cloud_registered', sensor_msgs.msg.PointCloud2, queue_size=10)
    
    rate = rospy.Rate(100)  # 发布频率 1 Hz

    # 创建合并后的点云消息
    cloud_msg = create_pointcloud_msg(combined_points, frame_id="world")

    while not rospy.is_shutdown():
        # 每次循环中只发布消息，而不重新生成点
        pub_cloud.publish(cloud_msg)

        rospy.loginfo(f"Published tunnel and obstacles with {len(combined_points)} points.")
        rate.sleep()

if __name__ == '__main__':
    try:
        obstacle_publisher()
    except rospy.ROSInterruptException:
        pass
```	
接着启动剩下的节点：
```
source devel/setup.bash
roslaunch fsm_ctrl swarm.launch
```
上述为启动用户指令输入界面，通过socket通信切换指令
```
source devel/setup.bash
roslaunch fsm_ctrl single2.launch 
```
上述同时启动状态机节点和mavros
```
source devel/setup.bash
roslaunch ego_planner happy_fly.launch 
```
上述启动Ego Planner规划器
```
source devel/setup.bash
roslaunch so3_quadrotor_simulator simulator_example.launch 
```
上述启动仿真器rviz

启动节点后我们在rviz中首先订阅PointCloud2的点云类型，这里的点云类型是我们模拟的无人机机载电脑通过网线接收到的来自于Mid——360的点云信息

![image](https://github.com/user-attachments/assets/863aaab0-a6f6-42b9-a57a-0ddf8d90628c)

如上述所示的流程添加点云可视化，轨迹可视化，此时我们发现无人机的位置在0，0，1，这里我们也可以调节simulator_example.launch的参数来实现初始rviz中位置的变化

![1](https://github.com/user-attachments/assets/213e618f-b688-4184-8850-41daac91abde)

具体实现流程如下效果所示

https://github.com/user-attachments/assets/55cd8bfb-0d76-46a6-b050-dcfe4a1d222c

在仿真过程中，我们直接使用自带的so3仿真器，无人机可以视为质点，不具备大小体积，此时调节advanced参数中的障碍物膨胀系数也没有作用，在仿真中该参数不被读入。因此我们可能会遇到几种特殊的情况

![0466ef17dea67fedfd7375b2588060e](https://github.com/user-attachments/assets/184cf3aa-48dd-4570-83d2-6d6bb791fcfb)

暂时没有特别好的解决办法，也会出现偶然报错卡着不动，但是Ego Planner的replan重规划是没有问题的，逻辑是正确的，确保无人机处在障碍物外部，将无人机从障碍物内部退离开，规划的时候以垂直的方向推离开。出现卡顿卡住不动可能因为so3仿真中的无人机没有具体完备的动力学模型，或者电脑无法同时处理那么多replan的新点，问题不大。

关于规划只给一个起点一个终点的情况，这个跟具体地图的尺度有关系，如果地图尺度很大，那么无人机会出现飞行问题，不一样按照最靠前的方向飞行，可能会出现如下图所示的情况：

![23ece35e267911d4f42069886b17ceb](https://github.com/user-attachments/assets/34b42b0a-4913-44bb-9f5b-d200caf46195)

但至少能够保证，飞机能够避障，也在尽力尝试飞向目标点，最终到达目标点。


![e0b00a88d3ec871e78c85ca11d3f9ac](https://github.com/user-attachments/assets/f509432a-8745-4321-a49f-b3b7ec1597f7)

因此，在实际飞行比赛中，我们尽可能的多设置途径点，也就是在拐弯的地方设置拐点，增加拐点的数量，确保飞机以正方向向前探索。

总体效果图如下，此处设置了一个回到原来初始点坐标的规划。

![6569a13c6dbccd467c290aea6c06c03](https://github.com/user-attachments/assets/5c21511d-5786-496e-ab60-d8b632b3c1ac)

值得一提的是，我们的探索方案是需要知道起点和终点的具体点坐标的，我们的逻辑是基于起点和目标终点的路径规划，而不是一个彻头彻尾的未知区域探索方案，先验的坐标信息很关键，如果丧失了先验坐标信息，我们将无法完成自主陌生环境探索。因此后续未来可以尝试将无人机的规划方式向无人车的规划方式靠拢，无人车是全局RRT，局部teb，可以实现在定位飘的情况下仍然按照相对坐标，朝着局部前沿点运行，但是飞机依赖于nmpc控制（或者原始的PX4自带的控制），此控制方法依赖无人机的闭环反馈位置姿态信息，如果没有准确的坐标反馈闭环信息，无人机的控制会出现巨大问题，坐标飘了几米甚至几十厘米就可能引起炸机，为此，有两个可行的方向：

1.优化适合人工长廊的LIO定位方法，我们用的是RA-LIO，也尝试过FAST——LIO2（z轴飘非常严重，完全实现不了闭环控制），但是效果均不是很好。

可以参考香港大学https://github.com/hku-mars/FAST_LIO

2.重新改变规划逻辑,以探索未知区域为核心，但定位漂移仍然无法实现无人机的控制。

可以参考香港科技大学周老师https://github.com/HKUST-Aerial-Robotics/FUEL

所以关键问题在于：如何在洞穴中控制无人机飞行！！！关键在于精准的定位信息！！！因此也回归问题1。

### 4.2 运动控制及状态机部分讲解

状态机部分：https://meeting.tencent.com/crm/2y1gggQ242

涉及到源码部分讲解，请向 杨宇哲同学 申请权限。

### 4.3 SLAM部分讲解

等待补充。

如下图所示，该部分由于Mid-360存在安装角度，一般为14-16°左右，在LaserMapping.cpp文件中的雷达初始化角度修正代码，

每次启动无人机，都会自动修正雷达坐标系和世界坐标系，理论上不加下面的代码的话，需要雷达正向水平朝上安装。

![9ef6a3181a4ecd286573067a5c9a8f6](https://github.com/user-attachments/assets/e28cbcda-1f0b-40b6-98a4-5084d1854a59)


### 4.4 视觉识别部分讲解

等待补充。
