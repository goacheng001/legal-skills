# 劳动实体 skill 路由表（ldzy · 《劳动争议二十讲》蒸馏）

> 本文件由 labor-os 及子 skill **强制引用**。总控只调度，实体判断执行对应 `ldzy-*` skill 的 **E 步骤**（并遵守其 B 边界）。  
> 源路径：`~/.claude/skills/ldzy-*/SKILL.md`（软链至仓颉 books）。  
> 索引：`~/Codex/仓颉/books/laodong-zhengyi-ershijiang-2025/INDEX.md`

---

## 0. 调用铁律

1. **先红线后实体**：已加载 `redlines-labor.md` 与立场/时效后，才可调用 ldzy。  
2. **按争点点名，不整包加载**：一次最多并行 1–3 个 ldzy，避免上下文爆炸。  
3. **E 步骤可核验**：产出须能看出认定四规则 / 通知事由锁定 / N·2N 算式 / 加班四闸等痕迹。  
4. **法源时效**：ldzy 含 2025 书中口径；引用地方纪要须标属地；《司法解释二》等以**正式文本**为准。  
5. **不完全劳动关系**（`ldzy-complete-incomplete-triage`）：B 标明倡议成分，**禁止**写成全国强制法。  
6. **竞业**：本轮未挂独立 ldzy；涉及竞业仍用 knowledge 主题库 + 人工。

---

## 1. 争点信号 → ldzy 路由

| 争点/用户信号 | 主 skill | 可组合 |
|---|---|---|
| 是不是劳动关系、合作/承揽名实、平台骑手、多层转包告谁 | `ldzy-labor-relation-id` | `ldzy-special-forms-subjects`；边界弱从属 → `ldzy-complete-incomplete-triage` |
| 学生/超龄/派遣外包/混同/两不找/董监高 | `ldzy-special-forms-subjects` | `ldzy-labor-relation-id` |
| 解除/终止合法吗、末位淘汰、违纪解除、通知书事由 | `ldzy-dismissal-review` | 规章 → `ldzy-work-rules-discipline`；调岗 → `ldzy-job-transfer-dual-gate`；金额 → `ldzy-severance-calc` |
| 经济补偿 N、违法解除 2N、分段/合并年限 | `ldzy-severance-calc` | 先定性解除 → `ldzy-dismissal-review` |
| 加班费、包薪、值班、隐形加班、不定时审批 | `ldzy-overtime-compliance` | 工时制度条款审查可并 `ldzy-work-rules-discipline` |
| 规章民主程序/公示/罚款/39 条 2 项 | `ldzy-work-rules-discipline` | 解除后果 → `ldzy-dismissal-review` |
| 单方调岗、不胜任调岗降薪 | `ldzy-job-transfer-dual-gate` | 随后解除 → `ldzy-dismissal-review` |
| 未签书面合同二倍工资、无固定期限强制缔约 | `ldzy-written-contract-double-wage` | 关系是否成立 → `ldzy-labor-relation-id` |
| 仲裁能不能收、公积金/精神损害等混提 | `ldzy-case-acceptance-filter` | 通过后 → 定性/实体 |
| 裁审新增请求、一裁终局、举证偏在 | `ldzy-arbitration-litigation-bridge` | 与 `labor-bridge` 程序文书配合 |
| 工伤找谁赔、转包链、私了 | `ldzy-work-injury-chain` | 关系确认 → `ldzy-labor-relation-id` |

---

## 2. 按 labor-os 阶段的强制挂钩

### 争议轨

| 阶段 | skill | 强制 ldzy | 产出痕迹要求 |
|---|---|---|---|
| L1 接案 | `labor-intake` | ① `ldzy-case-acceptance-filter`（请求清单过滤，可简）② **`ldzy-labor-relation-id`（步骤「劳动关系审查」必须按 E 四规则，禁止只写旧三要素口号）** ③ 命中特殊主体/形态 → `ldzy-special-forms-subjects` | 接案报告含：推定/事实优先/形式标志或平台三类之一；特殊主体分型 |
| L2 仲裁 | `labor-arbitration` | 按 S1 请求项查上表路由；金额类 **必须** `ldzy-severance-calc` 或 `ldzy-overtime-compliance` 给出可复核算式；解除类 **必须** `ldzy-dismissal-review` | 申请书/答辩书请求项旁注明所用 ldzy slug |
| L3 裁审 | `labor-bridge` | 窗口与新增请求 → `ldzy-arbitration-litigation-bridge`；起诉状实体段落复用 L2 已挂 ldzy 结论 | 换轨备忘录含终局判断与举证提示 |
| L4 执行 | `labor-execution` | 金额核对可调 `ldzy-severance-calc`（仅复核，不重开实体辩论） | 执行标的与生效主文一致 |

### 顾问轨

| 入口 | skill | 建议/强制 ldzy |
|---|---|---|
| 合同起草/审查 | `labor-contract-design` | 书面/期限/二倍风险 → `ldzy-written-contract-double-wage`；工时加班条款 → `ldzy-overtime-compliance` |
| 制度起草/审查 | `labor-policy-design` | **强制**对照 `ldzy-work-rules-discipline` 三环节+禁止罚款 |
| 离职方案 | `labor-exit-plan` | 路径合法性 → `ldzy-dismissal-review`；成本 → `ldzy-severance-calc`；调岗不胜任路径 → `ldzy-job-transfer-dual-gate` |
| 用工体检 | `labor-audit` | 工时模块 → overtime；制度模块 → work-rules；合同模块 → written-contract；离职档案 → dismissal+severance |

---

## 3. state 建议字段（可选写入 labor-os-state.json）

```json
"ldzy": {
  "invoked": ["ldzy-labor-relation-id", "ldzy-dismissal-review"],
  "last_at": "ISO-8601",
  "notes": "一句话：本阶段实体结论摘要"
}
```

不阻塞旧 state；有则断点恢复可展示「已挂实体 skill」。

---

## 4. 红绿灯（写入类操作）

| 色 | 操作 | 规则 |
|---|---|---|
| 绿 | 只读 ldzy / 生成 intermediate 草稿 | 可自动 |
| 黄 | 将 ldzy 结论写入接案报告/申请书草稿 | 用户可见；终稿仍须「可以出了」 |
| 红 | 改 stance、删除 state、裁判材料写入顾问 knowledge | **禁止自动**；须用户明示 |

---

## 5. 缺失 fallback

| 触发 | 一线 | 兜底 |
|---|---|---|
| `ldzy-*` 未安装/断链 | 报 slug；尝试路径 `~/.claude/skills/<slug>/SKILL.md` | 用 labor-os/knowledge 主题库降级，并首屏警告「实体 skill 缺失，尺度可能漂移」 |
| 争点多到 >3 个 ldzy | 按标的额/时效紧急度排序只跑前 3 | 其余列入「待续实体清单」 |
