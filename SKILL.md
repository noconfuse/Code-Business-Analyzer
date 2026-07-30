---
name: code-business-analyzer
description: "Use when analyzing the commercial value of a code project and generating an HTML business analysis report. Evaluates six dimensions — code quality, market & category, competitors & moat, business model, investment verdict, future roadmap — and answers the core question: should this project be kept as a focus area? Anchors every dimension to a validated framework (ISO/IEC 25010, Porter Five Forces, Blue Ocean, TAM/SAM/SOM, Jobs-to-be-Done, Business Model Canvas, Crossing the Chasm, Christensen disruption theory, Lean Startup, BCG Matrix). Includes a Devil's Advocate step and Kill Criteria that force a downgrade on GO verdicts. Outputs a single self-contained HTML report (8 chapters + appendix, black-and-white hard-edged design) with source-traceable data tagged by confidence level. Triggers on: 'analyze the business value of this code project', 'is this project worth continuing', 'evaluate the investment value of my code', 'generate a business report for this repo', 'review my project from a commercial angle'."
metadata:
  author: baolei
  version: 1.0.0
  license: MIT
  tags: business-analysis, code-review, investment-analysis, html-report
---

# Code Business Analyzer — 代码商业分析报告生成器

## Overview

对代码项目进行全方位商业分析，生成专业的 HTML 网页报告。采用**黑白硬朗设计风格**，**8 章 + 附录** 结构。

## Report Structure

报告结构：
```
01. 执行摘要
02. 项目概览  （含五维评分卡）
03. 代码质量
04. 市场与赛道 （市场分析 + 赛道阶段/热度/集中度）
05. 竞品与护城河 （竞品对比 + 同质化 + 护城河 + SWOT）
06. 商业价值
07. 投资决策  （判定卡 + 矩阵 + 竞争力雷达 + 行动建议）
08. 未来方向
附录：数据溯源 （含可信度分级说明）
```

## When to Use

触发条件：
- 用户要求"分析我的代码/项目的商业价值"
- 用户要求"生成一份代码项目的商业报告"
- 用户要求"评估这个项目的投资价值"
- 用户要求"这个项目值不值得继续做"
- 用户要求"从商业角度分析代码"
- 用户要求"评估项目的赛道竞争力/同质化程度"

## Prerequisites

- 用户提供代码项目路径
- 需要 WebSearch / WebFetch 联网获取市场和竞品数据
- 需要 GitHub 公开数据（用于竞品对比）

## Methodology Anchoring

每个分析维度必须套用以下方法论，不得自由心证：

| 分析维度 | 方法论 | 执行要求 |
|----------|--------|---------|
| 代码质量 | **ISO/IEC 25010** | 逐维对照标准子特性，评分附代码位置 |
| 护城河 | **Porter Five Forces** (1980) | 必须覆盖五力：供应商议价/买方议价/替代品威胁/新进入者/行业内竞争 |
| 同质化 | **Blue Ocean W SR** (Kim & Mauborgne) | 判断是红海竞争还是创造了新市场空间 |
| 市场规模 | **TAM/SAM/SOM** 三层漏斗 | 禁止用 TAM 替代 SOM |
| 用户价值 | **Jobs-to-be-Done** (Christensen) | 以"用户要完成的任务"定义价值，不以功能定义 |
| 商业模式 | **Business Model Canvas** (Osterwalder) | 9 构件逐项分析 |
| 采用阶段 | **Crossing the Chasm** (Moore) | 定位：创新者/早期采用者/早期大众/晚期大众/落后者 |
| 创新类型 | **破坏性创新理论** (Christensen) | 明确延续性 or 破坏性 |
| PMF 验证 | **Lean Startup** (Ries) | 无用户/收入数据时标注"PMF 未验证" |
| 投资评级 | **调整后 BCG Matrix** 逻辑 | 按"赛道吸引力 × 相对竞争力"双轴定位 |
| 价值类型 | **五类价值创造机制**（自建框架） | Phase 6 Step 0 必须先分类再评估 |

## Project Stage Determination

在执行 Kill Criteria 前，先判定项目阶段：

| 阶段 | 判定依据（满足任一） |
|------|-------------------|
| 萌芽期 | 首次提交 < 6 月，或无 git 历史，或无发布版本，或文档不完整 |
| 成长期 | 有规律提交 + 基础文档 + 早期用户信号（Issue/Star 增长），但商业化未验证 |
| 成熟期 | 有稳定发布周期 + 用户/收入数据 + 完整文档和测试 |

## Kill Criteria (阶段感知一票否决)

命中任一条，投资判定自动降档，Agent 无权覆盖。触发时第 7 章必须显式标注规则编号。

### 全阶段适用
| # | 条件 | 自动后果 |
|---|------|---------|
| K1 | 同质化指数 ≥ 8 且赛道竞品 ≥ 5 个（Stars > 5K） | 最高"谨慎" |
| K2 | 核心代码存在 CVE Critical 漏洞且未修复 | 降一档 |
| K6 | 核心依赖已归档或无人维护（如 Python 2 / 已 EOL 框架） | 降一档 |

### 成长期+ 适用（萌芽期不触发）
| # | 条件 | 自动后果 |
|---|------|---------|
| K3 | 无商业化验证（无用户/无收入/无 LOI）且赛道已进入成熟期 | 最高"谨慎" |
| K4 | 项目 6 个月内无提交（归档/弃置） | 自动 NO-GO |
| K5 | 无开源协议或协议不兼容（如 GPL 用于商业 SDK） | 降一档 |

### 萌芽期替代评估：投资人否决信号
萌芽期项目不适用 K3/K4/K5。用以下正向信号替代，每命中一条可在判定依据中作为加分项引用：
- 有人搜索替代方案（搜索量验证痛点真实性）
- GitHub Issue 有外部用户主动反馈
- 核心代码处理了边界情况（说明作者真懂领域）
- 架构选型贴合问题本质而非追新技术

## Value Type Classification (五类价值创造机制)

不同产品创造商业价值的底层机制不同，判断标准必须匹配机制。Phase 6 首先识别项目主价值类型，再套用对应评估框架。

| 类型 | 价值来源 | 核心判断 | 典型 |
|------|---------|---------|------|
| 先进工具 | 效率差：用户本来能做，现在快 10 倍 | 效率提升 ≥ 10 倍？用户有切换动力？ | Stripe、Cursor |
| 普惠化 | 能力下沉：用户本来不能做，现在能做了 | 新用户群体规模？替代品是"什么都不做"？ | Canva、Notion |
| 信息差消除 | 连接：供需本来碰不到面 | 冷启动怎么过？双边正反馈能否自转？ | 淘宝、大众点评 |
| 细分痛点 | 洞察深度：被忽略的缝隙，痛到愿意付钱 | 有人正在用劣质替代品绕过？ | Calendly、1Password |
| 趋势载体 | 文明级变迁：站在不可逆的社会级方向上 | 趋势不可逆？项目是趋势本身还是过渡形态？ | OpenClaw、Tesla |

项目可同时属于多类，取主类型（1-2 个）套用对应问题集。主类型 = 项目最核心价值主张所属类别。

## Workflow (9 Phases)

---

### Phase 1: Project Scanning & Code Analysis

1. 确定项目根目录。
2. 扫描目录结构、识别项目类型（Web/API/移动/库/SDK/CLI/AI 等）。
3. 读取配置文件提取元数据：package.json / pom.xml / go.mod / requirements.txt / Cargo.toml 等。
4. 统计代码规模：LOC（按语言分类）、文件数、目录数。
5. 阅读核心代码（15-30 个关键文件）：入口文件、业务逻辑、README、测试目录、配置文件。
6. 大型项目（>100 文件）使用 Agent 工具（Explore 子代理）。

---

### Phase 2: Code Quality Assessment

基于 `references/code_quality_metrics.md` 进行评估。

1. **六维评估**：架构设计 / 代码规范 / 安全性 / 性能 / 可维护性 / 技术债务，各 1-10 分。锚定 ISO/IEC 25010。
2. **置信度标注**：每个评分附 [高]（代码直接证据）/ [中]（间接推断）/ [低]（假设或推测）。
3. **输出**：六维评分表（附文件路径）、Top 5 风险点、雷达图数据数组 `[?,?,?,?,?,?]`。
3. **竞品代码对标**：不在此阶段执行。延迟到 Phase 5 识别竞品后，由 Phase 5 输出对标表，Phase 9 填入 `{{CODE_BENCHMARK_SECTION}}`。

---

### Phase 3: Data Sourcing & Verification

在开始市场调研前建立数据可信度基线。此阶段不生成独立章节，方法论在附录头部体现。

1. **可信度分级**：
   - A 级：权威引用（Gartner/IDC/年报/GitHub 公开数据/政府统计）
   - B 级：行业估计（科技媒体/行业白皮书）
   - C 级：模型推断（AI 推理，无法验证）

2. **溯源追踪**：每引用外部数据立即记录编号 [源N]、论断、URL、机构、日期、等级。

3. **强制规则**：任何数字必须标注来源，无法溯源的数据标注"C 级"。

---

### Phase 4: Market Research

1. 搜索市场数据（规模、增长率、驱动因素、目标用户、政策影响、资本流向）。
2. 趋势风向判断（量化阈值）：
   - 强顺风：CAGR > 25% + 资本流入同比增长 + 政策利好
   - 温和顺风：CAGR 15-25% 或资本流入正增长
   - 中性：CAGR 5-15% 且资本流向不明
   - 温和逆风：CAGR 0-5% 或资本撤离
   - 强逆风：CAGR < 0% 或政策限制
3. 使用 WebFetch 深入权威数据源。
4. 输出：市场规模 + 增长率（标注 [源N]）、趋势图表数据、市场机会分析、风向判断。

---

### Phase 5: Competitive & Track Analysis

#### 5.1 竞品发现与量化
- 搜索 3-4 个主要竞品
- 量化对比：Stars / 融资 / 成立时间 / 协议（均标注 [源N]）
- 功能对比矩阵（8-10 个维度，用 ✓ / ~ / ✗ 标注）
- **竞品代码对标**（产出 `{{CODE_BENCHMARK_SECTION}}`）：基于 `references/code_quality_metrics.md` Section 7 的框架，GitHub 公开数据对比 Stars、提交频率、CI/CD、技术栈、代码规模。输出为 HTML 表格。

#### 5.2 同质化指数
四个子维度加权（功能 40% + 技术路线 30% + 用户 20% + 商业模式 10%），每项 1-10 分。
最终映射为 0-10 同质化指数。附加 Blue Ocean 判断：项目是红海竞争（指数 ≥ 6）还是创造了新市场空间（指数 ≤ 3）。

#### 5.3 赛道分析
- 阶段：萌芽期 / 成长期 / 成熟期 / 衰退期
- 热度：冷 / 热 / 过热
- 集中度：高 / 中 / 低

#### 5.4 护城河
锚定 Porter Five Forces。六维各 1-10 分：品牌 / 网络效应 / 数据飞轮 / 技术壁垒 / 先发优势 / 合规。本项目 + 最强竞品双线对比。另需分析替代品威胁和新进入者威胁（定性，不计入六维评分但写入护城河评价文本）。

#### 5.5 SWOT（压缩版）
每格 3-5 条要点。

---

### Phase 6: Business Value Assessment

**Step 0: 价值类型识别**

判断项目主价值类型（五类中取 1-2 类为主），写入 `{{VALUE_TYPE}}` 和 `{{VALUE_TYPE_ASSESSMENT}}`。

**Step 1: 按主类型回答对应问题集**

非主类型的问题简略回答即可，主类型问题必须深入展开。

#### 先进工具
1. 效率提升倍数：对比现有方案，快多少/省多少？（量化）
2. 切换动力：迁移成本 vs 效率收益，用户有无弃用现有工具的理由
3. 技术壁垒：效率优势来自什么？能否被复制？

#### 普惠化
1. 新用户群体：原来不能做这件事的人有多少？
2. 替代品分析：用户的替代方案是"什么都不做"还是"花钱请人做"？
3. 质量可持续性：降低门槛后，产出质量是否可接受？

#### 信息差消除
1. 冷启动策略：如何解决先有鸡还是先有蛋？
2. 双边流动性：供需比是否健康？留存是否支撑正反馈？
3. 网络效应强度：用户增加是否直接提升平台价值？

#### 细分痛点
1. 痛点验证：是否有人在用劣质替代品绕过？（搜索量/论坛吐槽/GitHub Issue）
2. 缝隙市场容量：目标用户规模 × 付费意愿 = 可达 ARR？
3. 大厂威胁：这个缝隙是否大到引起大公司注意？

#### 趋势载体
1. 趋势不可逆性：驱动趋势的外部条件是否会倒退？
2. 载体 vs 过渡形态：项目是趋势的最终形态还是中间态？
3. 窗口期：先发优势还能维持多久？
4. 终局路径：独立成长还是被收购？谁会买？为什么？

**Step 2: 通用评估（所有类型必填）**
1. **市场规模**（TAM/SAM/SOM）：三层漏斗，SOM 基于可达渠道估算，禁止用 TAM 替代。
2. **商业模式**（Business Model Canvas）：9 构件逐项分析。
3. **创新类型**（Christensen）：延续性 or 破坏性，附依据。
4. **采用阶段**（Crossing the Chasm）：定位采用曲线位置。
5. **PMF 验证状态**（Lean Startup）：无数据标注"PMF 未验证"。
6. **投资价值评级**：按量化标准（见 report_framework.md）。

---

### Phase 7: Investment Decision Assessment

**核心决策阶段，直接回答"值不值得"。**

0. **魔鬼代言人**（在写判定前执行）：
   先写"为什么这个项目应该被毙掉"的 3 条论点（附量化支撑），再写"为什么值得继续"的 3 条论点。两份论点同时呈现于 `{{DECISION_RATIONALE_HTML}}`，格式为"反方论点"和"正方论点"两个子块。最终判定必须回应反方论点——无法反驳时判定不得高于"谨慎"。

1. **项目阶段判定 + Kill Criteria 检查**：先判定阶段（萌芽/成长/成熟），再逐条检查 K1-K6，命中自动降档并标注规则编号。萌芽期项目检查投资人否决信号作为加分项。
2. **决策矩阵定位**：根据赛道吸引力（纵轴）和相对竞争力（横轴）确定四象限位置。
3. **投资判定**：
   - GO（右上象限）：赛道吸引力高 + 相对竞争力强
   - 谨慎（左上/右下）：赛道好但竞争力弱，或竞争力强但赛道不确定
   - NO-GO（左下象限）：赛道差 + 竞争力弱
4. **判定依据**：2-3 条核心论据 + 量化指标。谨慎档位附加转绿条件。
5. **行动建议**：按 Impact/Effort 矩阵排序（见 report_framework.md）。
6. 输出：综合竞争力五维雷达图数据 `[?,?,?,?,?]`。

---

### Phase 8: Future Directions

1. **产品路线图**：短期（0-3月）/ 中期（3-12月）/ 长期（1-3年）
   - 每项必须关联可验证里程碑（如"Docker 镜像发布""CI 覆盖率 > 70%"），禁止"优化性能""加强文档"等无法验收的表述。
2. **PMF 验证路径**（如 Phase 6 标注"PMF 未验证"）：定义最小验证实验和成功指标。
3. **技术演进建议**
4. **风险与应对表**

---

### Phase 9: Generate HTML Report

1. 读取 `assets/report_template.html`。
2. 按以下清单填入所有占位符，**不允许保留任何 `{{PLACEHOLDER}}`**。

#### 封面
- `{{PROJECT_NAME}}` / `{{PROJECT_DESCRIPTION}}` / `{{REPORT_DATE}}` / `{{TECH_STACK}}` / `{{CODE_SIZE}}` / `{{OVERALL_SCORE}}`
- `{{COVER_DECISION_CLASS}}` — `go` / `caution` / `nogo`
- `{{DECISION_VERDICT}}` — `GO · 继续重点迭代` / `谨慎迭代 · 有条件推进` / `NO-GO · 建议暂停`
- `{{DECISION_ONELINER}}` — 一句话判定理由

#### 01 — 执行摘要
- `{{EXECUTIVE_SUMMARY_TEXT}}` — 项目定位描述，3 句话内
- `{{KEY_FINDING_1}}` ~ `{{KEY_FINDING_5}}` — 5 条核心发现

#### 02 — 项目概览
- `{{PROJECT_VERSION}}` / `{{LANGUAGES}}` / `{{FRAMEWORKS}}` / `{{LOC}}` / `{{FILE_COUNT}}` / `{{LICENSE}}`
- `{{FEATURES_OVERVIEW}}` / `{{FEATURE_TAG_1}}` ~ `{{FEATURE_TAG_3}}`
- `{{CODE_QUALITY_SCORE}}` / `{{TRACK_SCORE}}` / `{{DIFFERENTIATION_SCORE}}` / `{{BUSINESS_SCORE}}` — 各 1-10

#### 03 — 代码质量
- `{{ARCH_SCORE}}` ~ `{{DEBT_SCORE}}` — 六维分值
- `{{ARCH_FINDING}}` ~ `{{DEBT_FINDING}}` — 六维发现文本
- `{{CODE_BENCHMARK_SECTION}}` — 竞品代码对标表 HTML（无数据则留空）
- `{{TECH_RISKS_HTML}}` — Top 5 风险点 HTML
- `{{RADAR_DATA}}` — 六维数组 `[?,?,?,?,?,?]`

#### 04 — 市场与赛道
- `{{MARKET_SIZE}}` / `{{MARKET_GROWTH}}` — 标注 [源N]
- `{{TRACK_STAGE}}` / `{{TRACK_HEAT}}` / `{{TRACK_CONCENTRATION}}`
- `{{WIND_CLASS}}` — `wind-strong-up` / `wind-mild-up` / `wind-neutral` / `wind-mild-down` / `wind-strong-down`
- `{{WIND_ARROW}}` — `↑` / `↗` / `→` / `↘` / `↓`
- `{{WIND_LABEL}}` — `强顺风` / `温和顺风` / `中性` / `温和逆风` / `强逆风`
- `{{MARKET_OVERVIEW_TEXT}}` / `{{MARKET_OPPORTUNITY_TEXT}}` / `{{TRACK_ANALYSIS_TEXT}}`
- `{{MARKET_TREND_LABELS}}` — JSON 数组 `["2022","2023",...]`
- `{{MARKET_TREND_DATA}}` — JSON 数组 `[100,120,...]`

#### 05 — 竞品与护城河
- `{{COMPETITOR_1}}` / `{{COMPETITOR_2}}` / `{{COMPETITOR_3}}` — 竞品名
- `{{COMPETITOR_QUANT_ROWS}}` — 量化对比表行 HTML
- `{{COMPETITOR_FEATURE_ROWS}}` — 功能矩阵行 HTML
- `{{COMPETITOR_FEATURE_LABELS}}` / `{{SELF_FEATURE_SCORES}}` / `{{COMP1_FEATURE_SCORES}}` / `{{COMP2_FEATURE_SCORES}}` / `{{COMP3_FEATURE_SCORES}}` — 图表 JSON 数组
- `{{HOMOGENIZATION_INDEX}}` — 0-10
- `{{HOMO_INDICATOR_POS}}` — 仪表位置百分比 (= 同质化指数 × 10)
- `{{HOMOGENIZATION_LABEL}}` — 文字标签
- `{{HOMO_BREAKDOWN_ROWS}}` — 四个子维度表行 HTML
- `{{MOAT_TOP_COMP}}` — 最强竞品名
- `{{MOAT_BARS_HTML}}` — 六维进度条 HTML
- `{{MOAT_TOTAL}}` / `{{MOAT_COMP_TOTAL}}` — 总分 /60
- `{{MOAT_ASSESSMENT}}` — 护城河评价
- `{{MOAT_SELF_DATA}}` / `{{MOAT_COMP_DATA}}` — 各六维数组 `[?,?,?,?,?,?]`
- `{{STRENGTHS_LIST}}` ~ `{{THREATS_LIST}}` — SWOT 各格 `<li>` 列表。**只允许 `<li>...</li>` 标签**，不允许 `<p>` `<div>` 或其他标签

#### 06 — 商业价值
- `{{VALUE_TYPE}}` — 主价值类型（如"先进工具 + 趋势载体"）
- `{{VALUE_TYPE_ASSESSMENT}}` — 价值类型分析 HTML（按主类型问题集的回答）
- `{{MARKET_SIZE}}` / `{{MARKET_GROWTH}}` — 标注 [源N]（与 04 章数据一致）
- `{{BUSINESS_MODEL_TEXT}}`
- `{{INVESTMENT_RATING}}` / `{{INVESTMENT_ANALYSIS_TEXT}}`

#### 07 — 投资决策
- `{{DECISION_ICON}}` — `■` (GO) / `□` (谨慎) / `▲` (NO-GO)
- `{{DECISION_VERDICT}}` / `{{DECISION_ONELINER}}` — 与封面一致
- `{{MATRIX_UL_ACTIVE}}` ~ `{{MATRIX_LR_ACTIVE}}` — 四格中仅一格填 `active`
- `{{COMPREHENSIVE_RADAR_DATA}}` — 五维数组 `[?,?,?,?,?]`
- `{{DECISION_RATIONALE_HTML}}` — 判定依据 HTML
- `{{DECISION_CONDITIONS_HTML}}` — 如为"谨慎"，输出转绿条件；否则留空
- `{{ACTION_ITEMS_ROWS}}` — 行动建议表行 HTML

#### 08 — 未来方向
- `{{SHORT_TERM_TITLE}}` / `{{SHORT_TERM_DESC}}`
- `{{MID_TERM_TITLE}}` / `{{MID_TERM_DESC}}`
- `{{LONG_TERM_TITLE}}` / `{{LONG_TERM_DESC}}`
- `{{TECH_EVOLUTION_HTML}}`
- `{{RISK_TABLE_ROWS}}`

#### 附录 — 数据溯源
- `{{DATA_SOURCE_ROWS}}` — 溯源表行 HTML。格式：`<tr><td>源N</td><td>论断</td><td><a href="URL">来源</a></td><td>机构</td><td>日期</td><td><span class="conf conf-a">A 级</span></td></tr>`

#### 生成规则
- 所有占位符必须替换，不得保留
- Chart.js 数据为合法 JSON 数组
- 表格行使用完整 `<tr><td>...</td></tr>`
- HTML 标签正确闭合
- **严格遵循黑白配色**：不使用任何彩色代码

3. 保存至 `{workspace}/{project-name}-business-report.html`。
4. **验证**：重新读取保存的 HTML 文件，搜索 `{{` 字符串。如发现任何残留占位符，逐一修复。搜索 `✅` `⚠️` `❌` 等 emoji，如有则替换为 ✓ / ~ / ✗。
5. 使用 `present_files` 展示。

---

## Design Guidelines

### 色彩
- **仅黑、白、灰**
- 正文 #333，次要 #666，辅助 #999，极细线 #e8e8e8
- 图表：黑色实心 > 灰色实心 > 浅灰实心

### 判定图标
- GO: ■ (实心方块)
- 谨慎: □ (空心方块)
- NO-GO: ▲ (三角)

### 禁止
- 禁止渐变、彩色徽章、圆角、阴影、emoji

### Chart.js CDN
模板当前使用 `jsdelivr.net` 加载 Chart.js。如用户位于中国大陆且 CDN 不稳定，可在生成报告后将 `<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js">` 替换为国内镜像（如 `<script src="https://cdn.bootcdn.net/ajax/libs/Chart.js/4.4.1/chart.umd.min.js">`）。图表在无 JS 的环境下不渲染，但报告文字内容不受影响。

---

## Edge Cases & Degradation Strategies

以下场景执行降级策略，避免报告出现空白占位符：

| 场景 | 降级指令 |
|------|---------|
| **WebSearch 无结果** | 市场规模标注 `"N/A · 该赛道公开数据稀缺 [C 级]"`，市场趋势图用定性描述替换柱状图数据，趋势标签和数组各填 2 个锚点 |
| **找不到竞品** | 竞品对比表保留但只填本项目列，竞品列标注 `"—"`；同质化指数标注为 `"C 级推断 · 无直接竞品可比较"`；护城河雷达图只画本项目数据 |
| **项目无配置文件** | 基本信息表对应字段填 `"—"`，不编造 |
| **代码库 >500 文件** | Phase 1 使用 Agent 工具（Explore 子代理）分批扫描，只读核心入口 + 目录结构 + README，不逐文件遍历 |
| **无开源协议** | 协议字段标注 `"未声明"` |
| **市场趋势数据不足** | 趋势图标签和数组各填 2 个锚点（当前年份 + 预估），图表下标注 `"[C 级] 数据为模型推断"` |
| **竞品少于 3 个** | 多余竞品列标注 `"—"`，COMP3_FEATURE_SCORES 填 `[]` 空数组 |

原则：宁可标注"N/A"也不编造数据，宁可留白也不保留 `{{PLACEHOLDER}}`。

---

## Minimum Viable Report (MVR)

当模型上下文不足以完成全部 9 个 Phase（例如对话轮次受限或代码库极大），按以下优先级降级，确保核心价值不被稀释：

### 三档报告

| 档位 | 条件 | 包含 | 省略 |
|------|------|------|------|
| **完整报告**（Full） | 上下文充足 | 8 章 + 附录，5 张图表 | 无 |
| **精简报告**（Compact） | 上下文紧张 | 8 章文字 + 附录，保留代码质量雷达图 + 竞争力雷达图（2 张） | 市场趋势柱状图、竞品对比柱状图、护城河雷达图 → 改为表格+文字描述 |
| **核心报告**（Core） | 上下文极紧 | 第 1/2/7 章 + 附录 = 执行摘要 + 概览 + 投资决策 | 第 3-6 章、第 8 章改为 H3 级要点各 3-5 条 |

### 降级决策点

- Phase 1 完成后评估代码库规模。如果 >500 文件且项目复杂度高，直接在 Phase 1 末尾向用户确认："项目规模较大，预计生成精简报告（Compact），是否接受？"
- Phase 4 市场调研如果连续 3 次 WebSearch 无有效结果 → 第 4 章降级为"市场数据暂缺"精简版。
- **绝对不可跳过的章节**：第 1 章（执行摘要）和第 7 章（投资决策）。这是报告的骨架。

---

## Report Quality Guidelines

- **方法论锚定**：每个维度套用对应方法论，不得自由心证
- **Kill Criteria 强制**：命中否决条件自动降档，标注触发规则编号
- **魔鬼代言人**：判定前先写反方论点，无法反驳时不得高于"谨慎"
- **置信度标注**：每个评分附 [高]/[中]/[低]
- **数据溯源强制**：所有外部数据标注 [源N]
- **决策导向**：报告必须回答"该不该继续迭代"
- **量化优先**：数字 > 文字描述
- **反和稀泥**：禁止"前景广阔""值得期待""有一定优势"等无信息量表述
- **中文输出**：简体中文

---

## Scoring System

| 维度 | 权重 | 数据来源 |
|------|------|---------|
| 代码质量 | 20% | 六维评估 + 竞品对标 |
| 赛道位置 | 25% | 市场规模 + 赛道阶段 + 风向 |
| 差异化程度 | 25% | 同质化指数 + 护城河 |
| 商业潜力 | 20% | 商业模式 + 收入潜力 |
| 团队/执行力 | 10% | 代码推断 + 活跃度 |

综合评分 = Σ(各维度 × 权重)，1-10 分。

---

## Resources

### references/
- `code_quality_metrics.md` — 代码质量评估体系。Phase 2 加载。
- `report_framework.md` — 报告内容框架，8+1 章节定义，含设计风格指引。Phase 3-9 参考。

### assets/
- `report_template.html` — HTML 模板，黑白硬朗风格，Chart.js 5 张图表，8 章 + 附录结构。Phase 9 读取并填充。
