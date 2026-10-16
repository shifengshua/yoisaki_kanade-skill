# 状态更新规则

## 基本原则

1. 只记录对话中明确出现的信息。
2. 不推测、不补全、不记录敏感隐私。
3. 单次对话对数值的调整不超过 0.1。
4. 模糊表达不覆盖明确状态。

## 触发规则

| 触发条件 | 状态变化 |
|---|---|
| 用户明确关心角色 | relationship.trust +0.03，relationship.affection +0.02 |
| 用户提及父亲或家庭 | state.flags.mentioned_father 或 mentioned_family 设为 true，mood 转为低落，intensity +0.1 |
| 用户分享音乐或创作 | state.familiarity +0.03，mood 转为专注 |
| 用户深夜出现 | state.flags.late_night 设为 true，energy -0.05 |
| 用户表达痛苦 | state.flags.user_distressed 设为 true，优先安抚，不急于推进关系 |
| 每轮结束 | state.counters.session_turns +1，state.counters.total_turns +1 |

## 合并规则

1. 会话结束后，将 session.md 中确认的事件追加到 timeline.md。
2. 根据 timeline.md 和本规则计算 state.json 与 relationship.json 的变化。
3. 更新 summary.md，只保留长期有效信息。
4. 将旧 session.md 归档到 archive/YYYY-MM-DD-HHmm.md。
5. 用 templates/session.default.md 重置 session.md。
6. 若 session.md 非空，先归档再覆盖。

## 归档规则

1. 会话显式结束时触发归档。
2. session.md 非空且轮次达到 20 轮时触发归档。
3. 日期变更后首次启动时，若 session.md 非空则先归档。
4. 用户可手动要求归档。
5. 归档只追加，不修改旧归档。
6. 归档完成后重置 session.md。

## 归档流程

1. 读取 `memory/session.md`
2. 若为空，跳过
3. 生成路径：`memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md`
4. 写入归档文件
5. 更新 `memory/archive/index.md`
6. 将确认事件合并到 `memory/timeline.md`
7. 更新 `memory/state.json`
8. 更新 `memory/relationship.json`
9. 更新 `memory/summary.md`
10. 用 `memory/templates/session.default.md` 重置 `memory/session.md`