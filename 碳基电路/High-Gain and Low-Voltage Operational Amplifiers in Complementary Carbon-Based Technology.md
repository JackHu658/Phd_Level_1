# High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology

简单摘要：利用碳纳米管做放大器，增益超过了1000（60dB），是除了硅基之外通过其他材料做放大器能够实现增益最高的。这款放大器在电压低至0.4V依旧能够保证线性工作。除此之外，文章还全面表证了碳纳米管放大器的性能，探索了其在模拟电路领域的潜力。CNT的P-type使用$Y_2O_3$作为钝化层，N-type利用$Y_2O_3/AlN/Al_2O_3$作为钝化层。

## Results and Discussion

### CNT-FETs的制造和性能表现

![碳管的版图](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image.png>)

如上图(a)和(b)所示，PMOS和NMOS制造在同一块衬底上，都使用底栅工艺，b图是其光学显微镜下的结构图（$\frac{W}{L}=\frac{30\mu m }{4 \mu m}$）
该器件是静电掺杂制造的

![碳管的转移曲线和输出曲线](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-1.png>)

图(c)和(d)展示了24个碳管CMOS的特性转移曲线和输出特性曲线，即使在大的偏压下，也表现出了较好的一致性和不错的开关比($>10^4$)
当一致性不满足的时候，会对OPA电路性能造成很大的损失，尤其是对CMRR

![阈值电压、跨导、转移曲线性能一致性测试](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-2.png>)

图(e)和(f)是对FETs的性能一致性的再次测试（在图(f)中维持$V_{DS}=\pm 2 V$），可以看出制造出来的晶体管的阈值电压和跨导能保持较好的一致性和稳定性，转移曲线也能保持很好的可重复性和稳定性

### CNT电流镜和本征增益

![电流镜随VDD和VB变化的影响](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-3.png>)

如上图(a),(b)所示，用CNT做电流镜（M5：$\frac{W}{L}=\frac{30\mu m }{4 \mu m}$，M8：$\frac{W}{L}=\frac{80\mu m }{4 \mu m}$），并观察其随着VDD和VB的变化影响，其中偏压VB的改变用来模拟现实中温度的变化，根据结果可知，电流镜基本能保持稳定，且稳定值接近理论值

![与硅比较本征增益](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-4.png>)

图(c),(d)将CNT-FETs与商用硅基晶体管进行了比较，发型其在饱和区更平缓，意味着更大的输出阻抗，在本征增益对比中也显现出较大的潜力

### CNT OPAs 性能

![OPA电路与版图](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-5.png>)

如图(a),(b)所示，OPA用经典两级结构实现，并用光学显微镜展示了其版图

![DC增益](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-6.png>)

图(c),(d)展示了其DC增益（感觉类似比较器了），能接近轨到轨，在Supply Voltage电压为$\pm 1V$和$\pm 0.4V$时都有很高的直流增益，且在$\pm 0.2V$的时候，也有着4.3的AC增益

![OPA测试重复性](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-7.png>)

图(e)是对OPA重复测试65次，展现出了较好的一致性，图(f)改变Vin-，展示出其较宽的工作范围

![差分增益](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-8.png>)

图(a),(b)展示了求差分增益的过程，测出的差分增益为46dB(@100 Hz)

![共模抑制比](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-9.png>)

图(c),(d)展示了CMRR的测试过程，输出下降了14dB，得到的CMRR=60dB(@100 Hz)

![AC性能](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-10.png>)

图(e),(f)表示用$ 10mV $ peak-to-peak 电压测的AC性能：最大增益：52dB，单位增益：56.1 kHz，相位裕度：$36^\circ$，且在连续的时间内保持AC增益的稳定性

在高频处电路增益的下降是受限于在工作时，电流需要对每个节点充电，寄生电容的影响越来越重要。相位裕度连45度都不到，需要改进（减小栅长提高频率推远次极点；Miller Compensation）

![增益、速度与其他材料比较](<Pictures/High-Gain and Low-Voltage Operational Amplifiers in Complementary Carbon-Based Technology-image-11.png>)

图(a),(b)将CNT-OPAs与其他不同的材料进行比较，发现增益是最高的（因为较高的本征增益），但速度不是最突出的，可能是因为制造的CNT-FETs的重叠有很多（寄生电容）和工艺制造的限制性（工艺不成熟）
