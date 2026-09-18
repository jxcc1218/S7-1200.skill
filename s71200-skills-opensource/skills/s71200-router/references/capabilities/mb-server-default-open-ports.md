# 能力卡：MB_SERVER 默认全开 I/Q 区（QB/IB_Count=65535、Start=0，S3）

## R（原文引用）
> "警告：访问过程映像的风险。每个 Modbus TCP 客户端对过程映像输入和输出以及 Modbus 保持寄存器定义的数据块或位存储区域都具有读写访问权限。未经授权的读写操作可能会将 PLC 变量更改为无效值并破坏过程操作。……请限制对特定 Modbus 客户端的 IP 地址的访问。"（p827）
> "QB_Count UInt 65535 远程设备可以写入的字节数。如果 QB_Count = 0，则远程设备无法写入输出。"（p828）
> ——《S7-1200 系统手册》V4.7，14.5 Modbus 通信

## I（自述）
MB_SERVER 出厂默认把整个输入/输出过程映像开放给任意 Modbus 客户端：QB_Start/IB_Read_Start=0，QB_Count/QB_Read_Count/IB_Read_Count 全为 65535 → 开箱即"全 I/Q 可读、全 Q 可写"，而 RemoteAddress 默认 0.0.0.0 接受任何来源。收口三件套：①按业务最小需要设置六个静态变量（读/写各一套起点+数量，不需要的置 0）；②RemoteAddress 填指定客户端 IP；③可选用 Data_Area_Array 定义数据区——注意 S5：**不配数据区时请求直接落过程映像**，不是"没配就不暴露"。

## A1（合成演练，含可复算数值）
原书未提供本条算例。合成演练：要求客户端（192.168.2.241）只能读 IB0–IB9、只能写 QB20–QB29：IB_Read_Start=0、IB_Read_Count=10；QB_Start=20、QB_Count=10；QB_Read_Count 与 IB_Read 不用的写权限置 0；RemoteAddress=192.168.2.241。验证：越界读 Q 区 → Modbus 例外码（02/03 非法地址/数据）。数字口径：p828 官方示例"只允许 QB10–QB17 → QB_Start=10、QB_Count=8"可复算（10+8−1=17）。

## A2（未来触发）
- 场景：Modbus 服务器上线前安全检查；"外部 PLC 把我的输出全改了"事故复盘；审计要求收敛暴露面。
- 语言信号：中"MB_SERVER 默认开放／限制 Modbus 可访问范围／QB_Count"；英"MB_SERVER default open / restrict Modbus access / QB_Count IB_Read_Count"。
- 相邻区分：客户端侧请求行为 → mb-client-frozen-inputs；连接资源占用（几台客户端）→ connection-budget（晋级卡）；HR_Start_Offset 寻址偏移细节见本卡 B 段。

## E（执行步骤）
1. 输入契约：预期可读/可写区清单 + 客户端 IP 清单。
2. 设置六个静态变量：QB_Start/QB_Count（写输出）、QB_Read_Start/QB_Read_Count（读输出）、IB_Read_Start/IB_Read_Count（读输入）；不需要的 Count=0。
3. CONNECT 结构：RemoteAddress 填客户端 IP（0.0.0.0 仅调试用）；LocalPort 默认 502。
4. 可选：Data_Area_Array 定义 ≤8 个数据区，条目必须连续无空隙（第一个空白条目终止搜索，p830）。
5. 验证：用客户端读越界地址应得异常码；非白名单 IP 应无法交互。
6. 完成标准：越界访问被拒 + 白名单外无响应，两项实测通过。

## B（边界）
- 反场景：MB_CLIENT（我是客户端）问题 → frozen/timeout 卡；保持寄存器（功能 3/6/16）范围问题 → 本卡 B 段勘误。
- 矛盾/勘误：B19/B4——表 14-69 粘连数字（400990/4011920）与正文算例不符，正确上界按正文复算（起始 40021、长度 100 字 → 上界 40120）；B18——p828 QB_Read_Count 说明误写"如果 QB_Count = 0"（应为自身变量名）；S5——不配 Data_Area_Array 时请求落过程映像。引用寻址上限一律以正文算例口径复算，不照抄表 14-69。
- 外链缺口：无（警告与变量表手册自足）。
