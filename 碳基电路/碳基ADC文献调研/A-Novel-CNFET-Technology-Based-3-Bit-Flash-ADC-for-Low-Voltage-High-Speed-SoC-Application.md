# A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application

## 文献

> Sankar, P. A. G., & Sathiyabama, G. (2015). A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application. International Journal of Engineering Research in Africa, 19, 19–36.

Flash ADC适合1～4位的设计，插值、折叠类的FLash适合6～10位的设计

文章主要论述的是基于 32 nm CNTFETs的3位Flash ADC的设计，在比较器的设计上采用了一种叫做Threshold Inverter Quantization（TIQ）的技术，不再需要Resistor Ladder来生成参考电压，并且也加快了比较速度

编码分别采用了ROM和Fat tree技术

==核心点：反相器用作模拟电压比较器。反相器阈值用作比较器的参考电压。==
缺点：对工艺波动很敏感，反相器阈值的波动不应该大于1/2LSB

## CNTFETs宽度W的定义

定义两个碳管的中心间距为$Pitch$，碳管数量为N
则最小栅极宽度为：$$W_g=Max(W_{min},N*Pitch)$$
$W_{min}$代表能制作的最小栅极宽度

## CNTFETs的阈值电压

由于碳管的带隙与直径成反比，阈值电压可通过对半带隙进行一级近似得到：$$V_{th}=\frac{E_g}{2^*e}=\frac{\sqrt{3}}{3}\frac{a*V_\pi}{e^*D_{CNT}}=\frac{0.43}{D_{CNT}(nm)}$$
其中$a=2.49A,V_{\pi}=3.033eV$，$V_{\pi}$表示在紧束缚模型中的键能
$$D_{CNT}=\frac{a*\sqrt{n^2+m^2+nm}}{\pi}$$

## 基于TIQ的Flash ADC架构

![基于TIQ的Flash ADC架构](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image.png>)
如上图所示，输入信号进来后依次经过比较、放大、01数字转换、编码，之后输出
其中TIQ比较器是最重要的

### Threshold Inverter Quantization（TIQ）

![TIQ Comparator](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image-1.png>)

具体的结构就是一个两级反相器，通过将输入$V_{in}$和反相器内部的阈值电压$V_m$进行比较，当$V_{in}>V_m$的时候，输出为1
内部阈值电压$V_m$的定义是输入输出曲线中$V_{in}=V_{out}$的点
![反相器特性曲线](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image-2.png>)
改变$V_m$可以通过改变N管和P管的栅极宽度来实现
$V_m$可以替代传统架构中Resistor Ladder生成的$V_{ref}$

![阈值电压调制](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image-3.png>)
通过选取合适的矢量参数，设计CNTFETs的宽度，可以调制得到不同的内部阈值电压$V_m$，进而生成7个不同的$V_{ref}$

### Gain Booster

用两个级联的反相器组成，相当于缓冲器，目的是将TIQ的输出放大到全摆幅，输出0/1，这时候从这出来的信号就是温度编码了（Thermometer Code）

### Thermometer Code(TC) to Binary Code(BC) Encoder

将温度编码（TC）转为二进制编码（BC）是Flash ADC转换的最后一步，文章在编码（Encoder）之前，先用了1-out-of-n技术，这一步是由与门和反相器组成，目的是将温度编码中的最高位“1”的位置提取出来，全0的话最低位置1

### ROM Type Encoder

![ROM Encoder](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image-4.png>)
用ROM编码实现，逻辑清晰，但是晶体管多，速度受限，延迟高，是整个Flash ADC系统的速度瓶颈

### Fat Tree Encoder

![Fat Tree Encoder](<Pictures/A Novel CNFET Technology Based 3 Bit Flash ADC for Low-Voltage High Speed SoC Application-image-5.png>)
a7到a0分别代表1-out-of-n的编码结果，每一位均可以通过通过两级或门生成
作者提出的这种编码方式速度更快，但是规则性不如ROM，没有考虑逻辑错乱的问题，结果中很难不有毛刺

## 工艺波动

考虑了反相器内部阈值电压在工艺波动（温度、电源电压、碳管直径、源漏接触长度）下的影响，最终说明这些因素对CNT ADC的性能影响不大
