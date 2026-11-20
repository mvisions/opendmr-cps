# CSV 导入 / 导出

[English](csv.en.md)

格式和 **OpenGD77 CPS** 的「File → CSV」完全一样：OpenGD77 CPS 导出的文件可以直接导入，从别的写频软件转过来的信道表，
按这个格式改好（Excel 或文本编辑器都行）也能导入。OpenDMR 多出来的设置放在 Channels.csv 最后几列，Channel Type 也多了几个值。

## 导出

「导出 CSV」下载一个 zip，里面是：

| 文件 | 内容 |
| --- | --- |
| Channels.csv | 信道，一行一个 |
| Zones.csv | 区域：Zone Name，然后 Channel1 … Channel80 是信道名称 |
| Contacts.csv | 联系人：Contact Name, ID, ID Type, TS Override |
| TG_Lists.csv | TG 列表：TG List Name，然后 Contact1 … Contact32 是联系人名称 |
| APRS.csv | APRS 配置 |

文件是 UTF-8（带 BOM，Excel 打开中文不乱码），逗号分隔。

## 导入

「导入 CSV」可以选 zip，也可以一次选一个或多个 CSV 文件（比如 OpenGD77 CPS 导出的整个文件夹里的文件）。只给一部分也可以，
比如只有 Channels.csv。

- 按第一行（表头）认出是哪个表，按列名找列（不分大小写、空格），**列的顺序随意，少的列用默认值，不认识的列不管**。
- 分隔符：逗号、分号（有些国家的 Excel）或 Tab；频率的小数点可以是 `.` 或 `,`。
- 编码：UTF-8 或 GBK（中文 Windows 上 Excel「另存为 CSV」的编码）。
- 引用都按**名称**：信道的 Contact / TG List / APRS，TG 列表里的联系人，区域里的信道。找不到的名称会提示并留空。
- 有错误（比如频率不在电台的频段里）时什么都不改，并列出每个错误的文件和行号；提示（比如名称太长被截断）不影响导入。
- DTMF.csv 不导入。

两种方式（和 OpenGD77 CPS 一样）：

- **替换**（OpenGD77 的 Import）：文件里有的表整个换成文件的内容，信道号按 Channel Number 列（空的话依次排）。
  没导入的表保留，它们对被替换的表的引用按名称重新对上。
- **追加**（OpenGD77 的 Append）：信道加在现有信道后面（不用文件的信道号）；同名的区域、联系人、TG 列表、APRS 配置被文件的替换，其他的新增。

## Channels.csv 的列

前 28 列和 OpenGD77 一样，后 3 列是 OpenDMR 的。

| 列 | 值 |
| --- | --- |
| Channel Number | 1 – 1024（只在「替换」时用） |
| Channel Name | 最多 16 字节（汉字算 2 个），GB2312 里没有的字显示成 ? |
| Channel Type | `Analogue`（FM）、`Digital`（DMR）；OpenDMR 另有 `AM`、`USB`、`LSB`、`P25`、`NXDN`、`M17` |
| Rx Frequency | MHz，如 `439.46250` |
| Tx Frequency | MHz；空 = 和接收一样 |
| Bandwidth (kHz) | `12.5` 或 `25`（模拟信道） |
| Colour Code | 0 – 15（DMR） |
| Timeslot | 1 或 2（DMR） |
| Contact | 联系人名称，或 `None` |
| TG List | TG 列表名称，或 `None` |
| DMR ID | 这个信道用的 DMR ID，或 `None`（用电台的） |
| TS1_TA_Tx / TS2_TA_Tx ID | 发送 Talker Alias：`Off`、`APRS`、`Text`、`APRS+Text` |
| RX Tone / TX Tone | `None`、CTCSS 如 `88.5`、DCS 如 `D023N` / `D023I` |
| Squelch | `Disabled`（用电台的设置）、`Open`、`0%` … `100%`（5% 一档）、`Closed` |
| Power | `Master`（用电台的设置）、`P1` … `P9`、`-W+`；也可以写瓦数：P1 = 50mW、P2 = 500mW、P3 = 1W、P4 = 2W、P5 = 3W、P6 = 4W、P7 = 5W、P8 = 7W、P9 = 10W |
| Rx Only、Zone Skip、All Skip | `Yes` / `No` |
| TOT | 发射限时，秒（15 秒一档），0 = 不限 |
| VOX | `On` / `Off` |
| No Beep、No Eco | `Yes` / `No` |
| APRS | APRS 配置名称，或 `None` |
| Latitude、Longitude | 信道的位置，十进制度数（4 位小数），0 = 没有 |
| Use Location | `Yes` / `No`（按距离排序时用这个位置） |
| NAC | **OpenDMR**：P25 的 NAC，3 位十六进制，空 = 293（F7E：接收任意 NAC） |
| Step (kHz) | **OpenDMR**：步进，2.5 / 5 / 6.25 / 10 / 12.5 / 25 / 30 / 50 |
| Force DMO | **OpenDMR**：DMR 强制直通，`Yes` / `No` |
| RAN | **OpenDMR**：NXDN 的 RAN，0 - 63，空 = 1（0：接收任意 RAN） |
| CAN | **OpenDMR**：M17 的信道接入号，0 - 15，空 = 0（发射用；接收不过滤） |

`Yes / No` 的列也接受 `True / False`、`1 / 0`。

## APRS.csv 的列

| 列 | 值 |
| --- | --- |
| APRS config Name | 最多 8 个字符（最多 8 个配置） |
| SSID | 0 – 15 |
| Via1、Via2 | 路径，如 `WIDE1`，最多 6 个字符 |
| Via1 SSID、Via2 SSID | 0 – 15 |
| Icon table | 0 = 主表 `/`，1 = 副表 `\` |
| Icon | 图标：符号的 ASCII 码减 33（`!` = 0） |
| Comment text | 最多 24 个字符（ASCII） |
| Ambiguity | 位置模糊 0 – 7 |
| Use position | `True` = 用下面的固定位置 |
| Latitude、Longitude | 十进制度数 |
| TX Frequency | 信标的发射频率 MHz，空 = 用信道的 |
| Transmit QSY | `True` = 信标里报当前信道的语音频率 |
| Baud rate setting | 0 = 1200，1 = 300 |

## 例子

在 Excel 里新建一个表，只要几列就可以：

```csv
Channel Name,Channel Type,Rx Frequency,Tx Frequency,RX Tone,TX Tone,Power
北京中继,Analogue,439.46250,434.46250,88.5,88.5,P7
直通 1,Analogue,145.50000,,None,None,Master
BM 4600,Digital,439.12500,431.12500,,,P9
```

另存为 CSV，点「导入 CSV」→「追加」。
