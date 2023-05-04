Thursday, May 4, 2023 @ 11:15:35 AM

算一个静态场，ansoft12保佑

![](2023-05-04-11-34-39.png)

看不到mesh看这里哦meshplot visibility

![](2023-05-04-11-37-29.png)

磁力线flux line在这里哦

赶紧学会处理简单报告

Thursday, May 4, 2023 @ 03:16:52 PM

学一下场计算器

使用wavecoil可以创建线圈模型

控制网格细度的办法：inner和outer都选在气隙的正中心，通过on selection对于气隙进行特别细分

在建模窗口的左下角可以选择瞬态仿真的当前时间用于查看sliding mesh和当前场

![](2023-05-04-17-30-38.png)

搞好这个瞬态仿真，setup的时候就要首先选好stop time，周期比较直观，再确定每一步的时间长度

师兄要求A相调整到起始角，不然会影响backemf的相对相位，这样功率∠计算就会出问题