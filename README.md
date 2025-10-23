# TwinCAT3 经典功能块合集

这是一个包含常用TwinCAT3功能块(FB)的合集，适用于工业自动化项目。

## 功能块列表

### 1. FB_Blink - 闪烁功能块
- 功能：生成可配置的闪烁信号
- 输入：
  - bEnable: 启用闪烁
  - tOnTime: 高电平持续时间
  - tOffTime: 低电平持续时间
- 输出：
  - bOutput: 闪烁输出信号

### 2. FB_Counter - 计数器功能块
- 功能：可上下计数，支持复位和限值检测
- 输入：
  - bCountUp: 上升沿计数
  - bCountDown: 下降沿计数
  - bReset: 复位计数器
  - iMaxValue: 最大计数值
  - iMinValue: 最小计数值
- 输出：
  - iCount: 当前计数值
  - bMaxReached: 达到最大值标志
  - bMinReached: 达到最小值标志

### 3. FB_MotorControl - 电机控制功能块
- 功能：支持启停、正反转、急停、延时启动
- 输入：
  - bStart: 启动命令
  - bStop: 停止命令
  - bEmergencyStop: 急停命令
  - bForward: 正转命令
  - bReverse: 反转命令
  - tStartDelay: 启动延时
- 输出：
  - bMotorRunning: 电机运行状态
  - bMotorForward: 正转状态
  - bMotorReverse: 反转状态
  - bError: 故障状态
  - sErrorMsg: 故障信息

### 4. FB_Alarm - 报警处理功能块
- 功能：报警触发、确认、计数、时间戳
- 输入：
  - bTrigger: 报警触发信号
  - bAcknowledge: 报警确认信号
  - sAlarmText: 报警文本
  - iPriority: 报警优先级(1=低, 2=中, 3=高)
- 输出：
  - bAlarmActive: 报警激活
  - bAlarmPending: 报警待确认
  - dtAlarmTime: 报警时间
  - iAlarmCount: 报警次数

### 5. FB_Average - 平均值计算功能块
- 功能：计算指定样本数的移动平均值
- 输入：
  - rInput: 输入值
  - iSamples: 采样数量(1-100)
  - bReset: 复位
- 输出：
  - rAverage: 平均值输出
  - bReady: 数据就绪(已采集足够样本)

### 6. FB_PID - PID控制器功能块
- 功能：比例-积分-微分控制
- 输入：
  - bEnable: 使能控制
  - rSetpoint: 设定值
  - rActual: 实际值
  - rKp: 比例系数
  - rKi: 积分系数
  - rKd: 微分系数
  - rOutMax: 输出上限
  - rOutMin: 输出下限
  - tCycle: 控制周期
  - bReset: 复位
- 输出：
  - rOutput: PID输出
  - rError: 当前误差

### 7. FB_Hysteresis - 滞后功能块
- 功能：实现带滞后的比较器，防止抖动
- 输入：
  - rInput: 输入值
  - rUpperLimit: 上限阈值
  - rLowerLimit: 下限阈值
  - bReset: 复位
- 输出：
  - bOutput: 滞后输出

### 8. FB_ScaleValue - 数值缩放功能块
- 功能：将输入值从一个范围线性缩放到另一个范围
- 输入：
  - rInput: 输入值
  - rInputMin: 输入范围最小值
  - rInputMax: 输入范围最大值
  - rOutputMin: 输出范围最小值
  - rOutputMax: 输出范围最大值
  - bLimitOutput: 是否限制输出
- 输出：
  - rOutput: 缩放后的输出值
  - bError: 错误标志

### 9. FB_Filter - 一阶低通滤波器功能块
- 功能：使用指数移动平均实现平滑滤波
- 输入：
  - rInput: 输入值
  - rFilterTime: 滤波时间常数(秒)
  - tCycleTime: 扫描周期
  - bReset: 复位
- 输出：
  - rOutput: 滤波后的输出

### 10. FB_EdgeDetection - 边沿检测功能块
- 功能：检测上升沿、下降沿及边沿计数
- 输入：
  - bInput: 输入信号
  - bReset: 复位
- 输出：
  - bRisingEdge: 上升沿输出
  - bFallingEdge: 下降沿输出
  - bAnyEdge: 任意边沿输出
  - iEdgeCount: 边沿计数

## 使用说明

1. 在TwinCAT3中打开项目文件 `TwinCAT Project16.sln`
2. 所有功能块位于 `Untitled1/POUs/` 目录下
3. `MAIN.TcPOU` 包含了所有功能块的使用示例
4. 直接复制所需的功能块到你的项目中使用

## 项目结构

```
/
├── Untitled1/
│   ├── POUs/
│   │   ├── MAIN.TcPOU              # 主程序(包含使用示例)
│   │   ├── FB_Blink.TcPOU          # 闪烁功能块
│   │   ├── FB_Counter.TcPOU        # 计数器功能块
│   │   ├── FB_MotorControl.TcPOU   # 电机控制功能块
│   │   ├── FB_Alarm.TcPOU          # 报警处理功能块
│   │   ├── FB_Average.TcPOU        # 平均值计算功能块
│   │   ├── FB_PID.TcPOU            # PID控制器功能块
│   │   ├── FB_Hysteresis.TcPOU     # 滞后功能块
│   │   ├── FB_ScaleValue.TcPOU     # 数值缩放功能块
│   │   ├── FB_Filter.TcPOU         # 滤波器功能块
│   │   └── FB_EdgeDetection.TcPOU  # 边沿检测功能块
│   └── Untitled1.plcproj           # PLC项目文件
└── TwinCAT Project16.sln           # 解决方案文件

```

## 依赖库

- Tc2_Standard (Beckhoff标准库)
- Tc2_System (Beckhoff系统库)
- Tc3_Module (Beckhoff模块库)

## 许可

本项目为开源项目，可自由使用和修改。

## 作者

生成于 TwinCAT3 经典功能块合集项目
