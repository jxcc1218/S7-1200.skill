# 能力卡：IO2MOD 片段访问致 LADDR 静默错值

## R（原文引用）
> "ADDR IN or IN/OUT ? Variant I、Q、M、D、L （子）模块内的 IO 地址（I、Q、PI、PQ）。确保片段访问未用于参数 ADDR。如果使用了片段访问，将会在 LADDR 参数处输出不正确的值。"（p425）
> "在 ADDR 参数中输入 IO 地址。如果在此参数中使用了一系列 IO 地址，仅通过评估第一个地址来确定硬件标识符。"（p425）
> ——《S7-1200 系统手册》V4.7，10.11.3 IO2MOD（根据 I/O 地址确定硬件标识符）

## I（自述）
IO2MOD 由 I/O 地址反查硬件标识符（LADDR），为 RDREC/GET_DIAG 等需要 HW_ID 的指令供参。两个静默陷阱：①**片段（位）访问**——ADDR 里用了 `IX0.0`、`QX0.1` 这类位片段写法，LADDR 会输出**不正确的值**且指令不报错（RET_VAL 不提示）；②**地址区间只看第一个**——传入一段地址时只评估第一个地址，跨模块区间只能得到第一个模块的 HW_ID。正确用法：传（子）模块的起始绝对地址（如 `%IW64` 或符号名），SCL 中不能用 `%QWx:P` 这种带访问前缀的写法（p425 说明）。

## A1（合成演练，含可复算数值）
原书未提供数值算例。合成演练：目标模块起始地址 IW64。错：`IO2MOD(ADDR:="MyDB".Flag)`（Bool 片段）→ LADDR 得错值；对：`IO2MOD(ADDR:=%IW64)` → LADDR=该模块 HW_ID，可再喂给 RDREC。判别：RET_VAL=0 且 LADDR 明显对不上设备组态里的硬件标识符 → 高度怀疑片段访问。

## A2（未来触发）
- 场景：程序里动态拼硬件标识符；IO2MOD 返回的 LADDR 与组态对不上；用 Bool/Word 变量充当 ADDR。
- 语言信号：中"IO2MOD 返回错值／LADDR 不对／片段访问／硬件标识符反查"；英"IO2MOD wrong LADDR / fragment access / HW identifier from address"。
- 相邻区分：反方向（HW_ID → IO 地址）→ RD_ADDR（p425–426）；HW_ID → 插槽 → LOG2GEO（p424）；GETIO/SETIO 的截断问题 → getiopart 卡。

## E（执行步骤）
1. 输入契约：目标（子）模块的起始 IO 地址（绝对或符号）。
2. 自查 ADDR 类型：非片段（I/Q/PI/PQ 字节/字起始地址）、SCL 下不带 `:P` 前缀。
3. 调用 IO2MOD，取 LADDR。
4. 验证：与设备组态"系统常量"中该模块 HW 标识符比对；不一致 → 排查片段访问/地址区间第一地址选错。
5. 完成标准：LADDR 与系统常量一致，下游指令（RDREC 等）正常应答。

## B（边界）
- 反场景：只知道插槽不知道地址（用 LOG2GEO 反查路径）；模块不存在 → RET_VAL=8090（正常报错，非静默）。
- 手册口径：p424 说明"在 HW 类型不支持组件的情况下，将返回模块 0 的子插槽号"（LOG2GEO 侧口径），IO2MOD 侧无此句——两指令语义分开引用。
- 矛盾/勘误：无。
- 外链缺口：RDREC/WRREC 数据记录号含义 → 设备制造商文档（D8）。
