---
title: 使用2N7002的惠斯通电桥再生式(WBR)接收机
author: BH1PHL
---
# 使用2N7002的惠斯通电桥再生式(WBR)接收机
## 基本原理
电路基于N1BYT的WBR接收机，如下图所示。以Q1为核心组成电容三点式振荡电路，RV2和RV5调整是否起振（再生强弱）。L2、C7、C8和6个二极管D1~D6构成振荡槽路。输入信号通过RV1（起可调衰减作用）衰减和L1，馈入L2中间抽头。变容二极管阳极的直流通路由L1提供。

调谐电压由RV3和R10组成的分压器产生。当J4开路时，RV3分出的电压与动触头2位置$a$（取值范围$0\cdots1$，动触头2位于1端时为$0$，位于3端时为$1$）成线性关系($V_\mathrm{out}=V_\mathrm{cc}\cdot\frac{1+10a}{11}$)。由于变容二极管的容量随电压升高降低，并且在高电压时变化较小，故此时RV3动触头在图中下方（多圈电位器开始几圈）时，频率变化较快；动触头在图中上方（多圈电位器后面几圈）时，频率变化较慢。

J4短路时，输出电压和动触头2位置$a$的关系为$V_\mathrm{out}=V_\mathrm{cc}\cdot\frac{1}{1+10(1-a)}$。显然，$a=0$及$a=1$时，输出电压与J4开路时相同，而在中间状态时，变化呈先慢后快趋势。这种趋势比变容二极管的容量变化率随电压的变化趋势更为明显，故此时多圈电位器位于开始几圈时，频率变化较慢；位于后面几圈时，频率变化较快。

原版WBR接收机采用结型场效应管MPF102检波，此处改为MOS型场效应管2N7002。由于2N7002是增强型器件，采用R15、R16、C23组成的分压电路供给栅极偏压。R14的值足够大，因此偏压电路对振荡器的影响极小。Q2工作在$I_\mathrm{d}$非常小的状态，起检波作用。Q3、U2及外围元件构成音频放大器。U1及外围元件构成稳压电源电路（VCC电压9V）。
![](README_md_files/image_20220201140708.png?v=1&type=image&token=V1:KxNJY19yYlJnmXGaiVk6302ukbhrakx--xAjKPXHHUQ)

上图中振荡器和检波器的等效负载电容、C6、L2的两部分构成惠斯通电桥（如下图）。图中L11、L12代表L1的两部分，L21、L22代表L2的两部分，Cl代表振荡器和检波器的等效负载电容，Ct代表槽路电容，V1代表等效输入电压，V2代表等效振荡电压。
![](README_md_files/image_20220207151621.png?v=1&type=image&token=V1:D99lARUeSBibP2hKDFSr3Nd-D1MvnRvMpyVgIzR7Z_g)
图中OSC和INPUT两端电压如下图。
![](README_md_files/image_20220207151923.png?v=1&type=image&token=V1:D7IjLgkvZaCRHHrv10_EXHJPvWckzRIRUhGry9cyV98)
图中INPUT处几乎没有振荡电压（残余量取决于电桥的平衡情况），但从此处馈入的信号仍然能到达图中OSC处（也就是Q1的基极）。从此处馈入信号有如下好处：

1. 天线负载变化对振荡频率几乎没有影响；
2. Q1处于起振状态时，振荡器到天线的泄露极小（一般情况下很容易控制在-70dBm之内）。

电桥平衡时的直流电位由L1下端的接地点提供。RV1对输入信号起衰减作用。

对基极上的外来信号而言，Q1工作在具有正反馈的放大状态，与典型的再生接收机原理相同。在正反馈的作用下，接收的灵敏度和选择性都会得到很大的提高。工作在起振的临界点附近时，接收机的灵敏度和选择性都是最好的。如果Q1工作在刚好起振的状态，还可兼作拍频振荡器，接收CW和SSB信号。

## 工程
工程用KiCad 6.0绘制。使用四层板，尺寸为100mm×50mm。焊装完成后的实物如下图。
![输入图片描述](README_md_files/WBRRecv_assembled_20220207234335.jpg?v=1&type=image&token=V1:rNLiDB9C653gRgXYAHLVgq2XeSW-X2egEXg-FWUL834)

工程文件在[此处](WBRRecv_2N7002_20230227.7z)下载，git 仓库在 https://gitee.com/BH1PHL/WBRRecv_2N7002。

## 元器件选择

* 固定电阻均选用0805 1%贴片电阻。
* F1选用0805封装 0.2A自恢复保险丝（注意应选择耐压指标24V或以上者）。FB1选用0805磁珠（1kΩ@100MHz）。这两个元件的值并不关键。
* 二极管D1～D7选用[ES1JL](https://item.szlcsc.com/139984.html)快恢复二极管（山东晶导微电子），也可试用其他型号、其他厂家产品。注意变容二极管D1~D6采用整流二极管代用，不同型号、厂家的整流二极管反向电容特性不尽相同，并联数量需通过试验确定。
* Q1、Q3选用MMBT3904（SOT-23封装）。
* Q2选用[WST2N7002](https://item.szlcsc.com/88113.html)（WINSOK微硕），也可试用其他厂家产品。
* 固定电容均为多层陶瓷电容（MLCC），10uF电容采用1206封装，其余电容采用0805封装。小于等于1nF的电容采用NP0材质，大于1nF的电容采用X7R/X5R材质。电容选用时注意耐压（使用25V以上耐压的是安全的）。
* 可调电容C8选用常见的6mm塑封微调电容（9～30p，绿色）。注意质量不好的此类电容可能会遇到内部开路情况，建议焊机前先测量，或采用类似尺寸陶瓷外壳微调电容。
* 电位器RV1、RV2、RV4选用与ALPS RK097单联同封装产品；RV3选用多圈电位器（如BOURNS 3590S-2），用2.54mm排针引出。RV5选用与BOURNS 3362P同封装产品。
* U1选用数据手册中明示支持陶瓷电容的SOT-223封装1117可调版，如AZ1117、SPX1117、AP1117等。注意如果使用原版LM1117或最常见的AMS1117，C26可能必须采用钽电容。
* U2可采用LM386或任何兼容产品。
* J1采用夹板式SMA插座，J2采用[5脚直插式耳机插座](https://item.szlcsc.com/300204.html)，J3采用3.5mm脚距插拔式接线端子（[插头](https://item.szlcsc.com/376575.html)、[插座](https://item.szlcsc.com/376577.html)），J4采用2.54mm排针配合短路子。
* L1采用$\mu_\mathrm{i}=300$，OD×ID×h=12.7mm×7.9mm×6.4mm的铁氧体磁环绕制，双线并绕2圈。
* L2采用T50-6铁粉芯磁环绕制。不同波段的圈数（均为双线并绕）、C7、J4的情况及频率范围见下表：

|波段|电感 (T50-6)|C7|J4|频率范围|
|--|--|--|--|--|
|40m|17T×2|47p|短路|约6990~7220kHz|
|30m|15T×2|0p|开路|约9.6~10.2MHz|
|20m|11T×2|0p|开路|约13.5~14.4MHz|

## 调整方法
必备设备：电源、信号发生器、耳机或扬声器
可选设备：衰减器、示波器
步骤：
1. REGEN电位器调整到75%；VOLUME电位器调整到中间；TUNE多圈电位器调整到最小。
2. 连接电源、耳机或扬声器、信号发生器，电源设置到12V。
3. 接入0.1mV左右的信号：信号发生器设置到100mVpp+60dB衰减器，ANT电位器调整到最大；或1mVpp+ANT电位器调整到1/10左右。
4. 打开电源和信号发生器。调整VOLUME电位器至音量舒适。
5. 调整再生：调整PRESET微调电位器，使再生刚好起振（耳机内背景噪声从小突然变大）。注意：振荡过强时耳机内声音也可能很小。
6. 调低端覆盖：信号发生器设置到SWEEP模式，起始频率6.5MHz，终止频率7.5MHz，时间0.2s，调整可调电容C8，使耳机中出现声音。调整信号发生器扫频上下限和可调电容C8，缩小频率范围，使TUNE多圈电位器在最小（接收频率最低）时接收频率在6990kHz左右。
7. 确认高端覆盖和再生控制：信号发生器设置到7.2MHz，调整TUNE多圈电位器直到收到信号，调整REGEN电位器确认再生控制仍然起作用（从低向高调整时有一个背景噪声突然变大、啸叫突然出现的点）。
8. 如果需要，用热熔胶或蜡固封线圈（固封后可能需要微调可调电容C8）。

注：
* 第6步粗调也可改为在T50-6磁环内穿线2～3圈并连接示波器或高阻输入的频率计，在再生出现时观察信号频率并调整可调电容。
* 调整（尤其是微调）时，也可将信号输入端连接带频谱功能的成品接收机（如G90S），用接收机看振荡器到天线泄露信号的频率。

## 使用方法

1. 接入天线、电源、耳机或扬声器。电源电压应在10.5V以上，高压限制由所使用的1117和输入保险丝、滤波电容等元件的耐压决定。**注意电源极性！**

2. ANT电位器、VOLUME电位器调整到中间位置。REGEN电位器调整到刚好起振（背景噪声突然变大）的位置。VOLUME电位器调整至音量舒适。注意：没有起振时有广播干扰是正常现象。起振后若还有广播干扰，可适当左旋ANT电位器增加输入衰减量；若无论有无起振均无广播干扰，说明附近无强干扰广播电台，可适当右旋ANT电位器减少输入衰减量。

3. 调整TUNE电位器选台。一般需要同时调整REGEN电位器，以使再生工作在起振点附近，获得最好的接收效果。注意随着调谐频率升高，再生会变强，如果要保持再生强度不变，应当适当左旋REGEN电位器，反之亦然。

4. 接收CW/SSB电台时，再生调节到**刚好起振**的位置（REGEN电位器放在起振点略偏右侧）效果最好；接收AM电台时，再生调节到**刚好不起振**的位置（REGEN电位器放在起振点略偏左侧）效果最好。

   

----
© BH1PHL 2022~2023
