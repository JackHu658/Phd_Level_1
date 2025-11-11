# Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design

## 文献

> I. O'Connor et al., "CNTFET Modeling and Reconfigurable Logic-Circuit Design," in IEEE Transactions on Circuits and Systems I: Regular Papers, vol. 54, no. 11, pp. 2365-2379, Nov. 2007
> F. Prégaldiny, J.-B. Kammerer, and C. Lallement, “Compact modeling and applications of CNTFETS for analog and digital circuit design,” in Proc. IEEE ICECS, Nice, France, Dec. 10–13, 2006, pp. 1030–1033

这篇文章也是关于CNT的紧凑模型，是对第一篇文献的补充，讲述CNTFETs在模拟/数字领域的应用

## 传统数字电路

![数字电路](<Pictures/Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design-image.png>)
利用Unipolar CNTFETs，其行为表现和传统的MOSFETs一样，所以可以用传统的CMOS逻辑来设计数字电路，文中就设计出了INV和NAND门，然后用正弦波转方波进行了验证
![正弦波转方波](<Pictures/Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design-image-1.png>)

## 使用 Ambipolar CNTFETs 搭建模拟电路

![双极型晶体管](<Pictures/Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design-image-2.png>)
根据上图可以看到，CNTFETs的制造工艺不同，电流电压曲线也不同。上图就是双极型的CNTFETs，需要注意的是谷底是在$V_{GS}=\frac{1}{2}V_{DS}$，并且随着$V_{DS}$的增加，曲线是沿着红色曲线往上走的

根据这个特性，可以利用一个双极型晶体管做一个模拟倍频器，电流特性有：
![电流公式](<Pictures/Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design-image-3.png>)
当$v_{in}$从$-v_{in}$变化到$v_{in}$的时候，输出电流刚好经过一个完整的周期——但是$v_{in}$才变化了半个周期，借此可实现AC倍频！

![双极型倍频器](<Pictures/Compact Modeling and Applications of CNTFETs for Analog and Digital Circuit Design-image-4.png>)
放大器的使用是为了锁住信号，使得Ambipolar管的$V_{GS}=\frac{1}{2}V_{DS}$
电容$C_{in}$隔直流
