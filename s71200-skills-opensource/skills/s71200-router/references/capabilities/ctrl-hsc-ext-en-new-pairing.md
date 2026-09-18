# 能力卡：CTRL_HSC_EXT En/New 成对更新（不置 En 参数静默不生效）

## R（原文引用）
> "HSC 的 SDT 输入用前缀'En'或'New'来表示。带有前缀'En'或'New'的输入启用 HSC 功能或更新相应参数。前缀'New'表示更新值。要使新值生效，相应'En'位必须为真且'New'值必须有效。"
> ——《S7-1200 系统手册》V4.7，11.1.1.2 CTRL_HSC_EXT 指令系统数据类型 (SDT)，p439

## I（自述）
CTRL_HSC_EXT 的 SDT（HSC_Count/HSC_Period/HSC_Frequency）里，每个可运行时修改的参数都是一对字段：`NewXxx`（要写入的新值）+ `EnXxx`（该值生效的开关）。**只给 New 不置 En → 静默不生效**：指令正常执行、STATUS=0，但参数纹丝不动——这是手册唯一一处"必须为真"的显式配对条件。逐对字段（HSC_Count，p440）：EnHSC、EnCapture、EnSync、EnDir、EnCV、EnSV、EnReference1/2、EnUpperLmt、EnLowerLmt、EnOpMode、EnLmtBehavior。注意 EnSyncBehavior "不使用此值"。

## A1（合成演练，含可复算数值）
原书未提供数值算例。合成演练：运行中把上限从 10000 改 20000：`NewUpperLimit:=20000; EnUpperLmt:=TRUE` → 执行后 CurrentCount 到 20000 才触发上限行为；漏掉 `EnUpperLmt:=TRUE` → STATUS=0 无任何报错，行为仍是旧上限 10000。周期测量示例（p441）：NewPeriod 只允许 10/100/1000 ms 三档，写 500 + EnPeriod=TRUE 会因值非法不生效（80B4 类非法值）。

## A2（未来触发）
- 场景：运行时改 HSC 预设/上限/方向后"没反应且没报错"；审查既有 HSC 程序为什么参数改不动。
- 语言信号：中"CTRL_HSC_EXT 改了不生效／En 参数／New 值"；英"CTRL_HSC_EXT En bit / New value not applied"。
- 相邻区分：输入点功能分配冲突 → hsc-chaos-input-conflict；该用 EXT 还是老 CTRL_HSC → ctrl-hsc-ext-version-criterion；HSC 组态入门 → router。

## E（执行步骤）
1. 输入契约：待改参数清单（参数名+新值）。
2. 逐参数核对配对：NewXxx 有值 ⇔ EnXxx=TRUE；无配对即整改。
3. 值合法性核查：NewPeriod ∈ {10,100,1000} ms（p441）；NewDirection ∈ {1,−1}（p440）；越界值即使 En=TRUE 也不生效或报 80B1/80B4/80B5/80B6（p435）。
4. 执行并验证：读回 SDT 输出字段（CurrentCount 等）或观察行为变化。
5. 静默失败自查：STATUS=0 但行为未变 → 第一查 En 位漏置，第二查 New 值非法。
6. 完成标准：目标参数在运行中实际生效（行为/读回双重确认）。

## B（边界）
- 反场景：静态组态改参数（停机改设备组态，不走本卡）；周期测量读数计算（ElapsedTime/EdgeCount 公式，p441）另属 HSC 测量域。
- 手册口径：DONE 恒 1、BUSY 恒 0（同步指令，p435）——不能用 BUSY 判断是否生效。
- 矛盾/勘误：无；EnSyncBehavior/NewSyncBehavior 标注"不使用"，勿编程依赖。
- 外链缺口：无。
