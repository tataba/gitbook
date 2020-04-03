# 5.数据域定义

### 5.1请求与答复
信息(Message)可分为请求(Request)和答复(Response)两种，请求是指从上位机到控制器(LED Controller)的信息，答复是指从控制器到上位机的回复。所有的数据通讯必须由上位机来发起。通讯过程中广播通讯控制器不回复，点对点通讯时可配置控制器是否回复，配置为不回复的通讯必须采用单包模式（参考单包发送和分包发送）。配置为有回复的通讯，如果在超时时间(TimeoutValue)之内没有收到回复，上位机将
产生超时错误(Timeout Error)。


### 5.1.1请求信息
请求信息是PC机软件到控制器的信息，其格式如下：

| 参数     | 数据长度 | 默认值 | 描述                                                         |
| -------- | -------- | ------ | ------------------------------------------------------------ |
| CmdGroup | 1        |        | 命令分组编号                                                 |
| Cmd      | 1        |        | 命令编号                                                     |
| Response | 1        |        | 是否要求控制器回复。0x01——控制器必须回复，0x02——控制器不必回复。 |
| Reserved | 2        | 0      | 保留                                                         |
| Data     | N        |        | 发送的数据                                                   |

#### 示例：

卡型号6K3,128*32屏，手动换行，多行显示，显示内容：“欢迎光临”，字体大小16X16

a5 a5 a5 a5 a5 a5 a5 a5 fe ff 00 80 00 00 00 00 00 00 fe 02 2c 00 **a3 06 01 00 00 00 01 23 00 00 00 80 00 00 80 80 20 00 00 00 00 0a 00 00 00 00 02 01 01 00 02 0a 08 00 00 00 bb b6 d3 ad b9 e2 c1 d9 3c** 49 5a

| 参数           | 数据                    | 描述                        |
| -------------- | ----------------------- | --------------------------- |
| 命令分组       | A3                      | 动态区更新命令              |
| 命令编号       | 06                      | 动态区更新命令              |
| 控制器是否回复 | 01                      | 必需要回复                  |
| 保留字节       | 00 00                   | 2个保留字节                 |
| 删除区域个数   | 00                      | 删除区域                    |
| 更新区域个数   | 01                      | 更新区域                    |
| 区域数据长度   | 00 23                   | 区域数据长度                |
| 区域类型       | 00                      | 区域类型                    |
| X坐标          | 8000                    | x轴坐标0（像素为单位）      |
| Y坐标          | 0000                    | y轴坐标0（像素为单位）      |
| 区域宽度       | 8080                    | 区域宽度为128（像素为单位） |
| 区域高度       | 0020                    | 区域高度为32（像素为单位）  |
| 动态区编号     | 00                      | 动态区编号0                 |
| 行间距         | 00                      | 行间距为0                   |
| 动态区运行模式 | 00                      | 普通模式                    |
| 动态区超时时间 | 00 0A                   | 超时时间10秒                |
| 是否使能语音   | 00                      | 不使能                      |
| 扩展位个数     | 00                      | 扩展位个数                  |
| 字体对齐方式   | 00                      | 左对齐                      |
| 是否单行显示   | 02                      | 多行显示                    |
| 是否自动换行   | 01                      | 手动换行                    |
| 显示方式       | 01                      | 静止显示                    |
| 退出方式       | 00                      | 自动循环显示                |
| 显示速度       | 02                      | 速度02                      |
| 停留时间       | 0A                      | 停留时间10秒                |
| 显示数据长度   | 00000008                | 数据长度8个字节             |
| 显示数据       | BB B6 D3 AD B9 E2 C1 D9 | 显示内容为欢迎光临          |

### 5.1.2 答复信息

答复信息为控制器到 PC 机软件的信息，其格式如下：

| 参数     | 数据长度 | 默认值 | 描述         |
| -------- | -------- | ------ | ------------ |
| CmdGroup | 1        |        | 命令分组编号 |
| Cmd      | 1        |        | 命令编号     |
| CmdError | 1        |        | 命令处理状态 |
| Reserved | 2        | 0      | 保留         |
| Data     | N        |        | 发送的数据   |

#### 示例：

卡型号6K3,128*32屏，手动换行，多行显示，显示内容：“欢迎光临”，字体大小16X16，正确显示后的回复信息

A5 A5 A5 A5 A5 A5 A5 A5 A5 00 80 FE FF 00 00 00 00 00 00 FE 02 05 00 **A0 00 00 00 00 14 AC** 5A

| 参数           | 数据  | 描述         |
| -------------- | ----- | ------------ |
| 命令分组       | A0    | 命令分组编号 |
| 命令编号       | 00    | 命令编号     |
| 控制器是否回复 | 00    | 命令处理状态 |
| 保留字节       | 00 00 | 2个保留字节  |
| CRC检验        | 14 AC | CRC检验      |

### 5.1.3单包发送和分包发送
由于控制器数据接收缓存为 1024byte，所以当数据域数据大小大于1024byte时采用分包发送，小于1024byte时采用单包发送。采用分包发送时控制器可以不回复，但是这种做法不推荐，因为无法保证数据的正确接收。无论采用单包发送还是分包发送，在写文件命令前，必须先发送开始写文件命令（参考写文件命令8.1.2节）。