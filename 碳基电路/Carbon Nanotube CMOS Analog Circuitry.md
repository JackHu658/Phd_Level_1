# Carbon Nanotube CMOS Analog Circuitry

## 文献

> R. Ho, C. Lau, G. Hills and M. M. Shulaker, "Carbon Nanotube CMOS Analog Circuitry," in IEEE Transactions on Nanotechnology, vol. 18, pp. 845-848, 2019

## 本征增益优势

![与硅基相比，CNT的本征增益优势](<Pictures/Carbon Nanotube CMOS Analog Circuitry-image.png>)
对于硅CMOS而言，本征增益会随着技术节点的缩小而降低。而碳纳米管本质上更高的跨导（$g_m$）使得在先进的$10nm$技术节点下的CNTFETs仍能实现与硅基$>1 \mu m$技术节点下的硅场效应晶体管相同的本征增益。
==因此，CNTFETs能够在先进技术节点上实现模拟电路，同时获得高本征增益，从而实现模拟电路的大幅面积缩小。==
本征增益是通过电路仿真（Cadence Spectre）提取的，其中硅CMOS使用PDKs，CNTFETs使用经过实验校准的==紧凑模型==（使用沟道长度小于$10nm$的CNTFETs进行校准）

## 两级运放制造

![两级运放](<Pictures/Carbon Nanotube CMOS Analog Circuitry-image-1.png>)
这个里面运放正负端口标反了，但图画的是对的，利用制造出的多个相同的电路测试均一性，发现二级运放增益均大于700，一致性也很好，增益的最大值发生在0附近，代表失调很低，测量出的$V_{offset}<12 mV$

## CNT呼吸传感器

利用制造出来的二级运放，作者制造出了一款基于CNTFETs的呼吸传感器，用CNTFETs作为传感单元和模拟前端电路
![呼吸传感器](<Pictures/Carbon Nanotube CMOS Analog Circuitry-image-2.png>)

化学传感器输出电流信号，经过TIA转化为电压信号，后面再接一级Buffer
图(c),(d)显示出传感器的线性度和长时间的稳定性
图(e)是每分钟内呼吸和直接向传感器吹氮气交替进行，第一分钟在呼吸，Vout上升，一分钟后不再对着传感器呼吸，输出电压又逐渐下降到0，工作正常进行
