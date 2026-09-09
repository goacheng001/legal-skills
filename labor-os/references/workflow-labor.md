# 劳动法OS 工作流图(workflow-labor.md)

> 本文件是 labor-os 总控的一页纸工作流参考。
> 不在 SKILL.md 内重复,SKILL.md 通过链接引用本文件。

---

## 一、双轨全景图

```
                       ┌──────────────────────────┐
                       │  labor-os (总控,菜单)    │
                       └────────────┬─────────────┘
                                    │
                  ┌─────────────────┴─────────────────┐
                  │                                   │
        ┌─────────▼────────┐                 ┌────────▼────────┐
        │   顾问轨(轻状态)  │                 │  争议轨(重状态)   │
        │   project dir    │                 │  case dir +      │
        │ 无 hook / state   │                 │  state.json      │
        └─────────┬────────┘                 └────────┬────────┘
                  │                                   │
      ┌───────────┼────────────┐          ┌───────────┼────────────┐
      │           │            │          │           │            │
   ┌──▼──┐   ┌───▼──┐   ┌────▼───┐  ┌──▼──┐    ┌───▼──┐    ┌────▼───┐
   │合同 │   │制度   │   │ 离职    │  │接案  │    │仲裁   │    │裁审衔接 │
   │设计 │   │设计   │   │ 方案    │  │审查  │    │程序   │    │L3      │
   │LHD  │   │LPD   │   │ LEP    │  │LI   │    │LA    │    │LB      │
   └──┬──┘   └───┬──┘   └────┬───┘  └──┬──┘    └───┬──┘    └────┬───┘
      │           │           │         │           │           │
      │       ┌───▼───────────▼──┐      │      ┌────▼───┐       │
      │       │   labor-audit    │      │      │  labor- │       │
      │       │   (编排器)        │      │      │execution│      │
      │       └──────────────────┘      │      │  L4    │       │
      │                                  │      └────────┘       │
      │                                  │           │           │
      │                                  │           ▼           │
      │                                  │     labor-os-state   │
      │                                  │     .json (写入)     │
      │                                  │     + case-os 助手   │
      │                                  └──────────────────────┘

  LHD: labor-contract-design
  LPD: labor-policy-design
  LEP: labor-exit-plan
  LI : labor-intake
  LA : labor-arbitration
  LB : labor-bridge
```

---

## 二、争议轨九步法衔接(运行时)

```
┌──────────────────────────────────────────────────────────┐
│            case-os 案件目录结构(既存)                     │
│   CLAUDE.md(案件大脑)  LOG.md(intermediate/)             │
└──────────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────┐
│  L1: labor-intake(独立触发保障先行)                       │
│     - 三选一立场 + 时效红线首屏预警                          │
│     - 强制 ldzy-labor-relation-id（四规则）                 │
│     - 写 labor-os-state.json                               │
└──────────┬───────────────────────────────────────────────┘
           ▼
┌──────────────────────────────────────────────────────────┐
│  复用 case-os 九步法(运行时,劳动OS不写 case-os-state.json) │
│     S1 固定请求 ─► S2 依据 ─► ... ─► S9 预测                │
└──────────┬───────────────────────────────────────────────┘
           ▼
┌──────────────────────────────────────────────────────────┐
│  L2: labor-arbitration                                     │
│     - 代理人立场:申请书/答辩书 + 证据目录 + 时限表             │
│     - 裁判者立场:焦点 + 发问 + 三性 + 说理(用户先定调)        │
└──────────┬───────────────────────────────────────────────┘
           ▼
┌──────────────────────────────────────────────────────────┐
│  L3: labor-bridge(用户提交裁决结果后)                       │
│     - 15/30 日窗口倒计时                                    │
│     - 时效抗辩跨阶段提示                                     │
│     - 民事起诉状 / 撤销裁决申请书 / 接受确认                  │
└──────────┬───────────────────────────────────────────────┘
           ▼
┌──────────────────────────────────────────────────────────┐
│  L4: labor-execution(裁决/判决生效后)                      │
│     - 2 年执行期限倒计时                                    │
│     - 执行申请书 + 财产线索清单                              │
│     - 推进策略(查/冻/扣/划、失信、限高、中止)                  │
└──────────────────────────────────────────────────────────┘
```

---

## 三、顾问轨三步漏斗

```
┌─────────────────────┐
│  labor-audit(总览)   │  ← 一份用工体检报告,6模块扫描
│  (编排器,30天项目)   │
└──────────┬──────────┘
           ▼ 各子skill被依次调用(审查模式)
   ┌───────┴───────┐
   ▼               ▼
┌────────┐    ┌────────┐         ┌────────────┐
│ labor- │    │ labor- │         │  社保/工时/ │
│contract│    │policy- │         │  假期/离职  │
│design  │    │design  │         │  专项扫描   │
│(审查)  │    │(审查)  │         │ (audit内嵌) │
└────┬───┘    └────┬───┘         └──────┬─────┘
     │             │                   │
     ▼             ▼                   ▼
  13域挑坑       5项硬检查+模块挑坑      6项专项
```

顾问轨并行:
- 客户**直接**要做合同起草 → `labor-contract-design`(起草模式),不走 audit;
- 客户**直接**要做制度审查 → `labor-policy-design`(审查模式);
- 客户**直接**要做离职方案 → `labor-exit-plan`;
- 客户要做**总体体检** → `labor-audit`(编排器)。

---

## 四、各skill输入输出(I/O)一页纸

| skill | 模式 | 输入 | 输出(产出物) |
|-------|------|------|--------------|
| labor-os | 总控 | 触发词/选菜单 | 调度 + 断点恢复 |
| labor-intake | 接入 | 案件事实摘要 | `L1-接案审查/接案审查报告.md` + `labor-os-state.json` |
| labor-arbitration | 代理人 | L1 + 九步法产物 | `L2-仲裁程序/申请书/答辩书.md` + 证据目录 + 时限表 + `labor-os-state.json` |
| labor-arbitration | 裁判者 | L1 + 双侧九步法产物 | `L2-仲裁程序/焦点归纳.md` + 发问 + 三性 + **说理(用户定调后)** |
| labor-bridge | 换轨 | 裁决结果 + 送达日 | `L3-裁审衔接/` 起诉状 / 撤裁申请书 / 接受确认 + state 更新 |
| labor-execution | 执行 | 生效法律文书 | `L4-执行/执行申请书.md` + 财产线索清单 + 推进记录 + state 更新 |
| labor-contract-design | 起草 | 11项要素 | `deliverables/劳动合同_XX.md` |
| labor-contract-design | 审查 | 客户合同 | `deliverables/合同审查意见.md` |
| labor-policy-design | 起草 | 6项要素 | `deliverables/考勤管理制度_XX.md` 等 |
| labor-policy-design | 审查 | 客户制度 | `deliverables/制度审查意见.md` |
| labor-exit-plan | 离职方案 | 员工画像 + 八路径对比 | `deliverables/离职方案_XX.md` + 失败预案(转争议轨) |
| labor-audit | 体检 | 6类资料 | `deliverables/用工体检报告_XX.md` + 整改优先级 + 预算 |

---

## 四-b、实体 skill 层（ldzy · 2026-09 挂接）

> 完整路由表: `ldzy-entity-routing.md`（强制引用）。此处只给全景。

```
labor-os / 子 skill（流程）
        │  按争点点名
        ▼
┌──────────────────────────────────────────┐
│  ldzy-* 实体裁判技能（E 步骤可核验）        │
│  P0: relation-id / dismissal / severance │
│      / overtime                          │
│  P1: incomplete / work-rules / transfer  │
│      / double-wage / acceptance / bridge │
│  P2: injury / special-forms              │
└──────────────────────────────────────────┘
```

**阶段强制挂钩（摘要）**:

| 阶段 | 至少调用 |
|------|----------|
| L1 intake | `ldzy-labor-relation-id`（取代「仅三要素口号」）; 请求过滤可用 `ldzy-case-acceptance-filter`; 特殊主体 → `ldzy-special-forms-subjects` |
| L2 arbitration | 按请求项路由 dismissal / severance / overtime / written-contract 等; 文书旁注 slug |
| L3 bridge | `ldzy-arbitration-litigation-bridge` |
| L4 execution | 金额复核可调 `ldzy-severance-calc` |
| 顾问 contract | written-contract + overtime（条款相关时） |
| 顾问 policy | **强制** `ldzy-work-rules-discipline` |
| 顾问 exit | dismissal + severance (+ transfer 若走不胜任调岗) |
| 顾问 audit | 分模块挂 overtime / work-rules / written-contract / dismissal+severance |

**安装冒烟**（ldzy 软链）:

```bash
for s in ldzy-labor-relation-id ldzy-dismissal-review ldzy-severance-calc ldzy-overtime-compliance; do
  test -f ~/.claude/skills/$s/SKILL.md && echo OK $s || echo FAIL $s
done
test -s ~/.claude/skills/labor-os/references/ldzy-entity-routing.md && echo OK routing
```

---

## 五、与 case-os 的边界

| 能力 | 归属 |
|------|------|
| 九步法(S1-S9)的产物生成 | **case-os**(劳动OS 在运行时复用) |
| 案件目录骨架(CLAUDE.md / LOG.md / _archive/) | **case-os** |
| case-os-state.json 的写入 | **case-os**(其 schema fail-closed;劳动 OS 不写) |
| 劳动特有产物(L1-L4 报告、申请书、答辩书、起诉状等) | **labor-os** |
| labor-os-state.json 的读写 | **labor-os**(劳动OS 自己的伴生状态) |
| 立场、时效一裁终局浙江口径 | **labor-os** |
| 一般诉讼程序(一审/二审/再审) | **case-os**(劳动OS 在 L3 移交) |

---

## 六、安装后冒烟测试命令(建议)

```bash
# 0. 自动探测 skill 根(兼容 ~/.claude/skills、~/.agents/skills 与软链)
SKILL_ROOT="$(ls -d ~/.claude/skills/labor-os ~/.agents/skills/labor-os 2>/dev/null | head -1)"
[ -z "$SKILL_ROOT" ] && { echo "FAIL: labor-os 未安装"; exit 1; }
echo "labor-os 根: $SKILL_ROOT(真实路径: $(cd "$SKILL_ROOT" && pwd -P))"

# 1. 总控+8子skill 在位
for s in labor-os labor-intake labor-arbitration labor-bridge labor-execution \
         labor-contract-design labor-policy-design labor-exit-plan labor-audit; do
  ls -ld "$SKILL_ROOT/../$s" >/dev/null || echo "FAIL: $s 缺失"
done
	# 1b. ldzy 实体 skill（至少 P0）
	for s in ldzy-labor-relation-id ldzy-dismissal-review ldzy-severance-calc ldzy-overtime-compliance; do
	  test -f "$SKILL_ROOT/../$s/SKILL.md" || echo "FAIL: $s 缺失"
	done
	test -s "$SKILL_ROOT/references/ldzy-entity-routing.md" || echo "FAIL: ldzy-entity-routing.md"

# 2. 知识库挂载(期望13库)
[ "$(ls "$SKILL_ROOT/references/knowledge/" | wc -l | tr -d ' ')" = "13" ] \
  && echo "OK: 13库" || echo "FAIL: 库数不符"

# 3. 红线/工作流/索引三件套非空
test -s "$SKILL_ROOT/references/redlines-labor.md" \
  && test -s "$SKILL_ROOT/references/workflow-labor.md" \
  && grep -q "效力状态" "$SKILL_ROOT/references/knowledge/劳动社保法规索引.md" \
  && echo "OK: 契约三件套"

# 4. 模拟运行
# 争议轨案件目录输入:"继续" → labor-intake;"劳动仲裁" → labor-arbitration
# 顾问轨输入:"劳动合同审查" → labor-contract-design;"用工体检" → labor-audit
```

> 更省事:直接跑固化工具 `bash 劳动OS蒸馏/scripts/smoke-test.sh`(同一逻辑,FAIL 不合入规矩的执行体)。

---

*本文件与 `labor-os/SKILL.md` 配套安装;不在主 SKILL.md 内重复以保持正文 ≤300 行。*
