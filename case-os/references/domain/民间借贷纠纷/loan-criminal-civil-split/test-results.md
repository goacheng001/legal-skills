# loan-criminal-civil-split — 压力测试结果
- **测试模式**: 主流程自测（fallback）— 本环境无独立 sub-agent 盲测能力，依 methodology/06 回退；可信度低于独立盲测
- **通过率**: 6/6 = 100%（≥80% 达标；should_not_trigger 诱饵容错 0）
- **触发依据**: description 含'何时调用+何时不调用+中英 trigger 词'；A2 语言信号与 test prompts 逐条对照

| 用例 | 预期 | 自测判定 |
|---|---|---|
| should-trigger-01 | 见 test-prompts.json | PASS |
| should-trigger-02 | 见 test-prompts.json | PASS |
| should-trigger-03 | 见 test-prompts.json | PASS |
| should-not-trigger-01 | 见 test-prompts.json | PASS |
| should-not-trigger-02 | 见 test-prompts.json | PASS |
| edge-01 | 见 test-prompts.json | PASS |

## 失败分析

- 无失败用例。需注意：本结果为构造者自测，存在自我确认偏差；接入 darwin-skill 后应以独立盲测复跑。

## 边界说明

- 各 skill 的 B 段已写明反场景；同书兄弟 skill 的区分在 A2'与相邻 skill 的区分'中成对声明。
