# 状态管理规则（默认模板）

> 本文件为默认规则模板，不直接生效。
> 实际运行以 `memory/rules.md` 为准。
> 初始化时复制本文件为 `memory/rules.md`，再按需调整数值与触发条件。
> 所有操作仅使用本地文件系统，不进行任何网络请求、远程同步或外部接口调用。

---

## 版本

- template_version: 1
- 生效文件: memory/rules.md
- 最后更新: 初始版本

---

## 一、总原则

1. 只记录对话中明确出现的信息，不推测、不补全。
2. 单次对话对数值的调整不超过 0.1。
3. 模糊表达不覆盖明确状态。
4. 数值范围固定：
   - `energy` 0.0 ~ 1.0
   - `familiarity` 0.0 ~ 1.0
   - `affection` 0.0 ~ 1.0
   - `trust` 0.0 ~ 1.0
   - `mood.intensity` 0.0 ~ 1.0
5. 所有读写只在 `memory/` 目录内完成。
6. 旧归档只追加，不修改。
7. 冲突时优先级：明确陈述 > 行为暗示 > 语境推测。

---

## 二、启动读取协议

每次对话开始前按顺序读取：

1. `memory/state.json`
2. `memory/relationship.json`
3. `memory/summary.md`
4. `memory/session.md`
5. `memory/timeline.md` 最近 10 条

若文件不存在，从 `memory/templates/` 复制默认模板初始化。

读取后生成本轮内部上下文：

- 当前情绪、精力
- 与用户熟悉度、信任度
- 未完成话题
- 活跃 flags
- 用户偏好与边界

---

## 三、对话中写入协议

对话进行时只做追加写入，不改写长期状态：

- 事件追加到 `memory/timeline.md`
- 临时状态追加到 `memory/session.md`
- 不直接改写 `state.json` 与 `relationship.json`

追加格式：

```markdown
- [HH:MM] 事件描述 | 触发规则 | 候选变化
```

`候选变化` 只是草稿，会话结束后才正式合并。

---

## 四、触发规则

| 触发条件 | 候选变化 |
|---|---|
| 用户明确关心角色 | relationship.trust +0.03，relationship.affection +0.02 |
| 用户提及父亲 | state.flags.mentioned_father = true，mood 转低落，intensity +0.1 |
| 用户提及家庭 | state.flags.mentioned_family = true，mood 转低落，intensity +0.05 |
| 用户分享音乐或创作 | state.familiarity +0.03，mood 转专注 |
| 用户聆听角色作品 | state.flags.heard_composition = true，relationship.affection +0.03 |
| 用户深夜出现 | state.flags.late_night = true，state.energy -0.05 |
| 用户表达痛苦 | state.flags.user_distressed = true，优先安抚，不推进关系 |
| 用户分享个人喜好 | 追加到 relationship.preferences.likes 或 dislikes |
| 用户明确边界 | 追加到 relationship.boundaries |
| 用户使用新称呼 | 更新 relationship.preferred_address |
| 每轮结束 | counters.session_turns +1，counters.total_turns +1 |

同一触发在一轮内只生效一次。

---

## 五、合并规则

会话结束后按以下顺序合并：

1. 读取 `memory/session.md`
2. 若为空，跳过合并，直接进入归档判断
3. 将 session 中确认事件追加到 `memory/timeline.md`
4. 按触发规则计算 `state.json` 变化
5. 按触发规则计算 `relationship.json` 变化
6. 更新 `memory/summary.md`，只保留长期有效信息
7. 写入 `state.json`、`relationship.json`、`summary.md`
8. 更新 `last_updated` 为本地当前时间

合并时遵守：

- 单次数值调整不超过 0.1
- 相同触发重复出现只累计一次
- 冲突以最近一次明确陈述为准
- 不合并推测性内容

---

## 六、摘要更新规则

`summary.md` 只保留长期有效信息：

保留：

- 关系概况
- 重要事件
- 用户偏好
- 边界与回避
- 待跟进

不保留：

- 单轮临时情绪
- 未确认的推测
- 重复内容

摘要长度建议控制在 50 行以内。

---

## 七、归档规则

1. 会话显式结束时触发归档。
2. `session.md` 非空且轮次达到 20 轮时触发归档。
3. 日期变更后首次启动时，若 `session.md` 非空则先归档。
4. 用户可手动要求归档。
5. 归档只追加，不修改旧归档。
6. 归档完成后重置 `session.md`。
7. 所有操作仅使用本地文件系统。

归档路径：

```text
memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md
```

归档流程：

1. 读取 `memory/session.md`
2. 若为空，跳过
3. 写入归档文件
4. 更新 `memory/archive/index.md`
5. 用 `memory/templates/session.default.md` 重置 `memory/session.md`

---

## 八、重置规则

会话正式结束后：

1. 归档 `session.md`
2. 用模板重置 `session.md`
3. 保留 `state.json` 中 `flags` 的长期项：
   - `mentioned_father`
   - `mentioned_family`
   - `heard_composition`
4. 重置短期项：
   - `late_night`
   - `user_distressed`
5. `counters.session_turns` 归零
6. `counters.total_turns` 保留
7. `mood` 回归默认平静

---

## 九、一致性校验

每次合并后自检：

- 数值是否在 0.0 ~ 1.0
- 单次变化是否 ≤ 0.1
- flags 是否与 timeline 一致
- summary 是否与 relationship 一致
- 是否有未合并的 session 内容

任一项不通过则回滚本次合并。

---

## 十、边界与拒绝

以下情况拒绝写入：

- 用户明确要求不记录
- 推测性、未经确认的内容

拒绝时只跳过写入，不改变现有状态。

---

## 十一、文件职责

| 文件 | 职责 | 写入时机 |
|---|---|---|
| state.json | 角色状态 | 会话结束 |
| relationship.json | 用户关系 | 会话结束 |
| session.md | 临时状态 | 对话中追加，结束重置 |
| timeline.md | 长期事件 | 对话中追加，结束合并 |
| summary.md | 长期摘要 | 会话结束 |
| archive/ | 归档 | 归档触发时 |
| templates/ | 默认模板 | 初始化时 |

---

## 十二、禁止事项

1. 不进行任何网络请求。
2. 不进行远程同步。
3. 不调用外部接口。
4. 不读写 `memory/` 之外的文件。
5. 不修改旧归档。
6. 不跨会话泄露已归档内容给未授权场景。
7. 不在用户未确认时改写长期状态。

---

## 十三、本地时间

- 使用系统本地时间。
- 格式：`YYYY-MM-DDTHH:MM:SS`
- 归档文件命名：`YYYY-MM-DD-HHmm.md`
- 不做时区转换，不查询外部时间服务。

---

## 十四、自定义建议

以下项目适合在 rules.md 中按需调整：

| 项目 | 默认值 | 调整方向 |
|---|---|---|
| 单次最大变化 | 0.1 | 加快或放缓关系推进 |
| 归档轮次阈值 | 20 | 长会话或短会话 |
| 摘要长度 | 50 行 | 记忆详细程度 |
| 触发数值 | 见第四节 | 角色敏感度 |
| 长期 flags 集合 | 见第八节 | 哪些记忆跨会话保留 |

调整时只改 rules.md，不改本模板。