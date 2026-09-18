# S7-1200 Skills — 蒸馏自西门子官方系统手册的 8 个 Agent Skill

> 把《SIMATIC S7-1200 可编程控制器 系统手册》（V4.7, 12/2024, A5E02486685-AQ，1209 页）蒸馏成 **7 个独立 Skill + 1 个来源路由入口**，供 Claude / DeepSeek Harness 等 agent 在真实 PLC 工程中直接调用。

**不是书摘。** 每个能力都经过三重验证（来源充分性 / 可执行性 / 任务增益），其中最危险的一类——**静默失败**（写错不报错、手册还自认校验缺口）——被单独提炼成可执行的自查步骤。

## 这是什么

| 入口 | 作用 |
|---|---|
| `power-budget-dual-rail` | 5V/24V **双通道功率预算差额法**：超预算时 5V 只能拆模块、24V 可加外部电源（非对称），附完整算例 |
| `analog-scaling-27648` | 模拟量标定两段式（NORM_X→SCALE_X），**电压型 MIN=−27648 分叉**——误用静默偏移 75 °C 且 ENO 全程 TRUE |
| `32767-ambiguity-resolution` | 通道值 `32767` 的**四义判读树**（上溢/上电未就绪/断路/模块级）+ 启动 OB 轮询规避 |
| `optimized-standard-access-pairing` | 优化/标准块访问**必须配对**：失配四步因果链 → HMI/中断 OB 写入 STRUCT 静默丢失 |
| `connection-budget` | 34 个动态连接资源核算：**HMI 一台占 1/2/3 个资源**、加 CM/CP 不增加总数、各类最大值不可同时实现 |
| `emergency-ip-recovery` | 无法通过 IP 访问 CPU 的紧急恢复：三成因/两前提/DCP，**不受保护等级限制**（安全双面性） |
| `run-mode-download-side-effects` | RUN 模式下载：20 块上限、四类静默副效应、12 条临时失败指令（跨 OB 重试有效/同 OB 必败） |
| `s71200-router` | **来源路由入口**：内含 45 张能力卡（通信组态、Web 服务器、PtP/Modbus、安全与访问控制、诊断运维、附录计算、设备迁移与更换）+ 术语表 + cheatsheet |

## 快速开始

### Claude Code / Claude Desktop
```bash
# 克隆到你的 skills 目录
git clone https://github.com/<you>/<repo>.git
# 把 skills/ 下的 8 个文件夹拷入 Claude 的 skills 目录（或按你的加载方式配置）
```

### DeepSeek Harness (DSH)
将 `skills/` 下各文件夹放入会话工作区或按 preset 挂载；`s71200-router` 建议常驻，7 个独立 skill 按需加载。

### 直接阅读
每个 skill 文件夹的 `SKILL.md` 自包含（R/I/A1/A2/E/B 六段 + 原文页码引用），不依赖其余文件；`s71200-router/references/` 下是全部能力卡与术语表。

## 为什么可信

本包不是 LLM 一次性生成的摘要，而是**带完整审计轨迹的蒸馏流水线**产物（cangjie-skill 方法论）：

- **1,209 页逐页解构** → 151 项读者任务 → 1,030 条候选 → **1,042 条验证判定**（每条带原文页码与演练 input/expected/observed）
- 压测：21 条触发话术 0 误触发 0 漏触发；3 个代表任务实算核对（数值回原文查表复核）
- **24 处手册内部矛盾 + 22 处勘误**被识别并在能力卡 B 段标注取舍判据（见 `docs/manual-reliability-notes.md`）——例如：
  - 同一页参数表**连续三行输出说明误印成同一个变量名**（p834）
  - 高海拔合规：铭牌只覆盖 2000 m，表格却写 5000 m，且部分 CPU 类型**根本没有对应行**
  - **循环上电**在三处章节分别被"要求/允许/禁止"
- 15 条**静默失败**模式单独成册（如组态控制中标记"不存在"的模块：写不生效、读恒 0、无诊断、状态恒"正常"）

详见 `DIGEST.md`（导读）、`docs/pressure-test-report.md`（压测）、`docs/manual-reliability-notes.md`（手册可信度笔记）。

## 仓库结构

```
├── skills/                 # 8 个 skill（直接可用）
│   ├── power-budget-dual-rail/
│   ├── analog-scaling-27648/
│   ├── 32767-ambiguity-resolution/
│   ├── optimized-standard-access-pairing/
│   ├── connection-budget/
│   ├── emergency-ip-recovery/
│   ├── run-mode-download-side-effects/
│   └── s71200-router/      # 来源路由入口（45 能力卡 + 术语表 + cheatsheet）
├── docs/
│   ├── pressure-test-report.md
│   ├── manual-reliability-notes.md
│   └── capability-destinations.json
├── DIGEST.md               # 导读：蒸馏出了什么、最危险的坑在哪
├── LICENSE
└── README.md
```

## 边界（务必读）

以下内容**手册本身就不含**（整章外链给西门子其他文档），本包**不凭常识补造**，只给入口与缺口说明：

- 运动控制轴组态/调试/回原点/ErrorID（→《S7-1200 运动控制》）
- PID 整定步骤与参数初值（→《S7-1200/S7-1500 PID 控制》）
- CP 1243 系列远程通信组态（→ 三本 CP 操作说明）
- F 系列（故障安全）模块规格（→ SIMATIC Safety 手册）
- 第三方 Modbus 中继器选型、浪涌保护器件安装方法

## 来源与许可

- 蒸馏源：《S7-1200 可编程控制器 系统手册》V4.7, 12/2024（西门子版权所有，本仓库不收录原文，仅收录带页码引用的**能力化重述**）
- 本仓库代码与能力卡文本：MIT
- 方法论：[cangjie-skill](https://github.com/kangarooking/cangjie-skill)（RIA-TV++ / 三重验证 / 晋级门）

## 致谢

- 西门子工业在线支持提供的公开文档
- cangjie-skill / darwin-skill 生态
