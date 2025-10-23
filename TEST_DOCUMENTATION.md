# FB功能块测试文档

## 概述

本文档描述了TwinCAT PLC项目中功能块（FB）的测试程序和测试方法。

## 测试的功能块

### 1. FB_Counter（计数器功能块）

**功能描述：** 实现上/下计数功能，支持最小/最大值限制。

**输入参数：**
- `bEnable` (BOOL) - 使能计数器
- `bReset` (BOOL) - 重置计数器为0
- `bCountUp` (BOOL) - 上升沿触发加1
- `bCountDown` (BOOL) - 上升沿触发减1
- `iMin` (INT) - 最小值，默认0
- `iMax` (INT) - 最大值，默认100

**输出参数：**
- `iCount` (INT) - 当前计数值
- `bMinReached` (BOOL) - 到达最小值标志
- `bMaxReached` (BOOL) - 到达最大值标志
- `bError` (BOOL) - 错误标志
- `sErrorMsg` (STRING) - 错误信息

**测试用例：**
1. 重置功能测试 - 验证计数器能正确重置到0
2. 上计数测试 - 验证上升沿触发加1功能
3. 下计数测试 - 验证上升沿触发减1功能
4. 边界测试 - 验证最小/最大值限制功能
5. 错误处理 - 验证Min > Max时的错误处理

---

### 2. FB_CustomTimer（自定义定时器功能块）

**功能描述：** 实现定时器功能，支持启动/停止/重置，并提供进度百分比。

**输入参数：**
- `bStart` (BOOL) - 启动定时器
- `bStop` (BOOL) - 停止定时器
- `bReset` (BOOL) - 重置定时器
- `tSetTime` (TIME) - 设定时间，默认T#1S

**输出参数：**
- `bRunning` (BOOL) - 定时器运行中
- `bDone` (BOOL) - 定时完成
- `tElapsed` (TIME) - 已经过时间
- `tRemaining` (TIME) - 剩余时间
- `rProgress` (REAL) - 进度百分比（0-100）

**测试用例：**
1. 启动测试 - 验证定时器能正确启动
2. 完成测试 - 验证定时器在设定时间后完成
3. 进度测试 - 验证进度百分比计算准确
4. 停止测试 - 验证停止功能
5. 重置测试 - 验证重置功能

---

### 3. FB_StateMachine（状态机功能块）

**功能描述：** 实现典型的工业设备状态机（空闲-启动-运行-暂停-停止）。

**输入参数：**
- `bStart` (BOOL) - 启动命令
- `bStop` (BOOL) - 停止命令
- `bReset` (BOOL) - 重置命令
- `bPause` (BOOL) - 暂停命令
- `bEmergency` (BOOL) - 紧急停止

**输出参数：**
- `eState` (E_MachineState) - 当前状态
- `bBusy` (BOOL) - 设备忙标志
- `bError` (BOOL) - 错误标志
- `sStateText` (STRING) - 状态描述文本
- `iCycles` (INT) - 运行周期计数

**状态说明：**
- `Idle` (0) - 空闲状态，等待启动
- `Starting` (1) - 启动中，持续2秒
- `Running` (2) - 运行中
- `Paused` (3) - 暂停状态
- `Stopping` (4) - 停止中，持续1秒
- `Error` (5) - 错误状态
- `Emergency` (6) - 紧急停止状态

**测试用例：**
1. 初始状态测试 - 验证初始为Idle状态
2. 启动流程测试 - 验证Idle -> Starting -> Running转换
3. 停止流程测试 - 验证Running -> Stopping -> Idle转换
4. 暂停功能测试 - 验证Running -> Paused转换
5. 紧急停止测试 - 验证紧急停止功能
6. 重置功能测试 - 验证错误状态重置

---

## 测试程序说明

### 自动化测试模式

**使用方法：**
1. 在MAIN程序中设置 `bAutoTest := TRUE`
2. 设置 `bRunTests := TRUE` 开始测试
3. 观察 `fbTestRunner.stResults` 查看测试结果

**测试结果变量：**
```
fbTestRunner.stResults.sCurrentTest      // 当前测试名称
fbTestRunner.stResults.iTestsTotal       // 总测试数
fbTestRunner.stResults.iTestsPassed      // 通过数
fbTestRunner.stResults.iTestsFailed      // 失败数
fbTestRunner.stResults.bAllTestsPassed   // 全部通过标志
fbTestRunner.stResults.bCounterTest      // 计数器测试结果
fbTestRunner.stResults.bTimerTest        // 定时器测试结果
fbTestRunner.stResults.bStateMachineTest // 状态机测试结果
fbTestRunner.stResults.sLastError        // 最后错误信息
```

**测试流程：**
1. INIT - 初始化测试环境
2. TEST_COUNTER - 测试计数器FB
3. TEST_TIMER - 测试定时器FB
4. TEST_STATEMACHINE - 测试状态机FB
5. COMPLETE - 测试完成，显示结果

---

### 手动测试模式

**使用方法：**
1. 在MAIN程序中设置 `bManualTest := TRUE`
2. 通过在线模式修改控制变量
3. 观察FB输出变量验证功能

**计数器手动测试：**
```
控制变量：
bCounterEnable := TRUE;    // 使能计数器
bCountUp := TRUE;          // 触发上计数（脉冲）
bCountDown := TRUE;        // 触发下计数（脉冲）

观察变量：
fbCounter.iCount           // 当前计数值
fbCounter.bMaxReached      // 最大值标志
fbCounter.bMinReached      // 最小值标志
```

**定时器手动测试：**
```
控制变量：
bTimerStart := TRUE;       // 启动定时器
bTimerReset := TRUE;       // 重置定时器

观察变量：
fbTimer.tElapsed           // 已用时间
fbTimer.rProgress          // 进度百分比
fbTimer.bDone              // 完成标志
```

**状态机手动测试：**
```
控制变量：
bMachineStart := TRUE;     // 启动设备
bMachineStop := TRUE;      // 停止设备
bMachinePause := TRUE;     // 暂停设备
bMachineReset := TRUE;     // 重置设备

观察变量：
fbStateMachine.eState      // 当前状态
fbStateMachine.sStateText  // 状态文本
fbStateMachine.iCycles     // 周期计数
```

---

## 测试环境要求

- **TwinCAT版本：** 3.1.4026.11 或更高
- **PLC任务周期：** 10ms（可在PlcTask.TcTTO中配置）
- **必需库：**
  - Tc2_Standard
  - Tc2_System
  - Tc3_Module

---

## 测试执行步骤

### 1. 项目加载
1. 打开TwinCAT XAE（Visual Studio）
2. 打开项目 "TwinCAT Project16.sln"
3. 激活配置（Activate Configuration）

### 2. 运行自动化测试
1. 切换PLC到RUN模式
2. 在MAIN中设置 `bAutoTest := TRUE`
3. 设置 `bRunTests := TRUE`
4. 等待测试完成（约5秒）
5. 检查测试结果

**预期结果：**
- `iTestsTotal = 3`
- `iTestsPassed = 3`
- `iTestsFailed = 0`
- `bAllTestsPassed = TRUE`

### 3. 运行手动测试
1. 切换PLC到RUN模式
2. 在MAIN中设置 `bManualTest := TRUE`
3. 添加变量到Watch窗口
4. 通过修改控制变量进行交互测试
5. 观察输出变量验证功能

---

## 测试结果判定

### 通过标准
- 所有自动化测试通过（bAllTestsPassed = TRUE）
- 手动测试中所有功能按预期工作
- 无错误信息产生
- 所有边界条件处理正确

### 失败处理
如果测试失败：
1. 检查 `stResults.sLastError` 获取错误信息
2. 检查具体的测试标志（bCounterTest, bTimerTest, bStateMachineTest）
3. 切换到手动测试模式进行详细调试
4. 检查PLC任务是否正常运行
5. 验证库引用是否正确

---

## 常见问题

### Q1: 自动测试一直不完成
**A:** 检查PLC是否在RUN模式，任务周期是否正确配置（10ms）

### Q2: 定时器测试失败
**A:** 定时器依赖系统时钟，确保系统时间正确，任务周期稳定

### Q3: 状态机测试超时
**A:** 状态转换需要时间（Starting 2s, Stopping 1s），确保测试有足够等待时间

### Q4: 找不到数据类型
**A:** 确保项目中包含 E_MachineState.TcDUT 和 ST_TestResults.TcDUT 文件

---

## 文件结构

```
Untitled1/
├── POUs/
│   ├── MAIN.TcPOU               # 主程序（测试入口）
│   ├── FB_Counter.TcPOU         # 计数器功能块
│   ├── FB_CustomTimer.TcPOU     # 定时器功能块
│   ├── FB_StateMachine.TcPOU    # 状态机功能块
│   └── FB_TestRunner.TcPOU      # 自动化测试运行器
├── DUTs/
│   ├── E_MachineState.TcDUT     # 状态机枚举类型
│   └── ST_TestResults.TcDUT     # 测试结果结构体
├── PlcTask.TcTTO                # PLC任务配置
└── Untitled1.plcproj            # 项目文件
```

---

## 维护建议

1. **定期测试：** 每次修改FB后运行完整测试套件
2. **扩展测试：** 根据实际应用添加更多测试用例
3. **文档更新：** 功能变更时同步更新文档
4. **版本控制：** 使用Git跟踪代码和测试变更
5. **性能监控：** 监控FB的执行时间和资源占用

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| 1.0 | 2025-10-23 | 初始版本，包含3个FB和完整测试套件 |

---

## 联系信息

如有问题或建议，请联系项目维护团队。
