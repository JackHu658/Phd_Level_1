# Miller's Secret

## 文献

> C. Mangelsdorf, "Miller’s Secret [Shop Talk: What you didn’t Learn in School]," in IEEE Solid-State Circuits Magazine, vol. 17, no. 1, pp. 21-27, winter 2025
> A. Sheikholeslami, "Miller's Theorem [Circuit Intuitions]," in IEEE Solid-State Circuits Magazine, vol. 7, no. 3, pp. 9-10, Summer 2015

## Miller Effect

首先简要回顾一下Miller Effect，如下图所示：跨接在X和Y结点的“悬浮”阻抗$Z$可以等效为X端和Y端接地的两个阻抗$Z_1,Z_2$
![alt text](<Pictures/Miller's Secret-image.png>)
对于电容而言，电容$C_F$在X结点和Y结点看到的等效电容值分别为：$C_1=(1+A)C_F,C_2=(1+A^{-1})C_F$
![alt text](<Pictures/Miller's Secret-image-1.png>)
需要注意的是：如果阻抗Z是构成X结点和Y结点唯一的通路，那么Miller Effect通常是不成立的。在阻抗Z与主信号平行出现的情况下，Miller Effect是有效的
![alt text](<Pictures/Miller's Secret-image-2.png>)

也可以通过下图根据电流相等推导出该结论
![alt text](<Pictures/Miller's Secret-image-10.png>)
如果假设阻抗Z是电阻R的话，且增益为负数（A<0），则意味着电阻串中必然有个位置的电压为0，如果将这个位置直接接地，电路不会受到任何影响。因为从此处流向地的电流必定为0，从1处流入的电流必然全部流入2，接地位将电阻自然而然地分成R1和R2，根据（c）图可以清晰地看出其中存在一个位置电压为0
![alt text](<Pictures/Miller's Secret-image-11.png>)

值得注意的是：当增益A为正数且大于1时，会出现负阻效应，可以看出左右两个端点的电压分别为$V_1$和$AV_1$，将这条曲线做外延，可以在曲线的左侧找到电压为0的点，曲线和x轴的交点值为负数，即为从$V_1$结点处看到的负阻抗，而$R_2$的阻值会比原本的$R$还大
![alt text](<Pictures/Miller's Secret-image-12.png>)

## 理解极点分裂（通过非代数分析的角度）

### Miller Compensation并不只是提供一个大电容

下图是一个经过米勒补偿的二级运算放大器简化模型，在Miller Feedback处引入单位增益缓冲器是为了消除RHP零点
![alt text](<Pictures/Miller's Secret-image-3.png>)
将Miller Effect带来的效果换成相同倍数的电容值接在X结点和地之间，得到的频率响应如下图所示：显然可见Miller Compensation带来的效果并不是只有在X结点引入一个大电容那么简单（虽然这保证了主极点位置相同），但Miller Compensation还带来了次极点的改变。这种情况就叫做极点分裂（Pole Splitting）
![alt text](<Pictures/Miller's Secret-image-4.png>)
不仅第一级输出端的极点通过密勒倍增效应被推至非常低的频率，而且与第二级输出端相关的次极点被推向另一个方向，即更高的频率。因此，放大器中两个频率最低的极点实际上通过添加反馈电容被分开了。

### 直观的解释

对于一个闭环系统来说，其传递函数为：
$$\begin{cases}
    H=A/(1+Af)\\
    For \quad Af\gg1 \rightarrow H \approx 1/f \\
    For \quad Af\ll1 \rightarrow H \approx A
\end{cases}$$
反馈网络$f$一般为阻性的，因此其在波特图中表示的为一条横线
将$H,f,A$表示在同一张波特图中可以看出，闭环传递函数$H$的极点将由$A$与$1/f$的相等点决定
![alt text](<Pictures/Miller's Secret-image-5.png>)

在Miller Compensation中同样也可以用相同的思路表示：
![alt text](<Pictures/Miller's Secret-image-6.png>)
在这里绿色的线代表的是第二级和Miller反馈路径组成的局部传递函数，而不是整体环路；而这里的反馈路径是由Miller电容$C_c$构成，因此会以-20dB/dec进行滚降，闭环传递函数曲线由红色线构成，可以看出两个极点被分裂了，并且当$C_c$越大，极点会分的越开

次级点被推向更高频可以用Miller电容组成的Local feedback来理解：反馈的作用往往是用来降低输出阻抗，而通过Miller电容进行的反馈实际上是降低了第二级输出端的阻抗，从而拓展了极点的带宽：
$$R_{out,CL}=\frac{R_{out}}{1+A_2f}\approx\frac{R_{out}}{g_{m2}R_{out}f}=\frac{1}{g_{m2}f}\approx\frac{1}{g_{m2}}$$
输出阻抗由之前的$ro\parallel ro$变为了约$1/g_m$，从而提升了次极点的位置

我们在解释上述理论的一个前提是：从主极点到次极点的以-20dB/dec的滚降均是由主极点贡献的，但通过实际仿真发现不是这样
![alt text](<Pictures/Miller's Secret-image-7.png>)
如上图所示，在频率低于50MHz的时候确实是由第一级的主极点贡献滚降，但在50MHz后第一级的输出被零点拉直，而第二级的极点开始起作用，因此整体表现出的依旧是-20dB/dec的滚降，但在50MHz后这种滚降是由第二级贡献的，并不是主极点

### 实际理解

从上面一张图中我们可以看出，主极点的-20dB/dec的滚降是由两个极点接力进行的，造成这个现象的主要原因是在频率上升时，不可忽略的第二级增益

第二级的输入阻抗为：$$Z_{X}=\frac{1}{sC_{eq}}\approx\frac{1}{sA_{CL2}C_{3}}$$
其中$A_{CL2}$代表第二级增益，$C_3$代表Miller电容
+ 当频率$<50Mhz$时：由第一级的输出贡献主要的极点，此时第二级增益$A_{CL2}$还未明显下降，由于Miller倍增电容的影响，只有第一级极点的影响，函数整体表现出-20dB/dec的滚降
+ 当频率大于50MHz，小于3GHz时：这个时候第二级增益开始按照-20dB/dec的速度滚降，带来的效果是阻抗$Z_X$的升高，而电容本身会随着频率的上升而有-20dB/dec速度的滚降，这二者相互抵消，反而使得阻抗$Z_X$趋于一个常数（约$66.3\Omega$），这个时候第一级的波特图反而会趋于平坦，这对应了在第一级输出节点处有一个零点。这对零极点对的出现是有因果关系且可以完全抵消的：正是由于第二级增益的下降，导致了阻抗$Z_X$的上升，因而整体表现的是一种连续的以-20dB/dec下降的趋势
![alt text](<Pictures/Miller's Secret-image-8.png>)
+ 当频率大于3GHz的时候，有两种可能的方式会结束这种模式，导致幅频曲线以-40dB/dec下降，从而出现次极点
  + 当第一级的输出电容$C_1$随着频率的上升降至$66.3\Omega$以下，此时这个电容会占据主导地位，促使第一级的增益随着频率的上升继续下降
  + 第二种情况是当米勒电容相对$C_1$来说很大时，第二级的增益先降至0dB，Miller倍增的效果消失，这时候Miller Cap就是一个普通的电容，不再呈现电阻性

### 次极点的作用
从下图中可以看出，通过改变第二级输出结点的电容，其幅频曲线变化很小，因此Miller Compensation在一定程度上降低了负载对增益的影响；这也可以用来指导我们设计，例如我们可以通过增大第二级的增益且减小负载电容的大小来让次极点移向更高频
![alt text](<Pictures/Miller's Secret-image-9.png>)

在自然界中，“极点”分裂是一个很常见的事，当用一个无源器件将两个独立且具有相同时间常数的系统耦合在一起的时候，极点就发生了改变，这种变化可以通过区分共模、差模分量进行单独分析
