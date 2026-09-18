# 能力索引（完整版）

| capability_id | 标题 | 重要度 | 意图 | 关键词 | 能力卡 |
|---|---|---|---|---|---|
| cap.s71200.power-budget-dual-rail | 功率预算双通道差额法（5V 逻辑预算 / 24V 传感器电源预算，p1153–1155） | critical | 算这个 CPU/组态带不带动得了；5V/24V 预算 | 24V、5V、CPU、算这个、组态带不带动得了、预算 | capabilities/power-budget-dual-rail.md |
| cap.s71200.analog-scaling-27648 | 27648 模拟量标定 + 电压型 −27648 分叉（误用差 75°C 且不报错） | high | 把模拟量读数换算成工程量/温度/压力 | 压力、把模拟量读数换算成工程量、温度 | capabilities/analog-scaling-27648.md |
| cap.s71200.32767-ambiguity-resolution | 32767 四义识别框架（上溢 / 上电未就绪 / 断路 / 模块级报告，A11 矛盾处置） | high | 模拟量通道读到 32767/7FFF 是什么意思 | 32767、7FFF、是什么意思、模拟量通道读到 | capabilities/32767-ambiguity-resolution.md |
| cap.s71200.optimized-standard-access-pairing | 优化/标准访问失配四步因果链（HMI 写入丢失） | high | 看起来对但偶发丢数据；HMI 偶尔写不进/写了又丢 | HMI、不生效、偶尔写不进、写了又丢、写入的值偶发丢失、看起来对但偶发丢数据 | capabilities/optimized-standard-access-pairing.md |
| cap.s71200.connection-budget | 连接资源核算（34 动态资源，HMI 占 1/2/3，加 CM/CP 不增加总数） | high | 再接一台 HMI/上位机连不上；加 CM/CP 不增加连接总数 | CM、CP、CPU、HMI、上位机连不上、不增加连接总数、再接一台、最多能接几台 | capabilities/connection-budget.md |
| cap.s71200.emergency-ip-recovery | 紧急 IP 恢复（临时 IP 不受保护等级限制，p645–646） | high | 不知道 IP 也连不上 CPU；临时 IP 不受保护等级限制 | CPU、IP、不受保护等级限制、不知道、临时、也连不上、完全连不上、怎么找回 | capabilities/emergency-ip-recovery.md |
| cap.s71200.run-mode-download-side-effects | RUN 模式下载副效应清单 + 12 条临时失败指令（20 块上限/输出冻结/HSC 照跑/首扫位/DB 不可覆盖） | high | 同 OB 内重试无效；下载而不重新初始化 | HSC、OB、RUN、下载而不重新初始化、不停机改程序、内重试无效、在线下载会出什么问题 | capabilities/run-mode-download-side-effects.md |
| cap.s71200.power-budget-negative-asymmetric | 差额为负的非对称处置（5V 减模块 / 24V 加外部电源） | medium | 差额为负的非对称处置（5V 减模块 / 24V 加外部电源） | power-budget-negative-asymmetric | capabilities/power-budget-negative-asymmetric.md |
| cap.s71200.power-budget-precheck | 功率预算三条前置约束（扩展上限 / 禁并联 / 非隔离 M 同电位） | medium | 功率预算三条前置约束（扩展上限 / 禁并联 / 非隔离 M | power-budget-precheck | capabilities/power-budget-precheck.md |
| cap.s71200.power-budget-dual-column-procedure | 功率预算双列减法流程（VA/RA-11/RA-1 三源合并指针） | medium | 功率预算双列减法流程（VA/RA-11/RA-1 三源合并指 | power-budget-dual-column-procedure | capabilities/power-budget-dual-column-procedure.md |
| cap.s71200.max-modules-limits | 最大 SM/CM/SB 数量与插槽上限（3 CM 对 CP 同样适用） | medium | 最大 SM/CM/SB 数量与插槽上限（3 CM 对 CP | max-modules-limits | capabilities/max-modules-limits.md |
| cap.s71200.24v-parallel-ban | 禁止外部 24V 电源与 CPU 传感器电源并联——分区供电 | medium | 禁止外部 24V 电源与 CPU 传感器电源并联——分区供电 | CPU | capabilities/24v-parallel-ban.md |
| cap.s71200.24v-consumption-quotas | 24V 通道电流耗值三条定额（DI 4mA/点、继电器线圈 11mA/个、SB 4mA/点） | medium | 24V 通道电流耗值三条定额（DI 4mA/点、继电器线圈 | 24v-consumption-quotas | capabilities/24v-consumption-quotas.md |
| cap.s71200.power-budget-worksheet | 功率预算空白计算表模板（表 B-2 结构） | medium | 功率预算空白计算表模板（表 B-2 结构） | power-budget-worksheet | capabilities/power-budget-worksheet.md |
| cap.s71200.out-of-5v-budget-consequence | 超出 5V 功率预算继续挂扩展模块的后果（不可预期行为） | medium | 超出 5V 功率预算继续挂扩展模块的后果（不可预期行为） | out-of-5v-budget-consequence | capabilities/out-of-5v-budget-consequence.md |
| cap.s71200.ainfo-26-33-break-vs-overflow | AINFO 字节 26–33 解析（RALRM 判定断路/溢出，16#0006/0007/0008 + aaabb000） | medium | 手里有诊断中断报文 | AINFO、RALRM、aaabb、手里有诊断中断报文 | capabilities/ainfo-26-33-break-vs-overflow.md |
| cap.s71200.sm1231-power-on-32767-avoidance | 上电初始化期 32767 假值的启动 OB 轮询规避 | medium | 上电未就绪 | 上电未就绪 | capabilities/sm1231-power-on-32767-avoidance.md |
| cap.s71200.dis-airt-en-airt-guard | DIS_AIRT/EN_AIRT 跨 OB 共享值三段式保护 | medium | 偶发数据异常 | DIS_AIRT、EN_AIRT、偶发数据异常 | capabilities/dis-airt-en-airt-guard.md |
| cap.s71200.port-protocol-default-state | 端口-协议-默认状态表（仅 102/34964 默认开，谁启用它三分法） | medium | 连不上；资源耗尽 | 端口、资源耗尽、连不上、防火墙不通 | capabilities/port-protocol-default-state.md |
| cap.s71200.cpu-swap-password-sdcard-disposal | 更换 CPU 密码/存储卡处置六规则（含免复位两分支） | medium | 设备更换 | CPU、设备更换 | capabilities/cpu-swap-password-sdcard-disposal.md |
| cap.s71200.v3-v4-migration | V3.0→V4.x 迁移（下载后不可撤销 + 六类变化 + 保护等级映射 + 预留区清除） | medium | V3.0→V4.x 迁移（下载后不可撤销 + 六类变化 + | v3-v4-migration | capabilities/v3-v4-migration.md |
| cap.s71200.password-loss-blank-card-erase | 丢密码唯一恢复路径 = 空卡擦除（删整个程序） | medium | 丢密码唯一恢复路径 = 空卡擦除（删整个程序） | password-loss-blank-card-erase | capabilities/password-loss-blank-card-erase.md |
| cap.s71200.protection-level-loss-on-restore-interrupt | 恢复中断→永久丢失保护等级（手册未给恢复步骤） | medium | 恢复中断→永久丢失保护等级（手册未给恢复步骤） | protection-level-loss-on-restore-interrupt | capabilities/protection-level-loss-on-restore-interrupt.md |
| cap.s71200.no-secure-erase | S7-1200 不支持安全擦除（处置要求与设备能力矛盾） | medium | S7-1200 不支持安全擦除（处置要求与设备能力矛盾） | no-secure-erase | capabilities/no-secure-erase.md |
| cap.s71200.factory-reset-10-consequences | 复位为出厂设置三复选框决策与十项后果（MAC 不变） | medium | 复位为出厂设置三复选框决策与十项后果（MAC 不变） | MAC | capabilities/factory-reset-10-consequences.md |
| cap.s71200.sd-card-batch-and-force-points | 存储卡初始化批次与强制点随卡迁移陷阱 | medium | 存储卡初始化批次与强制点随卡迁移陷阱 | sd-card-batch-and-force-points | capabilities/sd-card-batch-and-force-points.md |
| cap.s71200.iolink-factory-reset-consequences | IO-Link 主站复位出厂的后果核对与入库前置动作 | medium | IO-Link 主站复位出厂的后果核对与入库前置动作 | Link | capabilities/iolink-factory-reset-consequences.md |
| cap.s71200.mb-client-frozen-inputs | MB_CLIENT 请求期冻结输入（改输入→无法确定活动实例→失序） | medium | MB_CLIENT 请求期冻结输入（改输入→无法确定活动实例 | MB_CLIENT | capabilities/mb-client-frozen-inputs.md |
| cap.s71200.mb-server-default-open-ports | MB_SERVER 默认全开 I/Q 区（QB/IB_Count=65535、Start=0，S3） | medium | MB_SERVER 默认全开 I/Q 区（QB/IB_Cou | IB_Count、MB_SERVER、Start | capabilities/mb-server-default-open-ports.md |
| cap.s71200.hsc-chaos-input-conflict | HSC 输入点冲突『紊乱情况』（可致人员死亡警告） | medium | HSC 输入点冲突『紊乱情况』（可致人员死亡警告） | HSC | capabilities/hsc-chaos-input-conflict.md |
| cap.s71200.ctrl-hsc-ext-en-new-pairing | CTRL_HSC_EXT En/New 成对更新（不置 En 参数静默不生效） | medium | 必须为真 | CTRL_HSC_EXT、New、必须为真 | capabilities/ctrl-hsc-ext-en-new-pairing.md |
| cap.s71200.datalog-capacity-budget | 数据日志容量公式（创建即全量预留，4 段字节预算 + 双上限） | medium | 数据日志容量公式（创建即全量预留，4 段字节预算 + 双上限 | datalog-capacity-budget | capabilities/datalog-capacity-budget.md |
| cap.s71200.datalog-lifecycle-full-file | 数据日志写满→换新文件续接框架 | medium | 创建即全量预留 | 创建即全量预留 | capabilities/datalog-lifecycle-full-file.md |
| cap.s71200.datalog-2kb-write-amplification | 数据日志每次写入至少占 2KB（写入放大与攒批对策） | medium | 数据日志每次写入至少占 2KB（写入放大与攒批对策） | datalog-2kb-write-amplification | capabilities/datalog-2kb-write-amplification.md |
| cap.s71200.get-error-reverse-semantics | GET_ERROR 反向语义（ENO=TRUE=有错；接管诊断缓冲区；只留首个错误） | medium | GET_ERROR 反向语义（ENO=TRUE=有错；接管诊 | ENO、GET_ERROR、TRUE | capabilities/get-error-reverse-semantics.md |
| cap.s71200.dactdp-partial-failure-masked | D_ACT_DP 80A1/80A6 部分失败伪装成 W#16#0000 成功 | medium | D_ACT_DP 80A1/80A6 部分失败伪装成 W#1 | D_ACT_DP | capabilities/dactdp-partial-failure-masked.md |
| cap.s71200.config-control-absent-module-silent | 组态控制『不存在』模块的静默剖面（读 0 / 写无效 / 无诊断） | medium | 组态控制『不存在』模块的静默剖面（读 0 / 写无效 / 无 | config-control-absent-module-silent | capabilities/config-control-absent-module-silent.md |
| cap.s71200.awp-space-silent-failure | AWP 注释空格疏漏→编译器静默不生成正确代码 | medium | AWP 注释空格疏漏→编译器静默不生成正确代码 | AWP | capabilities/awp-space-silent-failure.md |
| cap.s71200.certificate-unsupported-params-silent | 证书管理器表外参数『只是不工作』不报错 | medium | 证书管理器表外参数『只是不工作』不报错 | certificate-unsupported-params-silent | capabilities/certificate-unsupported-params-silent.md |
| cap.s71200.pwmpto-silent-write-drop | PWM/PTO 输出地址静默丢弃且不可强制 + 频率/时基静默边界 | medium | PWM/PTO 输出地址静默丢弃且不可强制 + 频率/时基静 | PTO、PWM | capabilities/pwmpto-silent-write-drop.md |
| cap.s71200.io2mod-fragment-laddr | IO2MOD 片段访问致 LADDR 静默错值 | medium | IO2MOD 片段访问致 LADDR 静默错值 | LADDR、MOD | capabilities/io2mod-fragment-laddr.md |
| cap.s71200.getiopart-setiopart-silent-truncation | GETIO_PART/SETIO_PART 区域>LEN 静默截断不报错 | medium | GETIO_PART/SETIO_PART 区域>LEN 静 | GETIO_PART、LEN、SETIO_PART | capabilities/getiopart-setiopart-silent-truncation.md |
| cap.s71200.scan-cycle-stop-ladder | 扫描周期监视 STOP 阶梯（二次超时即 STOP、OB 80 不豁免、删除 OB 80 更危险） | medium | CPU 偶发转 STOP | CPU、STOP、偶发转 | capabilities/scan-cycle-stop-ladder.md |
| cap.s71200.diag-buffer-interpretation | 诊断缓冲区解读框架（时间轴+状态切换+安全事件限流） | medium | 诊断缓冲区怎么读；诊断缓冲区条目按时间轴解读；看看日志里报了什么故障；安全事件限流导致多条合并 | 诊断缓冲区、diagnostic buffer、故障条目、事件查看、诊断日志、时间轴解读、安全事件限流、入口、终点 | capabilities/diag-buffer-interpretation.md |
| cap.s71200.full-protection-overrides-web-users | 保护等级『完全保护（无访问权）』覆盖 Web 用户权限 | medium | 保护等级『完全保护（无访问权）』覆盖 Web 用户权限 | Web | capabilities/full-protection-overrides-web-users.md |
| cap.s71200.cpu-1217c-spec-boundary | CPU 1217C 规格分界读数模板（1MHz HSC / 250KB 工作存储器 / 双端口） | medium | CPU 1217C 规格分界读数模板（1MHz HSC / | CPU、HSC、MHz | capabilities/cpu-1217c-spec-boundary.md |
| cap.s71200.expansion-cable-one-only | 每个 CPU 系统只允许一条扩展电缆 | medium | 每个 CPU 系统只允许一条扩展电缆 | CPU | capabilities/expansion-cable-one-only.md |
| cap.s71200.rtd-range-and-32767 | RTD 电阻量程满量程换算与『无传感器=32767』判据 | medium | RTD 电阻量程满量程换算与『无传感器=32767』判据 | RTD | capabilities/rtd-range-and-32767.md |
| cap.s71200.thermocouple-burnout-test-timing | 热电偶每 6 秒断路测试，每通道延长更新 9ms | medium | 热电偶每 6 秒断路测试，每通道延长更新 9ms | thermocouple-burnout-test-timing | capabilities/thermocouple-burnout-test-timing.md |
| cap.s71200.first-ip-assignment | 首次 IP 分配流程与『在设备上直接设 IP』双刃剑 | medium | 连不上 | 连不上 | capabilities/first-ip-assignment.md |
| cap.s71200.mb-client-timeout-variables | MB_CLIENT 超时静态变量取值口径（3.0s 默认 / 55s 硬上限 / 2.0s 响应） | medium | MB_CLIENT 超时静态变量取值口径（3.0s 默认 / | MB_CLIENT | capabilities/mb-client-timeout-variables.md |
| cap.s71200.ctrl-hsc-ext-version-criterion | CTRL_HSC_EXT 取代 CTRL_HSC 的版本判据 | medium | CTRL_HSC_EXT 取代 CTRL_HSC 的版本判据 | CTRL_HSC、CTRL_HSC_EXT | capabilities/ctrl-hsc-ext-version-criterion.md |
