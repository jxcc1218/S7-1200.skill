# 阶段 4 压力测试报告 — S7-1200 系统手册能力包（Bundle v2.5，编译前）

> 执行：阶段 4 压测 agent（cangjie 流水线）
> 测试对象：`books/s71200-system-manual/.cangjie/capabilities/` — verified.yaml（52 条）+ cards/（52 张）
> 方法论：`dsh-cangjie-skill/methodology/06-stage4-pressure-test.md`
> 判定依据：`_stage16/晋级决策表.md` 触发草案 + verified.yaml `frontmatter.description / one_liner / intents / keywords`
> 测试环境声明（方法论要求）：本次为主流程自测，非独立 sub-agent 盲测（fallback 结果，可信度低于独立盲测）。路由判定仅使用 frontmatter.description 与 one_liner/intents/keywords；输出质量抽查的实际数值全部回原书 JSONL 查表复算，未采信卡片自带数字。
> 机检口径核对：cards/ 实测 52 张 = 7 promoted + 45 router；verified.yaml capabilities 恰 52 条；destinations.json 恰 7 个 promoted_to 非空、45 个 router —— 三处口径一致 ✓

---

## ① 触发实测表（7 晋级能力 × 3 话术 = 21 条）

判定方法：只看该能力的 frontmatter.description + one_liner/intents/keywords，回答"这条话术会不会命中本能力"。依据列给出判据。

### A1. power-budget-dual-rail（promote#1）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 1 | "1214C 带三个 SM 1223 和一个 CM 1243-1 够不够电？5V 和 24V 预算算一下" | 正向 | ✅命中 | intents『算这个 CPU/组态带不带动得了』『5V/24V 预算』双命中；keywords 覆盖"预算""CPU" |
| 2 | "SM 1223 的 5V 耗电流是多少 mA？" | 近邻（查单个耗值） | ✅不命中（正确） | 决策表近邻负例：查某模块规格值→router 规格查询；description 是"差额法核算"非查表 |
| 3 | "两个 24V 电源能不能并联给 PLC 供电？" | 近邻（接线禁令） | ✅不命中（正确） | 接线禁令并入本卡 B 段，决策表近邻负例明说"接线审查→router"；description 无接线触发词 |

### A2. analog-scaling-27648（promote#2）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 4 | "4–20mA 信号怎么换算成工程量 MPa？NORM_X SCALE_X 参数怎么填" | 正向 | ✅命中 | intents『把模拟量读数换算成工程量/温度/压力』；keywords『压力』；description"NORM_X→SCALE_X 标定" |
| 5 | "通道一直读 32767，会不会是断线？" | 近邻（32767 卡） | ✅不命中（正确） | 决策表近邻负例：读到异常值 32767→32767 卡；keywords 仅含"温度/压力/换算"，无 32767 |
| 6 | "SM 1231 选 13 位还是 16 位的好？" | 近邻（选型） | ✅不命中（正确） | 选型/接线→router（A2 相邻区分原文）；description 无选型意图 |

### A3. 32767-ambiguity-resolution（promote#3）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 7 | "模拟量通道读到 32767 是什么意思？" | 正向 | ✅命中 | intents『模拟量通道读到 32767/7FFF 是什么意思』逐字命中；keywords['32767','7FFF','是什么意思'] |
| 8 | "诊断中断报文里字节 32 是 16#0006，怎么逐字节解析？" | 近邻（AINFO 卡） | ✅不命中（正确） | 决策表近邻负例：有报文要逐字节解→AINFO router 卡；分流判据（互写）＝有报文 vs 只有值。⚠️此对触发词高度相邻（共享"32767"），属**互斥成立但安全裕度薄**的通过，见①末说明 |
| 9 | "温度显示比实际偏了一截但没报错，怎么修？" | 近邻（标定卡） | ✅不命中（正确） | 标定公式→analog-scaling 卡（A2 相邻区分原文） |

### A4. optimized-standard-access-pairing（promote#4）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 10 | "为什么 HMI 写进 DB 的值过一会儿就没了？" | 正向 | ✅命中 | intents『HMI 偶尔写不进/写了又丢』同义；keywords['HMI','写了又丢','写入的值偶发丢失']；description"HMI 写入丢失" |
| 11 | "中断 OB 读到一半是旧值一半是新值，数据撕裂。" | 近邻（DIS_AIRT 卡） | ✅不命中（正确） | 决策表近邻负例：跨 OB 半更新数据→DIS_AIRT router 卡；分流词"读到新旧夹杂"归 DIS_AIRT |
| 12 | "CPU 网口指示灯不亮，HMI 完全连不上。" | 近邻（链路断） | ✅不命中（正确） | 通信链路断→connection-budget/port-state；本卡 keywords 全部是数据域词，无链路词 |

### A5. connection-budget（promote#5）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 13 | "1214C 最多能接几台 HMI？第 5 台连不上，前 4 台都正常。" | 正向 | ✅命中 | intents『再接一台 HMI/上位机连不上』；keywords['最多能接几台','再接一台','HMI']；description"『第 N 台 HMI 连不上』唯一根因工具" |
| 14 | "知道 IP，但 Modbus TCP 的 502 端口就是不通，防火墙查了也没问题。" | 近邻（端口态） | ✅不命中（正确） | 决策表近邻负例：端口/防火墙不通→port-state router 卡；A2 分流判据"数量不变而个别服务不通=端口/防火墙"成立 |
| 15 | "CPU 的 IP 被人改了，现在彻底连不上。" | 近邻（IP 找回） | ✅不命中（正确） | 不知道 IP/失联→emergency-ip-recovery 卡（A2 相邻区分原文） |

### A6. emergency-ip-recovery（promote#6）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 16 | "PLC 连不上网怎么办？CPU 的 IP 是多少都不知道，网上搜不到设备。" | 正向 | ✅命中 | intents『不知道 IP 也连不上 CPU』；keywords['不知道','也连不上','完全连不上','怎么找回']；description"彻底连不上" |
| 17 | "新买的 1214C 第一次上电，怎么给它分配 IP？" | 近邻（首配） | ✅不命中（正确） | 决策表近邻负例：首次给新 CPU 分配 IP→first-ip router 卡；A2 分流判据"全新 CPU 正常发现走 first-ip" |
| 18 | "Web 服务器 443 打不开，IP 是知道的，ping 也通。" | 近邻（端口态） | ✅不命中（正确） | 决策表近邻负例：知道 IP 但某服务不通→port-state；keywords 与"服务不通"无重叠 |

### A7. run-mode-download-side-effects（promote#7）

| # | 话术 | 类型 | 判定 | 依据 |
|---|---|---|---|---|
| 19 | "设备不能停机，想在 RUN 模式下在线改程序，会有什么风险？" | 正向 | ✅命中 | intents['下载而不重新初始化','同 OB 内重试无效']+keywords['不停机改程序','在线下载会出什么问题','RUN']；description"不停机改程序的必查清单" |
| 20 | "整个项目要全量下载到 CPU，走 STOP 再下载，流程是什么？" | 近邻（STOP 下载） | ✅不命中（正确） | 决策表近邻负例：STOP 下全量下载→router；keywords 全部是"在线/RUN"语境词 |
| 21 | "下载完 CPU 突然转 STOP 了，怎么排查？" | 近邻（STOP 触发排查） | ✅不命中（正确） | 决策表近邻负例：触发"CPU 转 STOP"排查→scan-cycle router 卡 |

**触发实测小结**：21/21 通过（7 正向全命中、14 近邻全抑制、0 误触发、0 漏触发）。

### 三组相邻对互斥裁决（决策表 §4 需实测项）

| 相邻对 | 草案分流判据 | 实测裁决 | 结论 |
|---|---|---|---|
| 32767 ↔ AINFO | 只有可疑读数→32767 卡；手里有诊断中断报文要逐字节解析→AINFO 卡（互写近邻负例） | 话术 7/8 正反两侧均按草案分流；判据与两卡 description 可执行 | **成立**（触发词共享度高，标注：安全裕度薄，非缺陷） |
| connection-budget ↔ port-state | 资源够不够（数量型）→connection-budget；端口/防火墙通不通（可达型）→port-state | 话术 13/14 正反两侧均按草案分流；A2 内"减回即恢复=资源问题"判据可操作 | **成立** |
| emergency-ip ↔ first-ip | 不知道 IP/失联找回来→emergency-ip；新 CPU 首次配 IP→first-ip | 话术 16/17 正反两侧均按草案分流；A2 分流判据"手上有可用组态救失联 vs 无组态起头建联"可执行 | **成立** |

---

## ② router 可达性抽查（15 张，覆盖 5 个主题域）

路径模型：用户问题 → router 入口（verified.yaml `entry/router_entry.description`，判定规则"先问用户要完成什么任务，再按任务域选择能力卡"）→ 能力卡（verified.yaml 登记有 intents/keywords + card 文件存在）。✅ = 路径成立。

### 域 1：功率预算/硬件容量

| # | 能力卡 | 话术 | 判定 | 依据 |
|---|---|---|---|---|
| 1 | max-modules-limits | "1212C 最多能带几个信号模块和通信模块？" | ✅可达 | router_entry"选型与容量核算"域；intents『最大 SM/CM/SB 数量与插槽上限』；card 文件存在 |
| 2 | 24v-parallel-ban | "外部 24V 开关电源能不能跟 CPU 传感器电源并在一起接？" | ✅可达 | intents『禁止外部 24V 电源与 CPU 传感器电源并联——分区供电』；card 存在 |
| 3 | expansion-cable-one-only | "CPU 离机柜远，想用两根扩展电缆行不行？" | ✅可达 | intents『每个 CPU 系统只允许一条扩展电缆』逐字对应话术 |

### 域 2：模拟量/温度

| # | 能力卡 | 话术 | 判定 | 依据 |
|---|---|---|---|---|
| 4 | rtd-range-and-32767 | "RTD 没接传感器的时候通道读数应该是多少？" | ✅可达 | intents『无传感器=32767 判据』；keywords['RTD'] |
| 5 | thermocouple-burnout-test-timing | "热电偶模块的断路检测多少秒测一次？" | ✅可达 | intents『热电偶每 6 秒断路测试，每通道延长更新 9ms』 |
| 6 | sm1231-power-on-32767-avoidance | "一上电那几秒所有温度通道都显示 32767，联锁误动作怎么防？" | ✅可达 | intents『上电未就绪』；keywords['上电未就绪']；router_entry"在线诊断"域覆盖 |

### 域 3：通信/Modbus/设备更换

| # | 能力卡 | 话术 | 判定 | 依据 |
|---|---|---|---|---|
| 7 | port-protocol-default-state | "CPU 上哪些端口默认是开的？102 和 34964 一直在监听正常吗？" | ✅可达 | intents['连不上','资源耗尽']、keywords['端口','防火墙不通']；description 含"仅 102/34964 默认开" |
| 8 | mb-server-default-open-ports | "Modbus TCP 服务器默认开放哪些保持寄存器区给客户端？" | ✅可达 | keywords['MB_SERVER','IB_Count','Start']；intents 覆盖"默认全开 I/Q 区" |
| 9 | mb-client-timeout-variables | "MB_CLIENT 超时时间默认几秒？最大能设到多少？" | ✅可达 | intents『3.0s 默认 / 55s 硬上限 / 2.0s 响应』；keywords['MB_CLIENT'] |
| 10 | first-ip-assignment | "第一次给新 CPU 分配 IP 地址，在线怎么操作？" | ✅可达 | intents『新设备上线第一步』；keywords['连不上'] 较弱但 description 明确"首次 IP 分配流程" |
| 11 | cpu-swap-password-sdcard-disposal | "换新 CPU 时密码和存储卡怎么处理，程序会不会丢？" | ✅可达 | intents['设备更换']；keywords['CPU','设备更换']；router_entry"设备迁移"域 |

### 域 4：诊断/排障

| # | 能力卡 | 话术 | 判定 | 依据 |
|---|---|---|---|---|
| 12 | scan-cycle-stop-ladder | "CPU 偶发转 STOP，是不是扫描超时？" | ✅可达 | intents『CPU 偶发转 STOP』逐字命中；keywords['CPU','STOP','偶发转'] |
| 13 | diag-buffer-interpretation | "诊断缓冲区里一串条目怎么按时间轴解读？" | ✅可达 | intents『入口』（该卡意图关键词过于泛化，但 keywords['入口','终点'] 与 title/intents 组合可判）；⚠️意图词薄弱，建议回填（见④） |
| 14 | get-error-reverse-semantics | "GET_ERROR 的 ENO 是 TRUE 才有错吗？" | ✅可达 | keywords['GET_ERROR','ENO','TRUE'] 完全覆盖 |

### 域 5：Web/安全与静默失败

| # | 能力卡 | 话术 | 判定 | 依据 |
|---|---|---|---|---|
| 15 | awp-space-silent-failure | "Web 自定义页面 AWP 注释写完编译不报错但页面不生效。" | ✅可达 | intents『AWP 注释空格疏漏→编译器静默不生成正确代码』；keywords['AWP']；router_entry"Web 服务器"域 |

**可达性小结**：15/15 可达。router 入口 description 的"先判断问题属于哪个域，再进入对应能力"判定规则在 5 个域上均可执行。

### also_read 链接可达性（verified.yaml 侧）

| 起点 | also_read 指向 | 目标登记 | 结论 |
|---|---|---|---|
| cap.s71200.32767-ambiguity-resolution | ainfo-26-33-break-vs-overflow；analog-scaling-27648 | 均在 Bundle 中（router/promoted），card 文件存在 | ✅ 链路成立，32767→AINFO 依赖可达 |
| cap.s71200.analog-scaling-27648 | 32767-ambiguity-resolution；rtd-range-and-32767 | 均登记 | ✅ |
| cap.s71200.connection-budget | port-protocol-default-state | 登记（router，promote 候补#1） | ✅ |
| cap.s71200.emergency-ip-recovery | first-ip-assignment；port-protocol-default-state | 均登记 | ✅ |
| cap.s71200.power-budget-dual-rail | max-modules-limits；power-budget-precheck | 均登记 | ✅ |
| cap.s71200.optimized-standard-access-pairing | dis-airt-en-airt-guard | 登记 | ✅ |
| cap.s71200.run-mode-download-side-effects | （空） | — | ✅ 决策表未给该卡相邻依赖，空表不构成断链（其 A2 相邻区分均指向 router 侧域而非具体卡） |

结论：从任一晋级卡出发经 also_read 均可到达其依赖；无悬空引用。

---

## ③ 输出质量抽查（3 个晋级能力代表任务，真实执行）

### 任务 1：power-budget-dual-rail — 算"1214C + 2×SM 1223 DC/DC/RLY + 1×CM 1243-1"功率预算

**按 E 段步骤真实执行**（耗值全部现场回原书查表，非抄卡）：

- **Step 1 前置门**（p1153 表 + p1019）：SM 数 2 ≤ 1214C 上限 8 ✓；CM ≤ 3 ✓（1214C 通信模块扩展最多 3 个，p1019）；SB ≤ 1 ✓（无 SB）。通过。
  ⚠️ 输入核查发现：**手册通信模块表（p1158 表 C-4）中不存在"CM 1243-1"订货号；PROFINET 以太网接口模块实名 CP 1243-1（6GK7243-1BX30-0XE0，表 C-6 通信处理器）**。E 段输入契约要求"模块清单（SM/CM/SB 各型号与数量）"并"缺一项先询问，不得猜值"——正确执行是在此处判停并向用户确认型号，而非猜一个 5V 耗值。本次演练按两种分支各算一遍（演示 + 诚实缺口处理）。
- **Step 2 双列减法**（耗值出处：p1019 CPU 1214C 5V=1600 mA、24V=400 mA、DI 4 mA/点；p1068 SM 1223 DI8/DQ8 继电器：SM 总线 145 mA、输入 4 mA/点、继电器线圈 11 mA/个）：

| 项目 | 5V DC | 24V DC |
|---|---|---|
| CPU 1214C 预算（p1019） | 1600 mA | 400 mA |
| 2×SM 1223 DI8×24V/DQ8×继电器，5V 电源（p1068） | 2×145 = 290 mA | — |
| CPU 本机 14 点输入（p1019 每点 4 mA） | — | 14×4 = 56 mA |
| 2×SM 1223 各 8 点输入（p1068） | — | 2×8×4 = 64 mA |
| 2×SM 1223 各 8 个继电器线圈（p1068） | — | 2×8×11 = 176 mA |
| CP 1243-1（若按 5V 供电模块） | 需查其样本/在线数据（**本手册未给该值**——检索全手册无 CP 1243-1 电流耗值行） | — |
| **小计（仅 SM+本机）** | **290 mA** | **296 mA** |
| **差额** | 1600−290 = **+1310 mA** | 400−296 = **+104 mA** |

- **Step 3 分支处置**：两列差额均 ≥ 0 → 组态可行；输出契约的两条 M 端同电位提醒（p1154 警告框：非隔离 M 端必须同一参考电位）齐备。若用户坚持"CM 1243-1"实为某 5V 耗值 > 1310 mA 的模块，5V 列才可能翻负——但手册内无此值，**不得外推**（卡 B 段"手册明确不给速查表，逐型号摘取"与本例一致）。
- **完成标准核对**：✅ 两个带符号差额算出且逐行算式自洽（290+0=290；56+64+176=296）；✅ 判停分支处理了"手册无该模块耗值"的缺输入场景（询问而非猜值）；✅ 输出含两列差额表 + 结论 + M 端提醒。**计算正确**（唯一外值 CP 1243-1 耗值手册不载，属手册事实而非卡缺陷）。

### 任务 2：connection-budget — "1214C 上接 4 台 HMI + 1 台 PG，还想再接 1 台触摸屏，连接资源够不够？"

**按 E 段步骤真实执行**（全部数值回 p477–479 原文）：

- **Step 1 建清单**：PG 1 台；HMI 4+1=5 台。✅ 无凭空类别。
- **Step 2 预留层对账**（p478 表：PG 保留 4/最大 4；HMI 保留 12/最大 18）：PG 1 ≤ 4 全部预留内；HMI 5 ≤ 12 全部预留内。
- **Step 3 动态层总账**：动态占用 0 ≤ 34 ✅。**双闸门**（p579 口径，卡内引用）：每台 HMI 变量 ≤ 400、订阅 ≤ 40——清单未给出单台变量数，输出按契约要求提示"需确认每台 HMI 功能面（读/写/报警诊断各占 1–3 资源）"，5 台即使全按最贵单价 3 资源/台 = 15 资源，仍在 HMI 保留 12+动态池 34 内。
- **Step 4 处置结论**：可行。**关键结论正确性复核**：①"最多能接几台 HMI"——预留保证 ≥4 台（p478"要始终确保至少有四个 HMI"），上限不封死（可组态自由连接增加，p477），"超过保留的台数从 34 动态池扣"口径与原文一致；②p478 表脚注"无法同时实现所有连接的最大值"、p478 说明"在添加 CM/CP 模块时连接总数不增加"均已核到原文，卡的两大反直觉结论**全部落在原文**；③p479 算例（5 台 HMI 占 2/2/2/3/3=12 资源）与卡 A1 一致。
- **完成标准核对**：✅ 预算表六字段（类型｜预留｜最大值｜本站数量｜单价｜动态占用）可填齐；✅ 合计行 + 结论行给出；✅ HMI 单价列按契约"写明判据"而非拍脑袋。**计算正确**。

### 任务 3：analog-scaling-27648 — "±10V 电压输入模块，读数 13824，求电压；再核对若误用 MIN=0 会偏多少"

**按 E 段步骤真实执行**：

- **Step 1 认类型**：±10 V 双极性电压 → NORM_X MIN = −27648（p239"请注意"句已核原文；p1015/p1027 表 A-44/63 同时实证 CPU 内置 0–10 V 单侧输入"负值不支持"——印证卡 B 段"MIN 取值随具体模块型号走，不可跨型号外推"）。✅ MIN 取值有页码依据。
- **Step 2 两段式标定**：N = (13824−(−27648))/(27648−(−27648)) = 41472/55296 = 0.25；对应 ±10 V：0.25×20 − 10 = **−5.0 V**。
  端点闭合检验：Raw=−27648 → N=0 → −10 V ✓；Raw=+27648 → N=1 → +10 V ✓。
- **倍差检查（卡 E 段静默失败自查点）**：误用 MIN=0 → N = 13824/27648 = 0.5 → 被当 **+5 V**。与正确值 −5.0 V 相差 **10 V = 半个量程（50%）**，且全程无报错——卡所述"偏移约半量程且无任何报错"与实算一致。
- **Step 4 溢出定性**：13824 在额定段内，不移交 32767 卡。✅
- **完成标准核对**：✅ 输出参数组 + 换算结果 + 两端闭合检验值；✅ 复算偏差 0（<0.1% 阈值）；✅ 数据类型约束（SCALE_X MIN/MAX/OUT 同型）在输出契约内。**计算正确**。

---

## ④ 未通过项与回炉建议（回炉 = 回阶段 2 改卡）

| # | 项 | 等级 | 问题 | 回炉建议 |
|---|---|---|---|---|
| 1 | diag-buffer-interpretation 卡 intents/keywords | 轻微 | intents 仅['入口','终点']、keywords 同——语义词几乎无召回能力，仅靠 title 与 description 兜底 | 回阶段 2 补 intents/keywords（如『诊断缓冲区条目』『时间戳解读』『诊断事件排序』『安全事件限流』），并在 frontmatter.description 强化任务词。不阻断晋级能力交付 |
| 2 | power-budget-dual-rail 卡 B 段 | 轻微 | 实测发现任务输入"CM 1243-1"为手册表外名（实为 CP 1243-1，以太网接口属通信处理器而非通信模块 CM）；卡未显式提示"表外型号名须先核对订货号再算" | 回阶段 2 在 B 段"相邻易混淆/易漏算项"补一条：模块名与手册表 C-4/C-6 核对（CM=通信模块 / CP=通信处理器），防止拿错表 |
| 3 | 32767 ↔ AINFO 互斥裕度 | 观察 | 互斥成立，但两卡共享"32767"触发词，盲测环境下（非本次自测）分流可能不稳定 | 不回炉；建议编译后在阶段 5 DIGEST 或 router 入口说明中把分流判据置顶重复一次；后续可用 scripts/run_trigger_evals.py 做独立盲测复核 |
| 4 | 方法论硬性要求：独立盲测 | 流程级 | 本次为主流程自测（fallback），方法论明确标注其可信度低于独立 sub-agent 盲测 | 不属于能力缺陷，属执行环境限制。建议补跑 scripts/run_trigger_evals.py 机械判分与 60/40 切分，作为编译前的加强验证 |
| 5 | 评测工件 test-prompts.json / test-results.md | 流程级 | 方法论 §输出 要求 darwin 兼容评测工件；本阶段按派工约束只产出本报告（话术与判定已全部落在本报告 ①②③ 表内，可机械转写） | 如需接入 darwin-skill 进化链路，从本报告转写生成 test-prompts.json；不构成回炉项 |

**不计为失败**：①② 为轻微补丁级回炉（改卡内容，不改能力契约）；③④⑤ 为观察/流程项。**7 个晋级能力与 45 张 router 卡无一个触发性/可达性/计算性失败**。

---

## ⑤ 总体结论

**结论：通过（附 2 项轻微回炉建议，不阻断阶段 5 编译）。**

| 维度 | 结果 | 通过率 |
|---|---|---|
| A 触发实测（7 晋级 × 3 话术） | 21/21（7 命中 + 14 抑制，0 误触发 0 漏触发） | 100% |
| 三组相邻对互斥 | 3/3 成立（32767↔AINFO 裕度标注） | 100% |
| B router 可达性（15 张/5 域） | 15/15；also_read 链 7/7 无悬空 | 100% |
| C 输出质量抽查（3 任务） | 3/3 计算正确（功率预算 +1310 mA/+104 mA；连接预算可行且两个反直觉结论落原文；标定 −5.0 V vs 误算 +5 V 半量程偏移实证） | 100% |
| 机检口径 | 52=7+45 三处一致；destinations.json 7 promoted_to 非空 | ✓ |

按方法论口径的诚实声明：
- 本次为 **fallback 主流程自测**，非独立 sub-agent 盲测；21 条触发判定由同一执行者完成，存在自我一致性偏差风险；
- 输出抽查为 3 个代表任务（每晋级能力覆盖一个正常完成场景），**未逐能力覆盖"边界/缺输入场景"**（power-budget 一个任务中已实证缺输入判停分支有效）；分母按实际执行数计（计划 3，完成 3，断言通过 3），无缺失样本排除；
- "通过"结论以触发/可达/实算三关为据，不含语义盲评与独立模型复核。
