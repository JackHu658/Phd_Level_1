# A 90-MS/s 11-MHz-Bandwidth 62-dB SNDR Noise-Shaping SAR ADC


!!! cite 文献
    J. A. Fredenburg and M. P. Flynn, "A 90-MS/s 11-MHz-Bandwidth 62-dB SNDR Noise-Shaping SAR ADC," in IEEE Journal of Solid-State Circuits, vol. 47, no. 12, pp. 2898-2904, Dec. 2012, doi: 10.1109/JSSC.2012.2217874.


## 摘要
本文指出：虽然电荷再分配式SAR ADC在中等分辨率时具有很高效率，但比较器噪声等因素限制其在超过10 b ENOB时的能效。作者提出一种低OSR的噪声整形SAR架构，用8 b电容DAC实现10 b ENOB。噪声整形同时作用于量化噪声与比较器噪声，从而将比较器精度与ADC整体分辨率解耦。环路滤波器采用“两抽头电荷域FIR + 积分器(IIR)”级联，在低质量积分器条件下仍获得良好整形。65 nm原型芯片面积0.03 mm^2，90 MS/s下功耗806 μW。

## 引言
近年来SAR ADC以结构简单、功耗低在中等带宽与分辨率下表现突出，但当分辨率提高时，比较器噪声、DAC不匹配、采样噪声以及开关与寄生效应会显著降低效率。对高分辨率SAR而言，DAC步进电压变小，比较器输入噪声变得不可忽视，常需功耗较高的前置放大器。作者提出的核心思想是通过低OSR的噪声整形，把比较器噪声与量化噪声一并高通化，从而使比较器不必达到ADC整体分辨率要求，进而提升能效。该方法以较低分辨率DAC和较低精度电路换取更高有效分辨率。

## SAR ADC回顾与残余获取
SAR ADC先将输入采样到电容阵列，再通过逐次逼近完成二进制搜索。本工作采用符号-幅度编码并使用双极参考电压。采样后，各电容底板先置于共模电压，比较器首先判符号，随后逐位决定MSB到LSB。在传统SAR中，最后一位判决完成后不会再切换一次DAC，因此最终残余并不等于完整量化误差。以8 b为例，残余是输入与7 b估计的差。若在最后一位判决后再做一次DAC切换，残余才等于完整量化误差，同时最后一次比较器噪声也会留在残余中。

### 关键公式（对应文中(1)–(4)）
量化误差定义：
$$e[n] = x[n] - y[n]$$
传统N位SAR完成后残余：
$$r[n] = x[n] - \hat{x}_{N-1}[n]$$
加入“最后一次切换”后：
$$r[n] = x[n] - y[n] = e[n]$$
残余包含比较器噪声：
$$r[n] = e[n] + n_c[n]$$

## SAR中的噪声整形
作者依次给出三种结构：简单噪声整形、加入积分器的改进结构，以及可实现的FIR+IIR级联结构。核心演化思路是从“直接使用残余反馈”逐步过渡到“带有可控滤波特性的环路”。

### 简单噪声整形
将上一拍残余`r[n-1]`加到当前比较器输入负端，使当前量化过程的输入为`x[n]`与上一拍残余之和，形成一阶延迟反馈结构。其噪声传递函数为高通，但低频衰减仅约6 dB，整形强度不足。

### 推导（对应文中(5)–(7)）
量化器输入：
$$x[n] + r[n-1] + n_c[n]$$
量化器输出建模：
$$y[n] = x[n] + r[n-1] + n_c[n] + e[n]$$
代入`r[n-1] = x[n-1] - y[n-1] + n_c[n-1]`并整理：
$$y[n] + y[n-1] = x[n] + x[n-1] + e[n] + n_c[n] + n_c[n-1]$$
Z域：
$$Y(z)(1+z^{-1}) = X(z)(1+z^{-1}) + E(z) + N_c(z)(1+z^{-1})$$
得到：
$$Y(z)=X(z)+\frac{E(z)}{1+z^{-1}}+N_c(z)$$
STF=1，NTF=`1/(1+z^{-1})`。

### 改进噪声整形：加入积分器
在残余采样后加入积分器，使系统等效为一阶ΣΔ结构。理想情况下NTF为`1 - z^{-1}`，低频噪声衰减显著。但积分器实现需要电容采样与运放，采样电容上的KTC噪声不再被整形，电容不能过小，否则噪声成为瓶颈。作者用质量因子`\rho`描述有限增益与电荷共享损耗，当`\rho`下降时整形变弱，但即便`\rho=0.6`仍可维持一定效果。

### 推导（对应文中(8)）
积分器状态：
$$v[n] = v[n-1] + r[n-1]$$
量化输出：
$$y[n] = x[n] + v[n] + e[n]$$
整理后得到：
$$Y(z)=X(z)+(1-z^{-1})E(z)+(1-z^{-1})N_c(z)$$
因此STF=1，NTF=`1-z^{-1}`。

### 实用噪声整形：FIR + IIR级联
为在低质量积分器条件下获得更强整形，作者在积分器前加入两抽头FIR。FIR在电荷域实现，用两组电容交错采样残余，形成两抽头滤波后再送入IIR积分器。文中采用`b_1=3, b_2=1`，`\rho≈0.64`，使OSR=4时的分辨率提升接近三阶ΣΔ。

### 推导框架（对应文中(9)–(10)）
FIR：
$$u[n]=b_1 r[n]+b_2 r[n-1]$$
IIR：
$$v[n]=\rho v[n-1]+u[n]$$
量化输出：
$$y[n]=x[n]+v[n]+e[n]$$
令
$$H(z)=\frac{b_1+b_2 z^{-1}}{1-\rho z^{-1}}$$
则：
$$Y(z)=X(z)+\frac{E(z)}{1+H(z)}+\frac{H(z)}{1+H(z)}N_c(z)$$
STF=1，NTF=`1/(1+H(z))`。

## 电路实现与时序
整体采用全差分结构。比较器为双尾锁存式双差分结构。两抽头FIR由两组交错电容阵列实现，每个残余用于生成两个连续的FIR输出，需交错采样避免电荷被破坏。时序采用异步生成方式：90 MHz主时钟只定义采样时刻，内部用单一延迟单元循环产生DAC稳定时间，保证各bit延迟一致而无需校准。

## 原型与测量结果
65 nm工艺，核心面积0.03 mm^2，DAC为8 b二进制电容阵列。90 MS/s、OSR=4下带宽11 MHz，SNDR 62 dB，ENOB 10.0 b。功耗806 μW（数字608 μW，模拟198 μW），FOM 35.8 fJ/conv-step。频谱显示噪声底被整形推向高频，SNDR随输入频率与幅度变化保持合理一致性。

## 总结
本文提出的噪声整形SAR架构在低OSR条件下实现10 b ENOB，核心在于“残余提取 + FIR/IIR整形”，并将比较器噪声与量化噪声同时高通化。该方案保持SAR核心结构，仅增加少量电容与低增益放大器，适合工艺缩放，在带宽与分辨率之间取得高效折中。

# 图表逐一解释

## Fig.1 Basic operation of the SAR ADC
该图展示SAR ADC的基本流程：采样保持后，逐位比较并切换DAC，比较器输出驱动SAR逻辑完成二进制搜索。它强调“逐次逼近”过程与电容阵列顶板残余的逐步收敛。

## Fig.2 Residue voltage after 8-b conversion
该图说明传统8 b SAR完成后残余等于输入与7 b估计的差，原因是最后一位判决后并未再次切换DAC，因此残余不是完整量化误差。

## Fig.3 One extra switching based on final decision
该图展示加入“最后一次DAC切换”后残余变为完整量化误差，从而使残余可直接表示`e[n]`并用于噪声整形。

## Fig.4 Final residue captures comparator noise
该图表明最后一次比较器噪声会留在残余中，因此残余不仅包含量化误差，也包含比较器输入等效噪声，这为后续整形比较器噪声提供基础。

## Fig.5 Simple noise-shaping SAR technique
该图给出最简单的噪声整形方式：把上一拍残余加到当前比较器负端，形成延迟反馈。

## Fig.6 Functional representation and signal flow
该图用功能框图与信号流图表达“输入前馈 + 残余延迟反馈”的结构，便于推导STF/NTF。

## Fig.7 NTF of simple noise shaping
该图显示简单结构的噪声传递函数在低频只有约6 dB衰减，说明整形力度有限。

## Fig.8 Improved noise-shaping SAR with integrator
该图展示在残余采样后加入积分器的改进结构，使系统等效为一阶ΣΔ。

## Fig.9 NTF of improved structure
该图显示改进结构的NTF为一阶高通，低频噪声显著降低。

## Fig.10 Simplified depiction with sampling capacitor and integrator
该图突出残余采样电容与积分器电容、寄生电容的关系，说明电荷共享与有限增益对整形的影响。

## Fig.11 Model of residue processing
该图用质量因子`\rho`刻画积分器损耗与非理想效应，便于分析NTF随`\rho`变化。

## Fig.12 NTF for three values of rho
该图表明`\rho`越低整形越弱，高通拐点上移，但仍保持一定噪声抑制。

## Fig.13 Noise shaping with cascaded FIR/IIR
该图给出FIR+IIR级联结构，用两抽头FIR增强整形，再通过IIR积分器累积误差。

## Fig.14 Circuit implementation of FIR-IIR filter
该图展示电路级实现：两组电容阵列实现FIR抽头，单级放大器与反馈电容构成IIR积分器。

## Fig.15 Capacitor banks in FIR filter
该图展示两组交错电容阵列，抽头系数由电容比决定。

## Fig.16 Interleaved FIR operation
该图说明交错采样时序：每个残余需要贡献到连续两个FIR输出，因此用交错电容避免电荷被破坏。

## Fig.17 NTF: IIR only vs FIR+IIR
该图对比仅IIR与FIR+IIR在低`\rho`条件下的整形效果，FIR+IIR明显更强。

## Fig.18 NTF comparison to ideal sigma-delta
该图显示OSR=4时该结构的分辨率提升接近三阶ΣΔ调制器的效果。

## Fig.19 Comparator
该图为比较器电路，采用双尾锁存式双差分结构以兼顾速度与低功耗。

## Fig.20 Clock generation
该图展示异步时序生成方式，单一延迟单元循环用于DAC稳定时间，保证bit间延迟一致。

## Fig.21 Die photograph
该图展示芯片版图，核心面积0.03 mm^2，去耦电容占比显著。

## Fig.22 Measured spectral density
该图为2 MHz输入下的输出频谱，噪声底被整形推向高频。

## Fig.23 SNDR versus input frequency and amplitude
该图展示SNDR随输入频率与幅度变化的趋势，验证带宽与线性区间。

## Table I Comparison table
该表对比了同期SAR/NS-SAR工作，突出本工作在FOM与“8 b DAC实现10 b ENOB”上的优势。
