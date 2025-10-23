# FB测试程序 - 快速开始指南

## 5分钟快速测试

### 步骤1: 打开项目
```
1. 打开 TwinCAT XAE (Visual Studio)
2. 打开文件: TwinCAT Project16.sln
3. 等待项目加载完成
```

### 步骤2: 激活配置
```
1. 右键点击 "SYSTEM" 节点
2. 选择 "Activate Configuration"
3. 确认激活
4. 等待TwinCAT进入RUN模式
```

### 步骤3: 启动PLC
```
1. 右键点击 "Untitled1 (PLC)" 节点
2. 选择 "Login"
3. 点击 "Start" 按钮
4. PLC进入RUN模式
```

### 步骤4: 运行自动化测试
```
1. 打开 POUs/MAIN.TcPOU
2. 找到变量 bRunTests
3. 在线模式下设置: bRunTests := TRUE
4. 等待5秒
```

### 步骤5: 查看测试结果
在Watch窗口添加以下变量：
```
MAIN.fbTestRunner.stResults.sCurrentTest
MAIN.fbTestRunner.stResults.iTestsPassed
MAIN.fbTestRunner.stResults.iTestsFailed
MAIN.fbTestRunner.stResults.bAllTestsPassed
```

**预期结果：**
- sCurrentTest = "Tests Complete"
- iTestsPassed = 3
- iTestsFailed = 0
- bAllTestsPassed = TRUE

---

## 手动测试示例

### 测试计数器
```
1. 设置: MAIN.bManualTest := TRUE
2. 设置: MAIN.bCounterEnable := TRUE
3. 反复切换: MAIN.bCountUp := TRUE/FALSE
4. 观察: MAIN.fbCounter.iCount 增加
```

### 测试定时器
```
1. 设置: MAIN.bTimerStart := TRUE
2. 观察: MAIN.fbTimer.rProgress 从0到100
3. 观察: MAIN.fbTimer.bDone 变为TRUE
```

### 测试状态机
```
1. 设置: MAIN.bMachineStart := TRUE
2. 观察状态变化: Idle -> Starting -> Running
3. 设置: MAIN.bMachineStop := TRUE
4. 观察状态变化: Running -> Stopping -> Idle
```

---

## 故障排除

### 问题: PLC无法启动
**解决:** 检查TwinCAT是否在运行，是否有其他程序占用端口

### 问题: 测试不运行
**解决:** 确认 bAutoTest = TRUE 且 bRunTests = TRUE

### 问题: 编译错误
**解决:**
1. 检查所有文件是否在项目中
2. 重新生成（Rebuild）项目
3. 检查库引用是否正确

---

## 下一步

详细信息请参考：
- **TEST_DOCUMENTATION.md** - 完整测试文档
- **POUs/MAIN.TcPOU** - 主程序源码
- **POUs/FB_*.TcPOU** - 各功能块源码

享受测试！
