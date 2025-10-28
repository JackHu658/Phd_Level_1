# CNTFET Modeling and Reconfigurable Logic-Circuit Design

## 文献

> I. O'Connor et al., "CNTFET Modeling and Reconfigurable Logic-Circuit Design," in IEEE Transactions on Circuits and Systems I: Regular Papers, vol. 54, no. 11, pp. 2365-2379, Nov. 2007

使用新材料与硅基CMOS对比的话需要对比：集成密度、速度、功耗、可靠性

碳纳米管的直径通常为1至3纳米，但长度可达数微米。碳纳米管可用于构建低电阻高强度互连线（金属管）以及高度可扩展的低功耗碳纳米管场效应晶体管（CNTFET）和单电子隧穿晶体管

可以认为，CNTFET可在两种场景下用于构建逻辑电路：

1. **直接用CNTFET代替MOSFET**。在此场景中，必须证明可以实现显著的性能提升，从而证明将制造技术转向基于CNTFET的逻辑电路是合理的。为此，有必要开发紧凑的CNTFET模型，这些模型不仅要适合逻辑设计人员使用，还需要能够评估新电路在预期故障、缺陷和参数变化方面的稳健性。这些紧凑模型必须既包含新的物理特性（**弹道输运和量子输运**），又要准确建模器件参数（**特别是纳米管直径和接触电阻**）的影响。
2. **利用CNTFET的特定性能来构建MOSFET无法实现的逻辑功能**。必须准确模拟CNTFET的特定特性（如双极性——ambipolarity）

## CNTFET的分类

### 肖特基势垒CNTFET（Schokkey-barrier CNTFET，SB-CNTFET）

![SB-CNTFET](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image.png>)

表现出强烈的双极性，会根据栅压在N型和P型间转换，开关比不好，适合用来搭建新型逻辑

## CNTFET双极性的应用

![动态可重构8功能函数逻辑门电路图](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-1.png>)
![功能表](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-2.png>)
如上图所示，CNTFET栅极有两个电压端口，可以通过调整CNTFET的背栅偏置电压改变晶体管极性，从而实现8种功能不同的逻辑门。当背栅电压为-1V时为p-type，1V时候为n-type

A和B是输入端布尔逻辑，Y端口输出
![逻辑时序图](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-3.png>)
根据时序图可判断和验证相应的功能（这里以NOR门为例），在背栅没有电压的时候，就是简单的1导通，0关闭逻辑，pc1、ev1、pc2、ev2依次导通完毕后生成最终的数字逻辑

另一种可重构逻辑门：
![另一种可重构逻辑门](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-4.png>)
![alt text](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-5.png>)

更复杂些的话还可以实现ALU
![ALU](<Pictures/CNTFET Modeling and Reconfigurable Logic-Circuit Design-image-6.png>)
优势是能够在更短的时间内实现1位加法器的功能，且可重构
