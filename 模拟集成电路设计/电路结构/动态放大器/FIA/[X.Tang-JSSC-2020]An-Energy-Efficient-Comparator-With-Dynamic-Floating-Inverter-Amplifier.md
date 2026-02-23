# An Energy-Efficient Comparator With Dynamic Floating Inverter Amplifier

!!! cite 文献
    X. Tang et al., "An Energy-Efficient Comparator With Dynamic Floating Inverter Amplifier," in IEEE Journal of Solid-State Circuits, vol. 55, no. 4, pp. 1011-1022, April 2020


## 1. 摘要 (Abstract)

本文针对低功耗高分辨率 SAR ADC 中比较器的核心瓶颈提出新结构：传统 StrongARM (SA) 动态比较器在低噪声设计时需要较大积分电容，存在不必要的全放电能耗，且性能（噪声、失调、延迟）对输入共模电压 $V_{CM}$ 和工艺角高度敏感。  

作者提出 **Dynamic Floating Inverter Amplifier (FIA)** 预放大器：以 floating reservoir capacitor ($C_{RES}$) 供电的 CMOS 反相器输入级，实现
- current reuse（$G_m$ 叠加）；
- dynamic bias（提升 $G_m/I_D$）；
- 无专用 CMFB 的输出共模稳定（隔离电源域）。

原型芯片（180 nm, 1.2 V）实测：
- 输入参考噪声：$46\,\mu V_{rms}$；
- 单次比较能耗：约 $0.98\,pJ$（$V_{CM}=0.6\,V,\ \Delta V_{in}=1\,mV$）；
- 相比 SA latch（约 $4.1\,pJ$）能效提升超过 $7\times$；
- 输入共模敏感性显著下降（噪声变化约降低 $4\times$，失调变化约降低 $>5\times$）。

## 2. 技术原理与理论推导 (Theoretical Analysis)

### 2.1 传统 SA 动态积分瓶颈

积分时间近似：
$$
T_{int}\approx \frac{C_X}{I_D}V_{THN}
$$

积分增益：
$$
A_{int}\approx \frac{g_m}{C_X}T_{int}\approx \frac{g_m}{I_D}V_{THN}
$$

输入参考噪声主导项：
$$
\sigma^2_{n,int}\approx \frac{I_D}{g_m}\frac{4kT\gamma}{V_{THN}C_X}
$$

总输入噪声（含锁存级折算）：
$$
\sigma^2_{n(os),in}\approx \sigma^2_{n(os),int}+\frac{\sigma^2_{n(os),latch}}{A_{int}^2}
$$

结论：传统 SA 中，积分节点共模下拉受限、$A_{int}$ 有上限，且 tail 管线性区导致 $I_D$ 强依赖 $V_{CM}$。

### 2.2 DB 积分与 FIA 的演进逻辑

DB（dynamically biased）通过尾电容抬升源极电位，降低过驱动，提升 $g_m/I_D$ 并避免 $C_X$ 全放电；FIA 在此基础上进一步引入 CMOS 输入对与浮置储能电容，兼顾能效与共模鲁棒性。

### 2.3 FIA 小信号与瞬态推导（论文核心）

在弱反型近似下：
$$
G_m(t)\approx 2\cdot \frac{I_D(t)}{nU_T}=\frac{I_{AMP}(t)}{nU_T}
$$

差分输出积分：
$$
\Delta V_{X,DM}(t)\approx \frac{\Delta V_{I,DM}}{nU_T C_X}\int_0^t I_{AMP}(\tau)\,d\tau
$$

尾电流衰减：
$$
I_{AMP}(t)=\frac{I_{AMP}(0^+)}{1+\dfrac{I_{AMP}(0^+)}{nU_T C_{TAIL}}t}
$$

源节点电压变化：
$$
\Delta V_S(t)=\frac{1}{2C_{RES}}\int_0^t I_{AMP}(\tau)\,d\tau
=nU_T\ln\!\left(1+\frac{I_{AMP}(0^+)}{2nU_T C_{RES}}t\right)
$$

预放增益：
$$
A_V(T_{INT})=\frac{\Delta V_{X,DM}(T_{INT})}{\Delta V_{I,DM}}
=\frac{2C_{RES}\Delta V_S(T_{INT})}{nC_XU_T}
$$

### 2.4 噪声推导与 FoM

动态积分通式：
$$
\sigma_o^2(t)=\frac{1}{2}\int_0^t S_i(t-\tau)\,|h_n(\tau)|^2\,d\tau
$$

FIA 输入参考噪声（积分结束）：
$$
\sigma^2_{in,FIA}(T_{INT})
=\frac{2nkT}{C_{RES}\Delta V_S(T_{INT})}\cdot \frac{I_D}{G_m}
$$

定义能效指标：
$$
\mathrm{FoM}=Energy\cdot (Noise\ Power)
$$

三类结构的 FoM：
$$
\mathrm{FoM}_{SA}= \frac{4nkT\,V_{DD}^2}{V_{THN}}\cdot\frac{I_D}{g_m}
$$
$$
\mathrm{FoM}_{DB}= 4nkT\,V_{DD}\cdot\frac{I_D}{g_m}
$$
$$
\mathrm{FoM}_{FIA}= 4nkT\,V_{DD}\cdot\frac{I_D}{G_m}
$$

相对提升：
$$
\frac{\mathrm{FoM}_{SA}}{\mathrm{FoM}_{FIA}}
=\frac{V_{DD}}{V_{THN}}\cdot
\frac{(G_m/I_D)_{FIA}}{(g_m/I_D)_{SA}}
$$

其物理含义：
- $V_{DD}/V_{THN}$：避免传统 SA 中 $C_X$ 深度无效放电；
- $G_m/I_D$ 提升：CMOS 输入对 current reuse + dynamic bias。

### 2.5 寄生电容对共模抑制的影响

当储能电容存在寄生 $C_P$ 时：
$$
I_{P+}(t)-I_{P-}(t)=I_{X,CM}(t)
$$
$$
\Delta V_{X,CM}(t)=-(\Delta V_{S+}(t)+\Delta V_{S-}(t))\frac{C_P}{C_X}
$$
$$
\Delta V_{X,CM}\approx -2\Delta V_{I,CM}\frac{C_P}{C_X}
$$

论文后仿显示在 $V_{CM}=0.4\sim 0.8\,V$ 变化下，输出共模变化小于 $100\,mV$，增益变化约在 $15\%$ 内。

## 3. 电路实现细节 (Implementation)

### 3.1 结构与时序

关键结构：`FIA 预放 + SA latch`。  
复位相（`clk=0`）：$C_{RES}$ 预充，积分节点复位到中间共模。  
比较相：FIA 先积分放大；SA 作再生判决；判决完成后关闭 FIA 以避免继续放电损耗。

### 3.2 关键设计参数与权衡

- $C_{RES}$ 太小：预放增益不足，后级锁存噪声折算上升；
- $C_{RES}$ 太大：dynamic bias 效果减弱、面积增大；
- 最终选型：$C_{RES}=2\,pF$（MoM 实现），积分节点等效 $C_X\approx 250\,fF$；
- 面向能效优先应用，偏置电流取较低值以换取更长积分/再生时间。

### 3.3 关键图表占位说明

- 图1（SA latch 经典结构）：展示传统动态比较器积分与再生耦合路径。  
- 图2（本文总电路）：FIA 作为前级，后接 SA latch。  
- 图4/6（行为模型与FIA原理）：对比 conventional/DB/FIA 的积分轨迹，突出共模稳定与 $G_m/I_D$ 提升。  
- 图8/9（寄生影响）：说明 $C_P$ 通过共模通道引入有限共模漂移。  
- 图10（时序波形）：显示 FIA 只做部分放电后交给 SA 再生，避免无效能耗。

## 4. 测试结果与性能对比 (Results)

### 4.1 测试方法

通过重复比较统计输出概率，拟合 Gaussian CDF 提取输入参考噪声；失调通过外部 DAC 微调至 50% 翻转概率估算。

### 4.2 关键实测结果

- 工艺与供电：180 nm CMOS，$V_{DD}=1.2\,V$；
- 输入参考噪声：SA 为 $62\,\mu V_{rms}$，FIA 为 $46\,\mu V_{rms}$；
- 能耗（$V_{CM}=0.6\,V,\ \Delta V_{in}=1\,mV$）：FIA 约 $0.98\,pJ/comparison$，SA 约 $4.1\,pJ/comparison$；
- 共模鲁棒性：  
  - 噪声随共模变化的波动，FIA 相比 SA 约降低 $4\times$；  
  - 10 颗样片失调随共模变化：SA 约 $5.3\,mV$，FIA 约 $0.9\,mV$（$>5\times$ 改善）。

### 4.3 与已有工作比较（按作者结论）

- 相比经典 SA：能效提升 $>7\times$；
- 相比此前最佳动态偏置类工作（文中对比 [12]）：总体能效提升 $>2.5\times$ 量级；
- 代价：为实现低噪声和储能电容，面积有一定上升（文中约 30% 额外开销，相对同噪声 SA 基线）。

## 5. 结论 (Conclusion)

本文的核心贡献是把 **current reuse + dynamic bias + floating supply domain** 三者统一到动态比较器预放结构中：  
1) 通过 FIA 提高 $G_m/I_D$ 并避免积分电容全放电，显著降低能耗；  
2) 通过浮置储能电容实现天然共模稳定，弱化 $V_{CM}$ 与工艺角扰动；  
3) 在低噪声目标下仍获得领先能效（$46\,\mu V$、约 $0.98\,pJ$、$>7\times$ SA 改善）。

对后续设计的启示：在超低功耗 ADC 中，比较器优化不应只停留在 latch 再生速度，而应优先围绕 **预放大阶段的能量利用效率、共模隔离机制和 $G_m/I_D$ 工程化提升** 展开。

