# The Cross-Coupled Pair

## 文献

> [1]B. Razavi, "The Cross-Coupled Pair - Part I [A Circuit for All Seasons]," in IEEE Solid-State Circuits Magazine, vol. 6, no. 3, pp. 7-10, Summer 2014
> [2]B. Razavi, "The Cross-Coupled Pair - Part II [A Circuit for All Seasons]," in IEEE Solid-State Circuits Magazine, vol. 6, no. 4, pp. 9-12, Fall 2014
> [3]B. Razavi, "The Cross-Coupled Pair?Part III [A Circuit for All Seasons]," in IEEE Solid-State Circuits Magazine, vol. 7, no. 1, pp. 10-13, Winter 2015
> [4]B. Razavi, “A 300-GHz fundamental oscillator in 65-nm CMOS technology,” IEEE J. Solid-State Circuits, vol. 46, no. 4, pp. 8983–9093, Apr. 2011.
> [5]S. Galal and B. Razavi, “40-Gb/s amplifier and ESD protection circuit in 0.18 μm CMOS technology,” IEEE J. Solid-State Circuits, vol. 39, pp. 2389–2396, Dec. 2004.

一个精妙的电路往往能够经受时间的考验。交叉耦合对（XCP）便是这样一种拓扑结构：它已发展了95年，并不断适应各种器件技术、电源电压和工作速度。

## PART Ⅰ

### 小信号特性 Small-Signal Properties

由于交叉耦合对内部存在正反馈，在小信号分析下，可以看作负阻：
![alt text](<Pictures/Cross-Coupled Pair-image.png>)
分析上图的小信号电路，在（a）图的两个漏端之间或（b）图的两个源端之间跨接一个电流源进行分析（不考虑$g_{ds}$），可以求得：
$$\begin{cases}
    Z_{\mathrm{inl}}=-Z_{1}-2/g_{m}\\
    Z_{\mathrm{in2}}=-Z_{2}+2/g_{m}
\end{cases}$$

对于$Z_{in1}$的应用很有趣：
+ 首先当$Z_1=0\rightarrow Z_{in1}=-2/g_m$的时候，电路的应用有很多，在振荡器、放大器、比较器中都能看到他的身影
+ 对于$Z_{in1}=-2/g_m$，当考虑到所有的实际电容时，这个值依旧不会变，但是当引入栅极电阻时，这个值会退化为：$[-g_m/2+R_GC_{GS}\omega^2/2]^{-1}$，可参考文献[4]
+ 当$Z_1$是电容的时候，$Z_{in1}$呈容性，可以看成是一个负电容，能够“中和”MOS管栅极的电容
+ 当$Z_1=0$的时候，输入端等效的噪声电压为：$8kT\gamma/g_m$

### 大信号特性 Large-Signal Properties

XCP可以作为双稳态元件工作，具有零静态功耗，这是一种在存储器和数字电路中得到利用的通用特性。

例如，在考虑下图(a)所示的电阻负载差分缓冲器，即使在输入为轨到轨时，它也会消耗静态电流：假设$V_{in1}=V_{DD},V_{in2}=0$，那么在极端假设下有$V_X=0,V_Y=V_{DD}$，这个电路左侧通路始终是存在的，因此会有静态功耗
而在（b）中，假设是同样的条件，$V_{in1}$将X结点的电压下拉，导致Y结点电压被上拉，从而削弱了$M_3$管的上拉作用，这个正反馈最终促进了X结点的上拉和Y结点的下拉，最终X结点和Y结点的电压会与（a）中截然相反，但由于$M_1$和$M_3$及$M_2$和$M_4$不能同时导通，因此不存在静态功耗。
这种结构还可用在dynamic RS Latch中
![alt text](<Pictures/Cross-Coupled Pair-image-1.png>)

### 滞后与放大 Hysteresis Versus Amplification

上图(b)中的双稳态会对在电路的输入-输出特性中产生迟滞现象。例如当$M_1$管在X结点电压为高电平的时候开启，$V_{in1}$必须上升到足够电压使得$M_1$的下拉强于$M_3$的上拉才能使得环路的稳态特性发生改变，这个过程叫做再生（regeneration），直到$M_3$关闭$M_1$进入深线性区为止。
因此由于电路中存在滞后现象，只有较大的输入摆幅才能改变状态。

如果想让再生过程发生在我们需要的时刻，那么我们可以通过引入一个时钟CK解决，如下图所示：
![alt text](<Pictures/Cross-Coupled Pair-image-2.png>)
X和Y结点电压的变化速率符合：$$V_{XY}(t)=V_{XY0}\exp\frac{t}{\tau_{\mathrm{reg}}}$$
其中再生时间常数：$\tau_{\mathrm{reg}}=R_{L}C_{L}/(g_{m}R_{L}-1)$，这是根据小信号求解出来的

### 留给读者的问题 Questions for the Reader

1. 负电容与正电感相同吗？
   1. 不同，因为前者的阻抗由$j/\omega C$给出，其幅值随频率降低，而后者的阻抗$j\omega L$的幅值随频率升高。
2. 正电容与负电容的抵消是否可能是一种谐振效应？
   1. 并不。在谐振器中，电压和电流之间的相位差在谐振频率处改变符号。正电容和负电容的串联或并联组合并不表现出这种特性。
3. 为什么图4(b)中的电路是动态锁存器？
   1. 如果用作静态的RS Latch，当输入电压全为低的时候，$M_1,M_2$的漏端漏电流会导致X结点和Y结点存储的电压发生变化，影响存储结果
4. 如图所示，我们在X结点和Y结点跨接一个电流源，当$t=0$的时候，电流源的电流从0突然跳变到一个正值（跳变比较小），我们的直觉会认为，这个时候X结点电压上升，Y结点电压下降；但是从负阻抗的角度来看，X结点的电压应该是下降，而Y结点的电压是上升的，如何解释这个不同？——直觉是基于我们认为每个节点对地都存在一定的电容，但直觉是错的，小信号的分析是对的：![alt text](<Pictures/Cross-Coupled Pair-image-3.png>)

## PART Ⅱ

在这一部分中我们将讨论XCP在数字电路中的应用。在数字信号电路中，如果在互补（差分）信号之间连接一个XCP，数字电路的性能可以得到提升。具体来说，我们可以添加一个时钟控制的XCP，或者用XCP替换互补逻辑中的PMOS器件

### 灵敏放大器 Sense Amplifiers

在数字（或模拟）设计中，一种常见情况是两个差分节点之间出现的微小初始不平衡$V_{XY0}$，并且需要被尽可能快地放大为（最好是）轨到轨互补信号。

可以将该电路设计为：两个节点由高阻抗驱动，并以“自然”时间常数向电源轨移动，如下图（a）所示
![alt text](<Pictures/Cross-Coupled Pair-image-4.png>)
最终当稳定时，可以认为：$g_{m1,2}R_{L}\mid V_{\mathrm{inl}}-V_{\mathrm{in2}}\mid\approx V_{\mathrm{DD}}$，而需要的时间为$t_1$
我们设定增益$G=V_{\mathrm{XY}}(t_{1})/V_{\mathrm{XY0}}$，于是我们可以认为在将X结点和Y结点的差值放大到$V_{DD}$需要的时间为：$$\frac{t_1}{\tau_1}=-\ln\left(1-\frac{G}{A_1}\right)$$其中$\tau_1=R_LC_L,A_1=V_{\mathrm{XY}}(\infty)/V_{XY0}\approx g_{m1,2}R_L$
而为了提升速度，利用XCP的再生放大能力，我们将电路改成（b）的结构，利用CK时钟信号关闭掉$M_1,M_2$，这时候所需要的时间可以表示为：$$\frac{t_1}{\tau_1}=\frac{1}{A_2-1}\ln G$$其中$\tau_{1}=R_{L}C_{L}/(g_{m3,4}R_{L}-1),A_2=g_{m3,4}R_L$
对于这两种情况，我们假设$A_1\approx A_2 \approx 5$，在（c）中可以看出，在需要同等的增益$G$的情况下，（b）电路所需要花费的时间更短，优势更明显

如下图所示，这是一个用在DRAM中的灵敏放大器，在预充电阶段，$M_3,M_4$管将X结点和Y结点上拉至$V_{DD}-V_{TH}$，$S_1$管用于消除X、Y结点之间的mismatch，这个mismatch可能是由上一个周期的残差引起，也可以是因为$M_3,M_4$的阈值电压失配，但这个电路有个缺点是不能达到rail-to-rail的输出，对于高电平的输出有影响
![alt text](<Pictures/Cross-Coupled Pair-image-5.png>)
灵敏放大器的时钟控制必须确保三个不同的时间间隔：预充电（均衡）、位线感测和放大。

为了达到rail-to-rail的输出，很多种架构被提出，下面是其中一种：
![alt text](<Pictures/Cross-Coupled Pair-image-6.png>)

随着对更高速和更低电源Supply的要求，失配在灵敏放大器中的影响越来越大，下图展示了在DRAM中早期的失调校准策略：用电容$C_1,C_2$存储$M_1,M_2$阈值电压上的失配
![alt text](<Pictures/Cross-Coupled Pair-image-7.png>)

### 锁存器 Latches

XCP的快速放大特性在锁存器设计中也证明是有用的。如果速度是主要考虑因素，可以使用具有中等电压摆幅（单端200–300 mV）的电流模逻辑（CML）锁存器。
![alt text](<Pictures/Cross-Coupled Pair-image-8.png>)
如图（a）所示，在CK为高时，$M_1,M_2$晶体管将X、Y结点的电压差放大，当CK为低时，XCP将X、Y的电压差迅速再生放大至$V_{DD}$，从而实现rail-to-rail，在再生阶段，因为rail-to-rail的最大电压差只能为$V_{DD}$，因此我们需要$g_{m3,4}|V_X-V_Y|R_D>V_{DD}$以确保电路有这个能力实现rail-to-rail，$|V_X-V_Y|$的最大值为$V_{DD}$，因此我们需要保证$g_{m3,4}R_D>1$
（a）电路是以利用静态功耗来实现的高速，除此之外，在先进工艺（28nm）下，如果需要极致的速度，可以通过（b）所示的电路，利用电感（通过引入电感，可以补偿高频时电容带来的衰减效应，从而扩展电路的带宽，使其能够以更高的速度工作）在消耗静态功耗和大面积的情况下实现高速。为了避免较大的过冲，需要保证阻尼因子$\zeta=(R_D/2)\sqrt{C_L/L_1}>0.7$

而在那些不需要很高速的场合，下图所示的电路结构很有用，（b）图展现了一种情况，可以看见$M_1,M_5$的串联必须“克服”$M_3$的作用。这种结构采用比逻辑，对于长宽比的设计有讲究，其次，与单端逻辑系列相比，互补的输入和输出摆幅产生的衬底噪声更小，这在混合信号设计中是一个有用的特性。
![alt text](<Pictures/Cross-Coupled Pair-image-9.png>)

最后，Latch的使用也可以包含逻辑门在其中，如下图所示，就包含了一个二输入NOR门，这里需要保证$M_3,M_4,M_5$的驱动能力大于$M_7$
![alt text](<Pictures/Cross-Coupled Pair-image-10.png>)

### 留给读者的问题 Questions for the Reader
1. 即使电路要作为放大器而非锁存器工作，我们是否必须在激活XCP时关闭图1(b)中的M1和M2？![alt text](<Pictures/Cross-Coupled Pair-image-11.png>)
   1. 必须关闭。因为电路在高速工作的情况下，差分Vin的变化是很快的，如果不关闭M1和M2，那么锁存器所做的决策会因为M1和M2而拖慢，进而不能判断出输出，导致输出的“模糊化”
2. 一个二分频电路集成了两个带电感峰化的CML锁存器实例。能否将与电感串联的电阻减小到零？
   1. 当电阻为零时，该分频器简化为注入锁定正交LC振荡器，其潜在工作速度更高，但锁定范围却非常有限（这意味着，只有当输入信号的频率与振荡器的自然谐振频率非常接近时，电路才能正常工作。如果输入信号的频率有少许偏离，振荡器就无法被成功“锁定”，导致电路功能失效。相比之下，带有电阻的原始 CML 锁存器可以在更宽的频率范围内可靠地工作）。

## PART Ⅲ
在本部分研究了XCP在模拟和射频电路中的应用。XCP在小信号工作时可用作负电阻或负阻抗转换器，在大信号工作时可用作再生电路。

### XCP作为负阻 XCP as Negative Resistance
如下图（a）所示，在M3的栅极注入一个小信号Vin，经过整个正反馈回路回到M3栅极的电压量是$(g_{m3,4}R_D)^2V_{in}$，所以要求回路增益$(g_{m3,4}R_D)^2<1$，同时可以注意到XCP还提供了一条电流支路，这允许$R_D$的取值可以更大，但缺点是限制了一个$|V_{GS3，4}|$的摆幅
![alt text](<Pictures/Cross-Coupled Pair-image-12.png>)
在某些应用场合下，XCP的非线性也是非常值得关注的，不能让$g_{m3,4}$的值总是在飘，我们可以借助（b）图中的电路来展现这种影响，通过计算当$Vout/\Delta I$下降到1dB（恶化$\approx 12\%$）来量化
$$V_{\mathrm{out}}=\Delta I_D[(2R_D)||(-2/g_{m3,4})]=\Delta I_D[2R_D/(1-g_{m3,4}R_D)]=1.12\Delta I_D\\
\Rightarrow \frac{g_{m3,4}}{g_{m3,4,\mathrm{eq}}}=1.12-\frac{0.12}{g_{m3,4,\mathrm{eq}}R_D}$$
其中$g_{m3,4,\mathrm{{eq}}}$表示XCP的平衡跨导，当$g_{m3,4,\mathrm{eq}}R_{D}=0.75$时，$g_{m3,4}/g_{m3,4,\mathrm{{eq}}}=0.96$，意味着当$Vout/\Delta I$恶化1dB的时候，可以容忍跨导恶化4\%，这里的关键点是，当$g_{m3,4,\mathrm{{eq}}}R_D$越接近于1，$g_{m3,4}/g_{m3,4,\mathrm{{eq}}}$的值越接近1，意味着可容忍的跨导下降百分比越小

此外，还可以利用衬偏效应来设计XCP，如下图（a）所示：![alt text](<Pictures/Cross-Coupled Pair-image-13.png>)这个电路的负阻$R_{eq}=-2/g_{mb},g_{mb}\approx0.2g_m$。这使得负阻的值可以更高，但缺点是为了避免PN结正偏，要严格控制M1,M2管的漏极电压不能低于$V_{DD}-0.5V$来确保漏极-阱正向偏压对晶体管性能的影响可忽略不计，为了控制BULK端，也要求将M1,M2管单独放在不同的N阱里。
在现代越来越低压的工艺设计下，因为不用担心PN结正偏太多的影响，这种结构的可用性越来越高。如图（b）所示，用这种结构来做op amp，因为$V_{DD}=0.5V$，那上述漏极电压的影响可以忽略不计，相比于传统的XCP，可以提升约5倍的电压增益。
在这个电路中输入共模电平和$V_b$可以被设置在接近于0的位置，从而确保上面四个晶体管全部工作在饱和区

需要指出的是：在存在失配的情况下，如果XCP的环路增益等于或大于1，它可能会再生并导致闩锁。因为这是正反馈

XCP也已被用于改善射频功率放大器（PA）的性能
在下图（a）中的PA输出级电路中，由于M1、M2需要驱动较大的电流，晶体管的尺寸会被设计的面积很大，从而具有较大的输入电容。通过引入XCP结构，能够抵消输入电容来降低驱动功耗（原本电流全部由电源提供进入M1，现在部分可以由XCP提供）；同时，它还能将放大器转变为一个高效的注入锁定振荡器，并能改善电路的共模稳定性，XCP结构的共模对地阻抗为$1/(2g_{m3,4})$
![alt text](<Pictures/Cross-Coupled Pair-image-14.png>)

### XCP作为负阻抗转换器（NIC） XCP as Negative Impedance Converter

一个常见的应用是创建负电容，以抵消端口处的正电容，从而提高速度
![alt text](<Pictures/Cross-Coupled Pair-image-15.png>)
由于输出级需要大电流，M1和M2往往较宽，表现出较大的输入电容。NIC会抵消部分该电容，从而增加X和Y处的带宽。

这种NIC存在两个问题
+ 在高速的情况下，M3和M4的寄生电容会影响NIC的性能，假设在饱和区只考虑MOS管$C_{GS}$的情况下，NIC的导纳可以表示为：$\\Y_{\mathrm{NIC}}=-\frac{1-\frac{C_{\mathrm{GS}}s}{g_{m}}}{(\frac{C_{\mathrm{GS}}}{C_{C}}+2)\frac{1}{g_{m}}+\frac{1}{C_{C}s}}\\$这里的关键点是$C_{GS}$会增大电阻分量，从而降低负电容的Q值，这会导致NIC的性能不佳，负电容的抵消值偏离前期设计值，带来的性能改善有限
+ 此外，导纳中的电阻部分会与形成张弛振荡器，在较高频的时候会产生振铃，只有使得$C_c$的值足够低才能避免这个效应

另一个表现出高电容的界面出现在包含静电放电保护的I/O焊盘处，因此会限制带宽。如下图所示，NIC可连接到这些焊盘，从而抵消部分电容。该电路可与T型线圈组合以进一步增强带宽[5]![alt text](<Pictures/Cross-Coupled Pair-image-16.png>)

此外还能够用作频率调谐，下图中M1，M2用作振荡（Oscillator），M3，M4用作NIC
![alt text](<Pictures/Cross-Coupled Pair-image-17.png>)

### 留给读者的问题 Questions for the Reader
1. 我们可以在PA输出级的输入端使用NIC来抵消输出级的输入电容吗？
   1. 由于RF预驱动级通常使用谐振负载，NIC反而将引起振荡。如果在这个阶段需要注入锁定，一个简单的交叉耦合对就足够了。
2. 利用衬底接漏极的XCP与传统的XCP相比，M1、M2管的热噪声对输出的影响二者相比有多大？
 ![alt text](<Pictures/Cross-Coupled Pair-image-19.png>)
   1. 该结构经过$R_L$产生的噪声为：$\sqrt{2}g_mR_L/(2-g_\mathrm{mb}R_LV_n)$，其中$V_n$代表每个栅极的参考噪声电压并忽略$R_L$的热噪声。而传统XCP结构的噪声为：$\sqrt{2}g_mR_L/(2-g_mR_LV_n)$（噪声计算存疑，因为量纲有错）
