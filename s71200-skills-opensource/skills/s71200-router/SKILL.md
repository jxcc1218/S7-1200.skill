---
name: s71200-router
description: |
  S7-1200 系统手册的来源路由入口：按用户问题路由到 45 张 router 能力卡 （通信组态、Web 服务器、PtP/Modbus、安全与访问控制、诊断运维、附录计算与设备迁移）。 判定规则：先问用户要完成什么任务，再按任务域选择能力卡； 静默失败与不可逆操作必须读卡内 B 段后再执行。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: router
  cangjie.bundle-id: bundle.s71200-system-manual
  cangjie.capability-count: 52
  cangjie.entrypoint-count: 8
---
# S7-1200 可编程控制器 系统手册 — 来源路由入口（compact pack）

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 运动控制轴组态/调试与 PID 整定（手册外链专用手册，本包只给入口与缺口说明）
- CP 1243 系列远程通信组态（手册指向三本 CP 操作说明）
- 第三方设备的 Modbus 寄存器语义与浪涌保护器件的安装方法

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 容量先于功能：功率预算、连接资源、装载存储器先算上限再实现
2. 配对约束普遍存在：优化/标准访问、PROFINET 设备名两处一致、组态控制记录与实装匹配
3. 诊断从责任方判定开始：先分清是本方组态错还是设备/网络问题
4. 区别组态期的门与运行期的墙：访问等级与密码不约束指令执行与 PLC 间通信
5. 可恢复性有边界：迁移下载不可撤销、丢密码只能空卡擦除、无安全擦除

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 算这个 CPU/组态带不带动得了；5V/24V 预算 | references/capabilities/power-budget-dual-rail.md | 已晋级为独立 Skill `power-budget-dual-rail`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 把模拟量读数换算成工程量/温度/压力 | references/capabilities/analog-scaling-27648.md | 已晋级为独立 Skill `analog-scaling-27648`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 模拟量通道读到 32767/7FFF 是什么意思 | references/capabilities/32767-ambiguity-resolution.md | 已晋级为独立 Skill `32767-ambiguity-resolution`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 看起来对但偶发丢数据；HMI 偶尔写不进/写了又丢 | references/capabilities/optimized-standard-access-pairing.md | 已晋级为独立 Skill `optimized-standard-access-pairing`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 再接一台 HMI/上位机连不上；加 CM/CP 不增加连接总数 | references/capabilities/connection-budget.md | 已晋级为独立 Skill `connection-budget`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 不知道 IP 也连不上 CPU；临时 IP 不受保护等级限制 | references/capabilities/emergency-ip-recovery.md | 已晋级为独立 Skill `emergency-ip-recovery`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 同 OB 内重试无效；下载而不重新初始化 | references/capabilities/run-mode-download-side-effects.md | 已晋级为独立 Skill `run-mode-download-side-effects`（已安装时优先直接使用；本卡仅作原文与背景补充） |
| 差额为负的非对称处置（5V 减模块 / 24V 加外部电源） | references/capabilities/power-budget-negative-asymmetric.md | references/capabilities/power-budget-dual-rail.md |
| 功率预算三条前置约束（扩展上限 / 禁并联 / 非隔离 M | references/capabilities/power-budget-precheck.md | references/capabilities/power-budget-dual-rail.md |
| 功率预算双列减法流程（VA/RA-11/RA-1 三源合并指 | references/capabilities/power-budget-dual-column-procedure.md | references/capabilities/power-budget-dual-rail.md |
| 最大 SM/CM/SB 数量与插槽上限（3 CM 对 CP | references/capabilities/max-modules-limits.md | references/capabilities/power-budget-dual-rail.md |
| 禁止外部 24V 电源与 CPU 传感器电源并联——分区供电 | references/capabilities/24v-parallel-ban.md | references/capabilities/power-budget-dual-rail.md |
| 24V 通道电流耗值三条定额（DI 4mA/点、继电器线圈 | references/capabilities/24v-consumption-quotas.md | references/capabilities/power-budget-dual-rail.md |
| 功率预算空白计算表模板（表 B-2 结构） | references/capabilities/power-budget-worksheet.md | references/capabilities/power-budget-dual-rail.md |
| 超出 5V 功率预算继续挂扩展模块的后果（不可预期行为） | references/capabilities/out-of-5v-budget-consequence.md | references/capabilities/power-budget-dual-rail.md |
| 手里有诊断中断报文 | references/capabilities/ainfo-26-33-break-vs-overflow.md | references/capabilities/32767-ambiguity-resolution.md、references/capabilities/analog-scaling-27648.md |
| 上电未就绪 | references/capabilities/sm1231-power-on-32767-avoidance.md | references/capabilities/32767-ambiguity-resolution.md |
| 偶发数据异常 | references/capabilities/dis-airt-en-airt-guard.md | references/capabilities/optimized-standard-access-pairing.md |
| 连不上；资源耗尽 | references/capabilities/port-protocol-default-state.md | references/capabilities/connection-budget.md、references/capabilities/mb-server-default-open-ports.md |
| 设备更换 | references/capabilities/cpu-swap-password-sdcard-disposal.md | references/capabilities/password-loss-blank-card-erase.md、references/capabilities/factory-reset-10-consequences.md、references/capabilities/no-secure-erase.md |
| V3.0→V4.x 迁移（下载后不可撤销 + 六类变化 + | references/capabilities/v3-v4-migration.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| 丢密码唯一恢复路径 = 空卡擦除（删整个程序） | references/capabilities/password-loss-blank-card-erase.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| 恢复中断→永久丢失保护等级（手册未给恢复步骤） | references/capabilities/protection-level-loss-on-restore-interrupt.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| S7-1200 不支持安全擦除（处置要求与设备能力矛盾） | references/capabilities/no-secure-erase.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| 复位为出厂设置三复选框决策与十项后果（MAC 不变） | references/capabilities/factory-reset-10-consequences.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| 存储卡初始化批次与强制点随卡迁移陷阱 | references/capabilities/sd-card-batch-and-force-points.md | references/capabilities/cpu-swap-password-sdcard-disposal.md |
| IO-Link 主站复位出厂的后果核对与入库前置动作 | references/capabilities/iolink-factory-reset-consequences.md | references/capabilities/factory-reset-10-consequences.md |
| MB_CLIENT 请求期冻结输入（改输入→无法确定活动实例 | references/capabilities/mb-client-frozen-inputs.md | references/capabilities/mb-client-timeout-variables.md |
| MB_SERVER 默认全开 I/Q 区（QB/IB_Cou | references/capabilities/mb-server-default-open-ports.md | references/capabilities/port-protocol-default-state.md |
| HSC 输入点冲突『紊乱情况』（可致人员死亡警告） | references/capabilities/hsc-chaos-input-conflict.md | references/capabilities/ctrl-hsc-ext-en-new-pairing.md |
| 必须为真 | references/capabilities/ctrl-hsc-ext-en-new-pairing.md | references/capabilities/ctrl-hsc-ext-version-criterion.md、references/capabilities/hsc-chaos-input-conflict.md |
| 数据日志容量公式（创建即全量预留，4 段字节预算 + 双上限 | references/capabilities/datalog-capacity-budget.md | references/capabilities/datalog-2kb-write-amplification.md、references/capabilities/datalog-lifecycle-full-file.md |
| 创建即全量预留 | references/capabilities/datalog-lifecycle-full-file.md | references/capabilities/datalog-capacity-budget.md |
| 数据日志每次写入至少占 2KB（写入放大与攒批对策） | references/capabilities/datalog-2kb-write-amplification.md | references/capabilities/datalog-capacity-budget.md |
| GET_ERROR 反向语义（ENO=TRUE=有错；接管诊 | references/capabilities/get-error-reverse-semantics.md | references/capabilities/diag-buffer-interpretation.md |
| D_ACT_DP 80A1/80A6 部分失败伪装成 W#1 | references/capabilities/dactdp-partial-failure-masked.md | references/capabilities/config-control-absent-module-silent.md |
| 组态控制『不存在』模块的静默剖面（读 0 / 写无效 / 无 | references/capabilities/config-control-absent-module-silent.md | references/capabilities/dactdp-partial-failure-masked.md |
| AWP 注释空格疏漏→编译器静默不生成正确代码 | references/capabilities/awp-space-silent-failure.md | — |
| 证书管理器表外参数『只是不工作』不报错 | references/capabilities/certificate-unsupported-params-silent.md | — |
| PWM/PTO 输出地址静默丢弃且不可强制 + 频率/时基静 | references/capabilities/pwmpto-silent-write-drop.md | — |
| IO2MOD 片段访问致 LADDR 静默错值 | references/capabilities/io2mod-fragment-laddr.md | — |
| GETIO_PART/SETIO_PART 区域>LEN 静 | references/capabilities/getiopart-setiopart-silent-truncation.md | — |
| CPU 偶发转 STOP | references/capabilities/scan-cycle-stop-ladder.md | references/capabilities/diag-buffer-interpretation.md |
| 诊断缓冲区怎么读；诊断缓冲区条目按时间轴解读；看看日志里报了什么故障；安全事件限流导致多条合并 | references/capabilities/diag-buffer-interpretation.md | references/capabilities/scan-cycle-stop-ladder.md、references/capabilities/get-error-reverse-semantics.md |
| 保护等级『完全保护（无访问权）』覆盖 Web 用户权限 | references/capabilities/full-protection-overrides-web-users.md | — |
| CPU 1217C 规格分界读数模板（1MHz HSC / | references/capabilities/cpu-1217c-spec-boundary.md | references/capabilities/power-budget-dual-rail.md |
| 每个 CPU 系统只允许一条扩展电缆 | references/capabilities/expansion-cable-one-only.md | references/capabilities/power-budget-dual-rail.md |
| RTD 电阻量程满量程换算与『无传感器=32767』判据 | references/capabilities/rtd-range-and-32767.md | references/capabilities/32767-ambiguity-resolution.md、references/capabilities/analog-scaling-27648.md |
| 热电偶每 6 秒断路测试，每通道延长更新 9ms | references/capabilities/thermocouple-burnout-test-timing.md | references/capabilities/32767-ambiguity-resolution.md、references/capabilities/rtd-range-and-32767.md |
| 连不上 | references/capabilities/first-ip-assignment.md | references/capabilities/emergency-ip-recovery.md |
| MB_CLIENT 超时静态变量取值口径（3.0s 默认 / | references/capabilities/mb-client-timeout-variables.md | references/capabilities/mb-client-frozen-inputs.md |
| CTRL_HSC_EXT 取代 CTRL_HSC 的版本判据 | references/capabilities/ctrl-hsc-ext-version-criterion.md | references/capabilities/ctrl-hsc-ext-en-new-pairing.md |

**非能力类查询**：
- 书名/作者/章节/整书概览 → references/overview.md
- 术语解释 → references/glossary.md
- 决策规则速查（不需要原文依据时） → references/cheatsheet.md
- 完整意图与关键词索引（本表未覆盖的意图先查这里） → references/capability-index.md

## 加载规则

- 每次任务先读本文件，再按路由表加载 **1** 张能力卡；任务明确跨域时最多加载 2 张。
- 概览/书名类问题不加载能力卡，用「核心原则」与 references/overview.md 回答。
- 路由表与 references/capability-index.md 都无法命中的意图，明确告知超出本书范围，不要硬套。

## 边界与判停

- 需要 F 系列（故障安全）模块规格或安全程序组态时停止并指向 SIMATIC Safety 手册
- 候选规则与手册页内容冲突且澄清清单未给取舍判据时停止，转 needs_review
