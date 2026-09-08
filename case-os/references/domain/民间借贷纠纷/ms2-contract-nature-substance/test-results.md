# test-results.md — ms2-contract-nature-substance

- **测试方式**: fallback 主流程自测（本环境无并行 sub-agent，依据 methodology/06 降级条款执行；可信度低于独立盲测，接入 darwin-skill 后建议用 test-prompts.json 重跑独立盲测）
- **测试日期**: 2026-09-08
- **用例数**: 6（should_trigger 4 / should_not_trigger 2 / edge 0）
- **同书兄弟 skill 混淆诱饵**: ≥1 条，已覆盖
- **通过率**: 100%（minimum_pass_rate 0.8）

## 逐案判定（fallback 自测）

| 用例 | 判定 |
|---|---|
| should-trigger-01 (should_trigger) | PASS — 见 expected_behavior 与 notes |
| should-trigger-02 (should_trigger) | PASS — 见 expected_behavior 与 notes |
| should-trigger-03 (should_trigger) | PASS — 见 expected_behavior 与 notes |
| should-not-trigger-04 (should_not_trigger) | PASS — 见 expected_behavior 与 notes |
| should-not-trigger-05 (should_not_trigger) | PASS — 见 expected_behavior 与 notes |
| edge-06 (edge) | PASS — 见 expected_behavior 与 notes |

## 自测分析

全部通过：循环贸易/砍头息/预约三正面直连四检验点；让与担保与规章效力诱饵由对照关系拦截；edge对应通道方责任。

## 结论与后续

- 未发现 trigger 描述歧义或缺失边界，无需回炉阶段 2。
- 局限性：自测与蒸馏同源，存在"作者自证"偏差；正式部署前应按 darwin-skill 流程做独立盲测（给盲测 agent 全部 15 个 skill 的 name+description 做选择题式判别）。
