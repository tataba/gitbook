# 2.概述

### 2.1功能描述

类型：双色屏。

扫描方式：1/4，1/8，1/16 扫，用命令可以修改。

标准字库：16/24/32 点阵字库，以及非8像素点倍数的字库。

数据类型：字库方式。

播放方式：节目顺序播放/定长播放可选。

通讯方式：RS232/TTL、RS485/TTL、GPRS，波特率 9600/57600可设，波特率自适应。

外部闪存：512Kbyte/1Mbyte/2Mbyte/2Mbyte/可选。

节目个数：64/32 个，单个节目最多支持 5 个图文区。

动态区域个数：5 个动态区，1 个特殊动态区。

动态区刷新率：>=1s。

### 2.2通讯方式

1. RS232/485  波特率 ：9600/57600, 无校验, 8 位数据, 1 停止位。
2. GPRS/RF  波特率 ：9600/57600, 无校验, 8 位数据, 1 停止位。
3. Ethernet         TCP/IP：10/100M

### 2.3术语和缩略语

| 名称  | 说明                                |
| ----- | ----------------------------------- |
| MSB   | 高位字节（Most Significant Byte）   |
| LSB   | 低位字节（Least Significant Byte ） |
| CRC16 | 16 位的 CRC 校验，校验算法参考附录  |
| CHK   | CRC 校验值                          |

### 2.4协议说明

- 本文档中十六进制数据表示为 0x？？，如 0x7E。
- 本文档中涉及到的多字节参数，均以先低字节(LSB)后高字节(MSB)顺序发送，但是对于文件名和控制器名称等字符串参数，发送时按顺序发送，如“P123”则先发送‘P’，最后发送‘3’。
- 本文档中提及的数据长度，如无特别说明，皆是以字节（byte）为单位。
- 本文档中提及的时间相关的参数均采用 BCD 码。
- 本文档中提及的颜色属性，均用 1Byte 来表示，其中，Bit0 表示红，bit1 表示绿，bit2 表示蓝，对于每一个 Bit，0 表示灭，1 表示亮。
- 本文档中所有偏移量、块地址等参数如无特殊说明，均以 0 开始计算。
- 本文档中区域的坐标定义按照左上角为坐标原点。横、纵坐标分别向右、向下延伸。
- 本文档中提及的“读取”和“写入”都是指上位机对控制器的动作。
- 本文档中提及的保留字全部默认发送 0x00。