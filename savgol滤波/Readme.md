# Savgol滤波

## 最终结果

英文结果

https://wreng0b1qw.space.minimax.io/

中文结果

https://jg4p7g4ndq.space.minimax.io/

感觉不太行 功能有挺多瑕疵




## 动机

因为实际工作中我们用到了这个滤波

我们要向别人解释Savitzky-Golay滤波的具体原理

因为是第一次使用minimax-agent

我这边打算英文的也试验一下

## 中文prompt

```
我希望建立一个中文页面，来向非技术人员来解释如何对一个信号利用Savitzky-Golay filter来对每个时刻的信号进行求导

我的问题具体是这样的

<problem>
我有用户的每天的上线情况A_t，是离散的0，1信号，比如

A_t = [5个1, 2个0, 5个1, 2个0, 6个1 , 6个0]

实际过程中，我先使用一个三角滤波

h[t] = 1 - |t|/15 (t 从-15到15)

对数据进行了滤波 得到了p[t]

然后，对p[t]进行了savgol滤波， win_size = 21, order = 3

也就是对每个t时刻的-10到10的信号进行3次多项式回归，然后再进行3次多项式拟合，得到了p，进一步得到了dp_dt[t]
</problem>

我希望建立一个页面以我们的数据形式，来讲解Savitzky-Golay filter滤波

- 用一个合理的交互可视化，展示A_t,p_t和dp_dt
- 能够在t时刻拖动，展现参于savgol滤波的数据点、以及滤波后的多项式、显示
- 可以重新生成数据，一般来说A_t 都是0，1数据，并且会是几个(3-15)1，之后跟几个0(2个）在尾端用户有50%概率离开游戏（跟10个0）
- 可以增加一些Savitzky-Golay filter的原理、历史讲解

```

### 后续修改

```
我发现在交互式可视化的页面

- 调整当前时刻的时候 当前时刻SG滤波窗口详细分析 这里 的数据并没有被更新，所有的窗口内数据点都是0.这里需要检查调试之后修复

- 速度用曲线表示并不是很明显，这里我们建议在每个时刻t
使用一个x = t, y从p_t 到 p_t + 3 * dp_dt[t]的曲线来表示，这样能够更直观的看到速度， 可以用红色来标记下降，绿色bar来表示上升
```

## 英文prompt

```
### 合并后的需求描述（中文）：

我希望建立一个交互式中文页面，向非技术人员解释如何利用Savitzky-Golay滤波器对信号进行实时求导。具体需求如下：

1. **数据背景**：
   - 原始数据是离散的0/1信号A_t（例如用户每日上线记录：[5个1, 2个0, 5个1, 2个0, 6个1, 6个0]）
   - 处理流程：
     - 先用三角滤波器 h[t] = 1 - |t|/15 (t∈[-15,15]) 生成平滑信号p[t]
     - 再对p[t]应用SG滤波器（窗口=21，阶数=3），即对每个时刻t的[-10,10]区间进行三次多项式拟合得到导数dp_dt[t]

2. **可视化要求**：
   - 同时展示A_t原始信号、p_t平滑信号和dp_dt导数信号
   - 支持时间轴拖动交互：
     - 实时显示当前时刻t的SG滤波窗口内的数据点
     - 可视化拟合多项式曲线
     - 用动态彩色条形图（红色下降/绿色上升）显示导数强度：从p_t到p_t + 3*dp_dt[t]的垂直线段
   - 提供数据重新生成功能（生成3-15个1后跟随2个0的随机序列，末端有50%概率追加10个0）

3. **教学内容**：
   - SG滤波器原理和历史背景的通俗讲解
   - 当前需修复的BUG：
     - 时间轴拖动时SG窗口数据未实时更新问题
     - 优化导数可视化方案（弃用曲线改用彩色动态条形图）

---

### English Version:

I want to create an interactive 
 webpage to explain how to perform real-time differentiation of signals using the Savitzky-Golay filter to non-technical audiences. Key requirements:

1. **Data Context**:
   - Raw data is discrete 0/1 signal A_t (e.g., daily user login records like [five 1s, two 0s, five 1s, two 0s, six 1s, six 0s])
   - Processing pipeline:
     - First apply triangular filter h[t] = 1 - |t|/15 (t∈[-15,15]) to generate smoothed signal p[t]
     - Then apply SG filter (window=21, order=3) - performing cubic polynomial fitting on [-10,10] interval at each timestep t to obtain derivative dp_dt[t]

2. **Visualization**:
   - Simultaneous display of:
     - Raw A_t signal
     - Smoothed p_t signal
     - Derivative dp_dt signal
   - Interactive timeline with:
     - Real-time display of data points within SG filter window
     - Visualization of fitted polynomial curve
     - Color-coded dynamic bars (red=decreasing/green=increasing) showing derivative intensity as vertical lines from p_t to p_t + 3*dp_dt[t]
   - Data regeneration function (random sequences with 3-15 ones followed by two zeros, with 50% probability of appending ten zeros)

3. **Educational Content**:
   - Beginner-friendly explanations of SG filter principles and history
   - Current bug fixes needed:
     - Real-time update issue of SG window data during timeline scrubbing
     - Derivative visualization optimization (replacing curves with color-coded dynamic bars) 
```