# labor-os × ldzy 挂接变更（2026-09-08）

## 做了什么
1. 安装 12 个 `ldzy-*` skill 软链到 `~/.claude/skills/`（源：仓颉 books/laodong-zhengyi-ershijiang-2025）
2. 新增 `references/ldzy-entity-routing.md` 争点路由 + 阶段强制挂钩 + 红绿灯
3. 更新 `labor-os/SKILL.md`：实体层调度说明、独立触发加载路由表、references 表
4. 更新 `workflow-labor.md`：§四-b 实体层 + 冒烟测 ldzy
5. 更新 `redlines-labor.md`：§1.2b 实体 skill 红线
6. 子 skill 挂钩：
   - intake：步骤3 强制 ldzy-labor-relation-id 四规则
   - arbitration：步骤2 按请求项挂 dismissal/severance/overtime/double-wage 等
   - bridge：步骤2 挂 arbitration-litigation-bridge
   - execution：金额复核可调 severance-calc
   - contract/policy/exit/audit：顾问轨对应 ldzy

## 未做
- 竞业独立 ldzy（次优先）
- labor-os-state.json schema 强制校验 ldzy 字段（仅建议）
- case-os 九步法本体改写（仍运行时复用）

## 回滚
- 删 `~/.claude/skills/ldzy-*` 软链
- 还原本目录 git/备份中的 SKILL 与 references（若有版本管理）
