# Voltage Follower

## 文献

> [1]A. Sheikholeslami, "Voltage Follower, Part 1 [Circuit Intuitions]," in IEEE Solid-State Circuits Magazine, vol. 14, no. 3, pp. 13-15, Summer 2022
> [2]A. Sheikholeslami, "Voltage Follower, Part II [Circuit Intuitions]," in IEEE Solid-State Circuits Magazine, vol. 15, no. 1, pp. 17-19, winter 2023
> [3]A. Sheikholeslami, "Voltage Follower, Part III [Circuit Intuitions]," in IEEE Solid-State Circuits Magazine, vol. 15, no. 2, pp. 14-26, Spring 2023
> [4]A. Sheikholeslami, "Voltage Follower, Part IV [Circuit Intuitions]," in IEEE Solid-State Circuits Magazine, vol. 15, no. 3, pp. 6-19, Summer 2023

## PART Ⅰ

一个理想的电压跟随器/电压缓冲器（Voltage Follower/Voltage Buffer）应该具有以下四个特征：

+ 输入阻抗无限大——使得Voltage Buffer不会从输入信号源吸收任何的电流，否则这可能会导致输入信号的电压下降
+ 输出阻抗为0——使得输出电压全部降落到负载上
+ 在空载状态下（$R_L=\infty$）电压增益为1——使得输出信号跟随输入信号
+ 带宽无限大——使得电压增益始终为1，而与频率无关

### 不考虑电容的影响

下图展示了理想特性下的Voltage Buffer，$V_{oc}$表示空载电压（open circuit）
![alt text](<Pictures/Voltage Follower-image.png>)
但是在实际情况下我们并不能够达到上述四种特性，为简单起见，我们先分析低频下的情况，然后再转向高频

如图2（a）所示，电压跟随器的一种常见实现方式由一个NMOS晶体管组成，其漏极连接到电源，源极连接到偏置电流。另一种实现方式包括一个PMOS晶体管，其漏极接地，源极接偏置电流，如图2(b)所示。在这两种实现方式中，输入电压施加到晶体管的栅极，输出从晶体管的源极获取。
![alt text](<Pictures/Voltage Follower-image-1.png>)
在分析前，我们先定义$g_{me}=g_m+g_{mb},g_mr_o\gg 1$；两个电路都表现出无穷大的输入电阻——因为MOS晶体管的栅极通过电介质（绝缘体）与其沟道隔离。

在DC状态下，输入与输出之间存在一个$V_{GS}$的偏移；在AC状态下，输出会跟随输入的变化。使用NMOS和PMOS的区别在于输出电压所经历的直流电平偏移，一个是向上偏$V_{GS}（PMOS），一个是向下偏$V_{GS}$（NMOS）

当考虑体效应和Early效应的时候，输出与输入的比值不再为1，而有略微下降：$$\frac{V_{\mathrm{out}}}{V_{\mathrm{in}}}=\frac{g_{m}}{g_{\mathrm{me}}+\frac{1}{r_{o}}}$$

图3是基于45nm工艺的Voltage Buffer电路仿真结果，可以看到电平的位移为520mV，NMOS和PMOS的区别就在于位移是向上还是向下；输出的峰峰值比上输入的峰峰值约为0.7，增益的下降就是由于体效应和Early效应的影响（除了晶体管自身有限输出阻抗的影响，其实还有电流源的有限输出阻抗影响）
![alt text](<Pictures/Voltage Follower-image-2.png>)

图4展示了两个源极跟随器的直流特性，并将其与理想电压跟随器的直流特性进行了比较。这里可以看出两个明显的偏差：

+ 我们所期待的Voltage Buffer可以接受rail-to-rail的摆幅（文中为$0\sim1V$），而在实际情况下，PMOS的输入范围是$0\sim0.6V$，NMOS的输入范围是$0.4\sim1V$，可以预料到晶体管的阈值电压$V_{TH}\approx 0.4V$
+ 电压增益为0.7，偏离理想的1

![alt text](<Pictures/Voltage Follower-image-3.png>)

### 考虑电容，一个小阶跃信号对于电路的影响

如果在输出端负载接一个电容，那么对于输入处的小正/负阶跃信号，由于输出端电容上的电压不能突变，输出电压会被电流源$I_B$限制，以NMOS为例，具体表现为：

+ 当输入端有一个正阶跃信号，$V_{GS}$上升，多出来的电流会流经$C_L$，输出端电压上升，晶体管位于饱和区
+ 当输入端有一个负阶跃信号，$V_{GS}<V_{TH}$，晶体管截止，电流从$C_L$抽出，输出端电压下降，电流受到$I_B$限制，可以理解为摆率限制

在理想状态下，$V_{in}$跳变多少，$V_{out}$就该跟随着跳变多少，但是由于实际情况下增益小于1，所以最终$V_{out}$的变化值会比输入的跳变量要小，换句话说就是$V_{GS}$会比原来的值更大
![alt text](<Pictures/Voltage Follower-image-5.png>)
图5展示了这一过程，可以看到每次跳变后$V_{GS}$都回不到跳变前的位置，意味着增益不是理想的1；在负跳变时，可以明显看到$V_{out}$的上升有一段直线，这是因为摆率电流的限制

我们在此的主要观察结果是：输出电压的上升沿和下降沿具有不同的压摆率。正压摆率会随着正向电压的增加而增大，而负压摆率则受到所提供偏置电流的限制。这种压摆率的不对称性是偏离理想电压跟随器特性的另一种表现。

### 考虑带宽

在Voltage Buffer电路中，3dB带宽大致为：$1/(2\pi R_{out} C_L)$

对于恒定的负载电容$C_L$，我们必须降低$R_{out}$，这需要我们增加$g_m$，这要以增加功耗和面积来实现

==这里的一个重要问题是，我们能否在不增加功耗或面积的情况下修改电压跟随器的设计，以降低输出电阻，从而增加带宽？==

### 总结

总而言之，简单的源极跟随器接近实现两个理想特性：1.无穷大的输入电阻；2.开路电压增益仅略低于1

Voltage Buffer的输出阻抗接近于$1/g_m$，这对于某些负载而言可能还不够低，尤其是在高频下的容性负载。

因此我们将在后面几个PART介绍其他电路拓扑结构的Voltage Buffer

## PART Ⅱ

### Flipped Voltage Follower（FVF）

在上一part中我们介绍了最基本的Voltage Follower结构，在这篇文章中被称为SVF（Simple Voltage Follower），表现为：无限的输入阻抗，有限的输出阻抗（大约为$1k\Omega$量级），略低于1的电压增益。在一般情况下这种Voltage Follower是可以接受的，但是当负载达到$1k\Omega$量级时，这种结构就变得不可接受了

因此在这一部分中，我们介绍Flipped Voltage Follower（FVF）结构，在保证与SVF拥有相同电压增益的同时，降低输出阻抗

下图展示了基本的SVF和FVF结构：
![alt text](<Pictures/Voltage Follower-image-6.png>)
对于FVF一种简单又直觉的理解方式是：从SVF结构中可以看出，因为负载的存在，每次从M1晶体管流出的电流为$I_B$加上流过负载的电流，而$I_B$电流又被固定，当$V_{in}$上升时，$V_{out}$必定也会抬升，导致从M1流出的电流增加，于是M1晶体管的$V_{GS}$会升高，导致实际的电压增益脱离1，当负载值越小，这种偏离就越明显；而FVF结构中为了避免这种问题，将电流源$I_B$放到了M1管上头，这样流过M1的电流就被$I_B$固定死了，导致M1的$V_{GS}$也被固定，于是电压增益就更加接近于理想的1了，但是由于在上头接了一个电流源，在实际情况中，电流源被晶体管替代，输入电压的范围是实打实被缩小了

将电流源的位置从M1的源极（SVF）到漏极（FVF）视为Flipping

### 小信号计算

#### 等效跨导

如下图所示，我们先求FVF的等效跨导，这个电路我们可以看作是一个两级放大器，第一级放大电压，第二级是一个跨导
![alt text](<Pictures/Voltage Follower-image-7.png>)
根据（b）图不难看出：$v_{g2}=v_{d1}=-g_{m}r_{o}v_{\mathrm{in}}\Rightarrow i_{sc}=(g_mr_o)g_mv_{\mathrm{in}}$
在这里我们假设M1、M2管具有相同的跨导，因此可以求得：$$G_m=\frac{i_{sc}}{v_{\mathrm{in}}}=(g_mr_o)g_m$$

由此可看出，相较于SVF，FVF结构的等效跨导提升了$g_m r_o$倍！这种跨导的改善是由M1的本征增益与M2的跨导共同提供的
换句话说，与SVF电路中M1产生小信号电流并将其输送给负载不同，FVF中的M1对负载贡献的电流为零，只是将电压放大了本征增益倍
从part1中我们知道，要想最终电压增益接近1，就要增大$g_m$，在SVF中这种增大要以功耗和面积为代价，而在这里能改善这种情况

#### 输出阻抗

如下图所示，流过M1源极的电流为零，因为该路径会在M2的栅极处形成开路。
而为了求出流过M2的电流，我们首先需要确定M2栅极的电压。图2(b)展示了求解过程：需找到其短路电流并将其乘以输出电阻（这其实就是诺顿定理）
![alt text](<Pictures/Voltage Follower-image-8.png>)
短路电流：$i_{sc}=v_x\left(g_{me}+\frac{1}{r_o}\right),g_{me}=g_m+g_{mb}$
求得的M2栅极电压：$v_{d1}=i_{sc}r_o=v_x(1+g_{me}r_o)$
因此得到：$i_2=v_xg_m(1+g_{me}r_o)+\frac{v_x}{r_o}$
假设$g_m r_o \gg 1$，最终求得的输出阻抗为：
$$R_{\mathrm{out}}=\frac{V_x}{i_2}\approx\left(\frac{1}{g_mr_o}\right)\left(\frac{1}{g_{me}}\right)$$
这个值相较于SVF结构，降低了$g_m r_o$倍！输出阻抗的减小，使得负载的驱动压力被大大减小
从直觉上来看，这个也是合理的，因为在输出端施加的一个小信号，会被M1晶体管放大$g_m r_o$倍，从而让M2晶体管在输出端“汲取”更多的电流，表现为输出阻抗的降低

#### 电压增益

将等效跨导和输出阻抗相乘，可以得到电压增益为：$$\frac{V_\mathrm{out}}{V_\mathrm{in}}\approx\frac{g_m}{g_{me}}$$
这个和SVF结构中得到的结果完全相同，实现的条件是更大的跨导（相比于SVF乘本征增益倍）和更小的输出阻抗（相比于SVF除本征增益倍）

### 仿真结果比较

仿真条件设置为：$I_B=50\mu A,C_L = 100fF$

| PARAMETER | NMOS (M1 & M2) |
| ---| ---|
| $W/L$ | $400nm/45nm$ |
| $V_{TH}$ | $0.43V$ |
| $g_m$  | $0.5mS$|
| $g_{mb}$ | $0.15mS$ |
| $ r_o $ | $11.15k\Omega$|

下图是仿真结果：
![alt text](<Pictures/Voltage Follower-image-9.png>)
可以看到，二者的电压增益大致都为0.7，FVF结构的DC偏移比SVF结构更高，这是因为电流源的存在，使得FVF结构下的$V_{DS1}$小于SVF结构下的，由于偏置电流不变，所以为了维持相同的电流，FVF结构的$V_{GS1}$会更大来保证电流不变

![alt text](<Pictures/Voltage Follower-image-10.png>)
从上图中可以看出，在$V_{in}>0.6V$才开始进入各自的跟随状态

最后，由于FVF结构的输出阻抗下降了$g_m r_o$倍，其频率也应该上升这么多倍，而在45nm工艺中，本征增益大致在5~10的范围，下图的频率响应完美符合了这个结论
![alt text](<Pictures/Voltage Follower-image-11.png>)
值得一提的是FVF在超过主极点后下降的速率是$40dB/dec$，这是因为M2的栅极处也有一个极点

### 总结

我们已经证明FVF提供与SVF相同的电压增益；它通过提供更大的短路跨导和更低的输出电阻来实现这一点
FVF电路还可以进行进一步的变化和改进，我们将在下一部分中介绍其中的一些

## PART Ⅲ

在这一部分中，我们探讨另一种FVF变体结构，又被称作*Supper Source Follower(SSF)*
三种基本结构如下图所示：
![alt text](<Pictures/Voltage Follower-image-12.png>)
SSF结构中使用一个PMOS，流过的电流为$I_{B2}$

### 小信号推导

假设流过的电流$I_{B1}=I_{B2}=I_B$，所有的MOS管$g_m,r_o$均相同，且位于饱和区

如下图所示，运用与推导FVF相同的方式来推导SSF的等效跨导和输出阻抗
![alt text](<Pictures/Voltage Follower-image-13.png>)
可以得到：$$\begin{cases}
    G_m=(g_mr_o)\times g_m\\
    R_{\mathrm{out}}=\frac{v_x}{i_x}\approx\left(\frac{1}{g_mr_o}\right)\left(\frac{1}{g_{me}}\right)
\end{cases}$$
最终得到的等效跨导和输出阻抗和FVF的结果完全相同，且电压增益均为：$$\frac{V_{\mathrm{out}}}{V_{\mathrm{in}}}\approx\frac{g_m}{g_{\mathrm{me}}}$$

### SVF、FVF、SSF比较

对于传统的SVF来说，负载电流的提供和负载电压的提供均由M1晶体管完成。若不接负载，则M1晶体管流过的电流恒为$I_B$，那么意味着输出电压始终会跟随着输入电压的变化（因为$V_{GS}$不变），这样虽然是个理想的Source Follower，但是也失去了缓冲的意义。因此==提出FVF、SSF结构就是将电流和电压的提供解耦，用M1和M2两个晶体管来分别完成==

在FVF中，M1的电流由$I_B$固定。没有信号电流能够流过M1，原因很简单，它与理想直流电流源串联。这将保证栅源电压$V_{GS1}$固定，因此无论有载还是空载，都能实现完美的电压跟随。另一方面，晶体管M2完全负责提供负载电流。这里有一个负反馈：假设输出节点处的电压上升超过输入节点的电压上升，这导致了$V_{GS1}$下降，但是$I_B$电流不变，于是要提升$V_{D1}$，这也与M2的栅极相连，从而会产生相应的电压增量，使得M2从输出节点汲取相应的电流，从而导致输出电压下降。
这种负反馈机制因为在输出端检测的是电压，因此是电压负反馈，降低了输出阻抗

### 仿真结果比较

这部分与PART Ⅱ的仿真结果差不多，不再过多赘述
![alt text](<Pictures/Voltage Follower-image-14.png>)![alt text](<Pictures/Voltage Follower-image-15.png>)![alt text](<Pictures/Voltage Follower-image-16.png>)

在这里有两个问题值得去思考：

+ 假设所有条件相同的情况下，SSF从电源汲取的电流是FVF的两倍，即消耗两倍的功率，同时提供相同的输出电阻。那使用SSF而非FVF有什么优势吗？
+ 通常情况下，在设计中我们分别在哪些情况下使用SVF、FVF和SSF？

## PART Ⅳ

在这一部分，我们强调上文提及的三种设计共有的一个主要局限性——正负摆率不对称
然后将FVF和SSF设计相结合，得到了一种改进的电压跟随器，称为AB类SSF（AB-SSF）。

简单起见，我们忽略电路中除负载电容$C_L$外的所有电容，且在AB-SSF中用到的电容$C_B$在dc状态下开路，在任意频率处分析时均短路

如下图所示，SVF的负摆率$SR^-$被偏置电流$I_B$限制，而正摆率$SR^+$则与$V_{GS}$的变化量相关，不会受到摆率的限制，在文章的仿真设计中，$SR^+$可以达到$SR^-$的$5 \sim 6$倍
![alt text](<Pictures/Voltage Follower-image-17.png>)

在FVF中，由于我们“Flipped”了电流的方向，所以会表现出相反的结论，即：$SR^+ < SR^-$

在SSF中，当负跳变时，M1截止，M2栅极电压迅速上升，导致上面流到下面的电流很小，因此得到与SVF中相同的结论：$SR^+ > SR^-$

我们希望提出一种设计，以消除这种不对称性，并为波形的上升沿和下降沿提供更大的压摆率。提高压摆率的一种方法是简单地增加偏置电流，但这会导致功耗增加，而我们理想情况下希望避免这种情况。

文中提出的解决方案是将FVF和SSF设计相结合，形成了一种具有AB类工作的SSF
![alt text](<Pictures/Voltage Follower-image-18.png>)
我们将首先论证AB-SSF如何保持FVF和SSF的优势（例如具有低输出电阻），然后论证其对称摆率特性，最后描述其AB类工作方式。

### AB-SSF如何保持FVF和SSF的优势

如图2(c)所示，晶体管$M_1$依旧承担着"Source Follower"的角色，流过的电流恒定为$I_{B1}$，所以$V_{GS1}$保持恒定，而晶体管$M_2,M_3$承担着提供电流给负载的任务
在直流条件下，$M_3$流过电流$I_{B1}+I_{B2}$，其中$M_1$管流过$I_{B1}$，$M_2$管流过$I_{B2}$
下面依旧照例进行小信号分析，我们用$v_{\mathrm{out}}=i_{\mathrm{sc}}\times R_{\mathrm{out}}$来计算输出电压，其中$i_{\mathrm{sc}}$代表短路电流；$R_{\mathrm{out}}$代表输出电阻
小信号模型如下图所示，假设$R_B$特别大，$C_B$在我们感兴趣的频率内近似为短路
![alt text](<Pictures/Voltage Follower-image-19.png>)
由此计算得出：$$\begin{cases}
i_{\mathrm{sc}}=(g_{m2}+g_{m3})g_{m1}r_{o1}v_{\mathrm{in}}\\
R_{\mathrm{out}}=\frac{1}{(g_{m2}+g_{m3})(g_{\mathrm{me1}}r_{o1})}
\end{cases}
\Rightarrow v_{\mathrm{out}}=i_{\mathrm{sc}}\times R_{\mathrm{out}}=g_{m1}\frac{v_{\mathrm{in}}}{g_{\mathrm{me1}}}$$
可以看出增益是和FVF、SSF保持一致的，但相较于二者，AB-SSF的短路电流提升了一倍，输出电阻又减小了一倍

### AB-SSF的摆率特性

从下图的仿真中可以看到：所有三种设计在输出节点处都表现出相似的摆幅；但是FVF和SSF分别在上升沿和下降沿表现出有限的摆幅，但AB-SSF没这种限制
![alt text](<Pictures/Voltage Follower-image-20.png>)
在这里改善摆幅的核心的思想是：当有阶跃上升时，AB-SSF表现出与SSF相似的行为；当有阶跃下降时，AB-SSF表现出与FVF相似的行为

下图是在输入信号为$3GHz$的条件下仿真得到的结果，可见只有AB-SSF不受摆率影响继续跟随
![alt text](<Pictures/Voltage Follower-image-21.png>)

### AB-SSF的工作方式

如下图所示，之所以说是AB类工作方式，是因为每个晶体管仅在周期的一部分（超过一半）导通电流，并在其余部分停止导通电流
![alt text](<Pictures/Voltage Follower-image-22.png>)
