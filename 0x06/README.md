# Fuzzing

## 实验过程

模糊测试 （fuzz testing, fuzzing）是一种软件测试技术。其核心思想是自动或半自动的生成随机数据输入到一个程序中，并监视程序异常，如崩溃，断言(assertion)失败，以发现可能的程序错误，比如内存泄漏。模糊测试常常用于检测软件或计算机系统的安全漏洞。

#### 用例类别

- 缓冲区溢出类测试用例：超长字符串。比如几时上百个a
- 随机数测试用例：很多系统支持的配置值是固定的，比如屏幕只支持1080p我们故意设1081p系统就可能把错了。负数，浮点数，超大数等分别来个测试用率
- 格式化字符串测试用例：%d、%s等符号在很多语言中是指导格式化用的，如果用做做为输入可能引发报错。长长短短随便来几个测试用例就行。
- 特殊字符测试用例：~!@#$%等等符号在很多语言中是有特殊含义的，作为输入可能会引发报错。最好每个字符及不同长度都来一个测试用例
- unicode编码测试用例：有些程序是不支持unicode的，输入unicode可能会引发报错。%uxxxx等长短不一来几个测试用例