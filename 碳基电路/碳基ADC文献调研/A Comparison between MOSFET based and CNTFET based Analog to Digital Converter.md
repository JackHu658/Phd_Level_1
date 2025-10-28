# A Comparison between MOSFET based and CNTFET based Analog to Digital Converter

## 文献

> A. K. V. Vinod Sarma and S. Choudhary, "A Comparison between MOSFET based and CNTFET based Analog to Digital Converter," 2018 International Conference on Recent Innovations in Electrical, Electronics & Communication Engineering (ICRIEECE), Bhubaneswar, India, 2018

这篇文章通过仿真的方法（CNTFETs利用VerilogA模型），比较了相似工艺下的基于MOSFETs和CNTFETs的ADC，得出的结论是：基于CNTFETs的ADC在低电压下能够有更低的功耗，且在低分辨率下的DNL/INL更低，随着分辨率上升，二者非线性差别不大

文章结合MOSFETs和CNTFETs做了一个3 bit Flash ADC

注意：==碳纳米管的宽度无法通过改变来提高电流驱动能力，因为一旦生长完成，其直径就固定了——只能通过并联更多根碳纳米管==


![使用运放当作比较器](<Pictures/A Comparison between MOSFET based and CNTFET based Analog to Digital Converter-image.png>)
二级运放用来当比较器使用，开环下用——有缺点，速度上并不合适，有回滞，无法很快的跟随输入的变化而变——用动态比较器来替代是个好方法

![Flash ADC架构](<Pictures/A Comparison between MOSFET based and CNTFET based Analog to Digital Converter-image-1.png>)
![温度编码真值表](<Pictures/A Comparison between MOSFET based and CNTFET based Analog to Digital Converter-image-2.png>)
FLash ADC的架构和真值表如上所示，设计的要点是：比较器的设置和编码器的设置

![性能比较](<Pictures/A Comparison between MOSFET based and CNTFET based Analog to Digital Converter-image-3.png>)
从3Bit的ADC来看，硅基工艺基于90nm，碳基基于100nm，性能上碳基的功耗更低，线性度更好——但是碳基所用的Voltage更小，如何归一表示在文章中并未提及，==不如使用FoM来量化==
