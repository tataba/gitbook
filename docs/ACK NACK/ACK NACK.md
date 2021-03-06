# 7.ACK和NACK

ACK 和 NACK 常用于不需要返回额外数据的命令的回复，比如PING命令时，控制器返回ACK，表明控制器在线。

### 7.1 ACK
| 参数     | 数据长度 | 默认值 | 描述         |
| -------- | -------- | ------ | ------------ |
| CmdGroup | 1        | 0xA0   | 命令分组编号 |
| Cmd      | 1        | 0x00   | 命令编号     |
| CmdError | 1        |        | 命令处理状态 |
| Reserved | 2        | 0      | 保留         |

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

### 7.2 NACK
| 参数     | 数据长度 | 默认值 | 描述         |
| -------- | -------- | ------ | ------------ |
| CmdGroup | 1        | 0xA0   | 命令分组编号 |
| Cmd      | 1        | 0x01   | 命令编号     |
| CmdError | 1        |        | 命令处理状态 |
| Reserved | 2        | 0      | 保留         |

#### 示例：

卡型号6K3,128*32屏，手动换行，多行显示，显示内容：“欢迎光临”，字体大小16X16，未正确显示后的回复信息

A5 A5 A5 A5 A5 A5 A5 A5 A5 00 80 FE FF 00 00 00 00 00 00 FE 02 05 00 **A0 01 00 00 00 15 50** 5A

| 参数           | 数据  | 描述         |
| -------------- | ----- | ------------ |
| 命令分组       | A0    | 命令分组编号 |
| 命令编号       | 01    | 命令编号     |
| 控制器是否回复 | 00    | 命令处理状态 |
| 保留字节       | 00 00 | 2个保留字节  |
| CRC检验        | 15 50 | CRC检验      |