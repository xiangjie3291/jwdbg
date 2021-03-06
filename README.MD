# jwdbg

1821教务系列命令行评测（民间）

**自动化测试；任何人都可以贡献测试数据！** [指南](#贡献测试数据)

**软件在活跃开发中，欢迎试用并提交bug和建议！**

## 附带数据
- [jw05数据详情](./jw05-readme.md)
- jw4数据（from crclz）`jw04.yml`
- jw4数据（from于浩淼）`jw04-samshui.yml`

## 功能特点
- 能够测试程序能否输出指定内容
- 如果不能输出指定内容，会给出**期望输出与实际输出的比较**，并提供**直到出错命令**的输入数据进行检验，**可以方便用于调试**。如果用传统方式（in.txt，out.txt，对比程序输出和out.txt），无法实现精确定位到某条指令。[说明](#阅读测试结果)
- 以yaml的方式书写测试数据，可读性高，易于扩展。[查看示例](./jw04.yml)

## 前提条件
- python`3.7+`
- 需要pyyaml模块 `pip install pyyaml`

## 傻瓜式运行测试（以jw4为例）
1. 拷贝`jwdbg.py`和`jw04.yml`(或者jw04-samshui.yml)到java输出目录（一堆.class那个目录下）
2. 命令行进入那个目录，运行`python .\jwdbg.py -cmd "java Test" -case jw04.yml`

## 运行测试
`python .\jwdbg.py -cmd "java Test" -case jw04.yml`

`-case`：测试用例文件

`-cmd`: 运行目标程序的命令。

**注意！** 建议在java文件的生成目录下运行。即：以java生成目录为工作目录运行本测试程序。因为要照顾到java，因为java的classpath依赖于当前目录的位置。

所以，建议把这个测试程序拷贝到java生成目录（一堆.class）里去。或者使用绝对路径来运行此程序和传递参数。

`-inputdata`: 附带此参数，程序的行为是：在控制台输出完整的测试输入。

## 阅读测试结果
如果测试失败，程序会：
1. 打印出发生不一致的行的对比。
2. 给出输入数据，便于用IDE来测试。例如，一共有20行输出，对应20条命令。程序在第15行输出的时候发生了不一致，那么就会提供第1-15行命令。
3. 如果程序Timeout，请看[常见问题](#常见问题)

## 获取完整的测试输入数据
`python .\jwdbg.py -cmd "java Test" -case .\jw05.yml -inputdata`

说明：虽然`-cmd "java Test"`没什么意义，但是为了编程方便起见，还是要求必须有。

## 常见问题
1. 超时异常：说明你的程序没有退出。要么是测试数据没有给出退出信号（例如QUIT），要么是你的程序有bug。请[获取完整的测试输入数据](#获取完整的测试输入数据)并去IDE测试。


## Bug与建议
欢迎提交bug和建议到issue区。bug请尽量附带详细信息。

## 贡献测试数据
可以用issue的方式贡献数据，也可以用fork+PullRequest的方式。

测试数据以yaml书写。yaml和json的区别是：yaml对人类读写友好。

测试数据的结构，用json来表示，则是：
```jsonc
{
    "name": "my test data",
    "data": [
        "SUDO",// 单行输入，无输出
        ["input1","output1"],// 单行输入，单行输出
        ["input2","output2\naaaaaaaa"],// 单行输入，多行输出
    ]
}
```
用yaml来表示，则是：
```yaml
name: my test data

data:
  - SUDO # 单行输入，无输出
  -
    - input1 # 单行输入，单行输出
    - output1
  - 
    - input2 # 单行输入，多行输出
    - |
      output2
      aaaaaaaa
    # 这是yaml对多行字符串的友好支持
```

具体可以参考`jw04.yml`的内容。

附上yaml一般用法（部分）：
```yaml
data: # data:[a, b, [c,d] ]
  - a
  - b
  -   # 数组嵌套
    - c
    - d
```

