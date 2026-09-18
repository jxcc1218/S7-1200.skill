# 能力卡：GETIO_PART/SETIO_PART 区域>LEN 静默截断不报错

## R（原文引用）
> "如果目标区域大于 LEN，则指令将写入目标区域的前 LEN 个字节。ERROR 接收 FALSE 值。"（GETIO_PART，p303）
> "如果源区域大于 LEN，指令将传输 OUTPUTS 的前 LEN 个字节。ERROR 接收 FALSE 值。"（SETIO_PART，p304）
> ——《S7-1200 系统手册》V4.7，10.3.4/10.3.5

## I（自述）
GETIO_PART（一致性读）/SETIO_PART（一致性写）对分布式 IO 模块部分过程映像操作。LEN 指定传输字节数；目标/源区域（INPUTS/OUTPUTS）比 LEN 大时**只传前 LEN 字节，ERROR=FALSE 正常返回**——区域多出来的部分不更新也不报错，属于典型静默截断。真正报错的情形：OFFSET+LEN 超出模块覆盖范围 → DW#16#4080B700。另外 OFFSET/LEN 必须不越过模块过程映像边界，ID 用 HW_SUBMODULE 硬件标识符。

## A1（合成演练，含可复算数值）
原书未提供数值算例。合成演练：读 8 字节输入（OFFSET=0，LEN=8），INPUTS 为 16 字节数组 → 执行成功 ERROR=FALSE，但只有第 0–7 字节被更新，第 8–15 字节保持旧值——若程序把 16 字节当整体处理，后半段是脏数据。整改：LEN=8 且按 8 字节消费，或 LEN 改 16（若模块覆盖允许）。

## A2（未来触发）
- 场景："部分读回来的数据是旧的"；一致性读写排障；LEN 与缓冲区大小核对。
- 语言信号：中"GETIO_PART 截断／只更新了前几个字节／区域比 LEN 大"；英"GETIO_PART truncation / partial update / buffer larger than LEN"。
- 相邻区分：整模块读写 → DPRD_DAT/DPWR_DAT（p314，router）；LADDR 反查错误 → io2mod 卡；错误极性两说（A22）见 B 段。

## E（执行步骤）
1. 输入契约：模块 HW_ID、OFFSET、LEN、目标/源区域尺寸。
2. 核对 LEN == 消费尺寸（区域可以 ≥LEN，但**程序必须只读/写前 LEN 字节**）。
3. 核对 OFFSET+LEN 不超模块覆盖（否则 4080B700）。
4. 调用后检查 ERROR/STATUS；ERROR=FALSE 仍要确认"前 LEN 字节"消费约定。
5. 静默失败自查：输出区后半段值不动 → 先查区域>LEN 截断，再查物理模块。
6. 完成标准：数据更新范围与 LEN 精确一致。

## B（边界）
- 反场景：LEN 大于模块区域（正常报错路径）；非分布式 IO（本地 I/O 不需一致性块）。
- 矛盾/勘误（A22 取舍判据）：p303 正文一处笔误式表述"如果数据传送过程中没有出现错误，则 ERROR 接收 TRUE 值"与同页参数表（ERROR=TRUE 才是有错）相反——**以参数表为准**：ERROR=FALSE 无错；正文那句按上下文应为笔误。引用时注明取舍。
- 外链缺口：RDREC/WRREC 记录号 → D8。
