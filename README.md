**Monday, April 10, 2023 @ 05:04:36 PM**

band和inner outer可以设置transpant方便查看之后的结果

band的设置，右键assign即可，注意选择旋转还是translation，就是暂态

###vector potential就是向量势，或者矢量势，A（x,y,t）就是边界条件使用电流密度不好表示的时候使用这个来表示∇ × A = B

如果在仿真中未使用向量势边界条件，则可能需要更频繁地进行电机中的电磁场采样

矢量磁势边界条件，定义边界上的矢量磁位A的常数值。边界处的磁场与边界正切，不会漏到边界外面去。

modelica实例实战http://modelicabyexample.globalcrown.com.cn/Release: v0.3.6-307-g33331e7-Early Access作者： Michael M. Tiller, Ph.D.

使用了timestamp，使用ctrl+shift+T可以创建时间戳

**Monday, April 10, 2023 @ 05:09:48 PM**

查看maxwell场图不显示的一个可能原因，没有隐藏掉band之类没有用的区域，把field遮住了

添加了markdown快捷键（使用markdown all in one），使用ctrlB和ctrlI可以实现快速斜体粗体

**Monday, April 10, 2023 @ 05:20:07**

使用快捷键：按下Ctrl + L，即可选中光标所在行。

*？加上的负载都是电流（和永磁体），那么为何load不需要与轴相关呢？是只有失去同步之后才会影响？*

玩poly brige有感，使用三角形的稳定性真的能做很多事情

mb3_excitation_assign eddy effect_可以分配涡流损耗，
*一般给永磁体？*

2d仿真别忘了设置轴向长度，mb3>model>assign depth即可

**Monday, April 10, 2023 @ 06:03:17 PM**
##轴向设计方法

插入一个rmxprt design

machine这一栏1.电流类型选择DC2.选择气隙长度，其他不用设置

stator这一栏全部要设置

core这一栏要注意外径给外壳留余量（也全部要设置，材料用steel就行）

slot这一栏根据槽型来看，最好都写，Rs可以不写不报错

winding这一栏注意conductor per slot是匝数乘以绕组层数（双层绕组是2）coil pitch指的是节距，设置1就行，漆包线厚度比较麻烦，需要查表，都是一一对应的，wire wrap指的是绝缘厚度，导线材料选铜

circuit也都填一下

stator依然注意是选择级数（x2）

core按照定子抄过来就行

pole分配的材料可以选NdFe35铝铁鹏，mag len不要选太大容易重合

之后再anali里面add solu，选一下电压转速，保存

右键setup，最下面的create 模型，这个circuit可以去掉如果不需要外电路

**Monday, April 10, 2023 @ 06:08:07 PM**

##双层轴向

click machine and then set fraction to one to get the whole machine.

删除掉多余的模型，选择select all。

右键材料查看property可以得到充磁方向。

这里一个比较方便的设置就是把上下都设为沿着z轴，所以经常会把电机这样摆放。

band设置要比运动部分稍大inner和outer正好包住就行摁住alt和方向可以快速转向

电流面coilterm可以用来加载电流方向和匝数等dependentsheet用来赋予边界条件

用draw里面这个按钮可以隐藏掉暂时不需要的区域（快捷键是f8还是f9，忘记了）

从右下角往左上角选择可以选择所有触碰到的obj，从左上角往右下角选择只能选择完全框住的

使用镜像命令镜像所有上半部分得到一个双定子结构之后使用参数化建模来约束下半个定子使得可以同步修改

在coil当中的createu user define part需要修改参数化外径内径等，之后就不用管了，都是根据coil_0复制过来的

之后在ndfe35（磁钢）里面改一下，diaouter设置为statorouter-2mm即可，单位不可缺失，不然默认是m

铁心也是一样操作，这个值要继承之前的修改

这几个域需要重新定义，需要完全重新设置band和outerregion才能

进行运动仿真

创建一个cylinder，改名为band，之后使用右键-measure-position可以

测量一下数值方便之后设置参数（三维之后感觉比较有用）

复制这个band，改变参数包住所有模型，height使用之前定义的参数加起来

选中这个域，进行split，先使用xz切一边再用yz切一边，可以保留四分之一模型

之后还要修改电流面coilterm和边界条件denpedent都需要在这里选择使用xz平面进行画图可以修改作图面

我怎么感觉这种简单的可以每次都重新画一下更快转到这个平面之后就很简
单了，仅仅画图的话使用around axis命令可以创建出从边界

右键band，assign band进行motion setupinitial角度选择7.5？关注吮指原味的后续视频
再选一个转速

边界选择assign bounders，因为模型不是对称的，（是否只有180deg才能叫做对称？）所以需要选择Hdep=-Hind

##参数化

之后开始设置激励，相对于前两个相对复杂

首先在exitation里面右键已经存在的phase选择add terminal，把属于a的两个激励选择加
进去以此类推
进
rmxprt里面，wind里面右键connect all coil可以得到线圈的连接情况方便理解电流的方
向，结合右手定则可以确定磁钢的NS极

设置一下winding，点击phaseA，改变激励type从external到 Current.

再点击单个线圈的，激励可以看到设置导体数目，其实设置的就是匝数。

点击set up。选择step time。计算一下电周期使用60秒除以转速，再除以极对数。记得是
除，因为之前算电角度的时候，是用机械角度乘以极对数。

**Monday, April 10, 2023 @ 06:13:08 PM**
##参数扫描
optimetric可以add一个参数，设置步长之后就可以进行扫描

HPC和analis这里可以设置多核计算

在中间的add中添加一个新的规则

add machine to list，设置core为4

之后右键parasetup进行仿真

*？？需要总结一下设计流程*

Ctrl/Cmd + Shift + V 快捷键在编辑器中打开 Clipboad Preview，然后直接粘贴图像即可，

Clipboad Preview 会自动将其转换为 Markdown 代码。

![Alt text](pic/%7DGYGG04R$I2O368J_5%25RQQK.png)

![Alt text](pic/@%60~78%60Z$@B%607QMHJ%7B%5B2EOZT.png)

**Monday, April 10, 2023 @ 06:30:47 PM**
##峰值转速仿真
一般先仿真峰值转速之后仿真峰值转矩，再仿真一个额定工作点就行了。

电机能达到某个转速的条件电机的线反电动势峰值小于母线电压并且要给电阻和电感留出余量

线反电动势峰值和转速正比，和直径成正比，和线圈匝数成正比

无刷直流电机的线反电动势峰值小于母线电压(24V)，并且要给电阻和电感留出余量

永磁同步!电机的线反电动势峰值大于母线电压加ld，削弱磁密，实现弱磁升速。

如此看来必须要联合变频器进行仿真才能得到高频下的这个，本来maxwell就可以加外电路，但是感觉还是不如抽出参数

**omega(rad/s)=2xpixf(Hz)
电频率(Hz)=机械频率(HZ)x极对数
机械速度(rpm)=机械频率(HZ)/60**

A:lqrms*sqrt(2)*sin(2*pi*rotor_speed con/60*10*time)I
B相:lqrms*sqrt(2)*sin(2*pi*rotor_speed_con/60*10*time-2*pi/3)
C相: lqrms*sqrt(2)*sin(2*pi*rotor_speed_con/60*10*time+2*pi/3)

**Monday, April 10, 2023 @ 07:05:26 PM**
了解一下磁场调制理论，下载知乎文章

**Monday, April 10, 2023 @ 09:18:29 PM**
使用report>winding>任意一相电压，即可查看反电动势

如果需要查看电感矩阵，需要在model>model depth>matrix compu里面勾选并运算之后到报告中查看，不然默认是空白

**Monday, April 10, 2023 @ 09:52:15 PM**
试试吧SD卡作为miui的主要存储空间，这个红米10也能玩转了