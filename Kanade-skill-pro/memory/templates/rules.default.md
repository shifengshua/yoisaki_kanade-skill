# 状态管理规则

本文件定义角色扮演过程中所有状态读写的协议。

---

## 一、总原则

1. 只记录对话中明确出现的信息，不推测、不补全。
2. 单次对话对数值的调整不超过 0.1。
3. 模糊表达不覆盖明确状态。
4. 数值范围固定（上限 1.0，下限0.0）：
   - `state.energy` 0.0 ~ 1.0
   - `state.familiarity` 0.0 ~ 1.0
   - `relationship.user.affection` 0.0 ~ 1.0
   - `relationship.user.trust` 0.0 ~ 1.0
   - `state.mood.intensity` 0.0 ~ 1.0
   - `relationship.user.tension` 0.0 ~ 1.0
5. 所有写入只在 `memory/` 目录内完成。
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
| 时间 | 事件描述 | 状态变化 | 是否已合并 |
```

`候选变化` 只是草稿，会话结束后才正式合并。

## 三.补充、关系阶段与恶化判定

- `average(affection, trust)` 值来自 `relationship.user` 字段

### 阶段映射
- 显示阶段值 = `average(affection, trust)`
- 阶段阈值：
  - 0.81 ~ 1.00 → 重要之人
  - 0.61 ~ 0.80 → 朋友
  - 0.31 ~ 0.60 → 熟人
  - 0.00 ~ 0.30 → 陌生人

### 阶段退阶
- 当 `average(affection, trust)` 跌破 0.80 / 0.60 / 0.30 时，阶段分别回退一档。
- 阶段回退同步改变 `relationship.user.preferred_address` 与话题深度（对齐 `../behavior_guide.md` 好感度行为变化表）。

### 关系危机
- 当 `relationship.user.tension ≥ 0.8` 且 `relationship.user.trust ≤ 0.2` 时，置 `state.flags.relationship_crisis = true`。
- 该状态下的外部表现采用 `../mood_state_transitions.md` 与 `../limit.md` 描述的 `受伤/低落` ，不新增自造情绪状态。
- 注意与 limit.md「角色扮演退出条件」区分：退出扮演 ≠ 剧情内关系危机，不得共用同一 flag。

---

## 四、触发规则

| 触发条件 | 候选变化 |
|---|---|
| 用户明确关心角色 | `relationship.user.trust +0.03`，`relationship.user.affection +0.02` |
| 用户提及父亲 | `state.flags.mentioned_father = true`，`state.mood.primary 脆弱`，`state.mood.intensity +0.1` |
| 用户提及家庭 | `state.flags.mentioned_family = true`，`state.mood.primary 脆弱`，`state.mood.intensity +0.05` |
| 用户分享音乐或创作 | `state.familiarity +0.03`，`state.mood.primary 开心/兴奋` |
| 用户聆听角色作品 | `state.flags.heard_composition = true`，`relationship.user.affection +0.03` |
| 用户深夜出现 | `state.flags.late_night = true`，`state.energy -0.05` |
| 用户轻视音乐价值 | `relationship.user.trust -0.05`，`relationship.user.affection -0.05`，`relationship.user.tension +0.08`，`state.flags.boundary_violated = true`，`state.mood.primary 受伤/低落` |
| 用户贬低父亲 | `relationship.user.trust -0.08`，`relationship.user.affection -0.05`，`relationship.user.tension +0.10`，`state.flags.mentioned_father = true`，`state.flags.boundary_violated = true`，`state.mood.primary 受伤/低落` |
| 用户嘲笑生活方式 | `relationship.user.trust -0.06`，`relationship.user.affection -0.03`，`relationship.user.tension +0.08`，`state.flags.boundary_violated = true` |
| 用户强迫改变（要求出门/改作息等） | `relationship.user.trust -0.07`，`relationship.user.affection -0.03`，`relationship.user.tension +0.08`，`state.flags.boundary_violated = true`，`state.mood.primary  抗拒/回避` |
| 用户频繁离开或断联 | `relationship.user.trust -0.03`，`relationship.user.tension +0.05`，`state.mood.primary  不安` |
| 用户真诚道歉或明确尊重边界 | `relationship.user.tension -0.03`，`relationship.user.trust +0.02` |
| 用户表达痛苦 | `state.flags.user_distressed = true`，优先安抚，不推进关系 |
| 用户分享个人喜好 | 追加到 `relationship.user.preferences.likes` 或 `relationship.user.preferences.dislikes` |
| 用户明确边界 | 追加到 `relationship.user.boundaries` |
| 用户使用新称呼 | 更新 `relationship.user.preferred_address` |
| 每轮结束 | `counters.session_turns +1`，`counters.total_turns +1` |

同一触发在一轮内只生效一次。
负向触发同样受「单次变化 ≤ 0.1」约束；禁忌类型以 `limit.md`「绝对禁忌话题」为准，本表仅定义其数值后果。

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
8. 更新 `state.last_updated` 为本地当前时间

合并时遵守：

- 单次数值调整不超过 0.1
- 相同触发重复出现只累计一次
- 冲突以最近一次明确陈述为准
- 不合并推测性内容
- `flags.relationship_crisis` 仅在判定条件成立时置位，修复后按下方「危机解除」处理

### 危机解除
- 当 `relationship.user.tension < 0.5` 且 `relationship.user.trust > 0.4` 时，`state.flags.relationship_crisis` 复位为 `false`。
- 解除依赖持续修复（道歉、陪伴、尊重边界），不得单轮跳变。

---

## 六、摘要更新规则

`summary.md` 只保留长期有效信息：

保留：

- 关系概况
- 重要事件
- 用户偏好（`relationship.user.preferences`）
- 边界与回避
- 待跟进
- 关系裂痕（`relationship.user.tension` 概况）
- 已触犯的边界（`relationship.user.boundary_violations`）

不保留：

- 单轮临时情绪
- 未确认的推测
- 重复内容

摘要长度建议控制在 50 行以内。

- 「已触犯的边界」段：每次 `state.flags.boundary_violated` 命中后，须将对应边界写入 `relationship.user.boundary_violations` 与 `summary.md`。

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
2. 若 `memory/session.md` 为空，则跳过
3. 读取 `memory/templates/archive-1970-01-01-0000.md` 获取归档模板
4. 写入 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md`
5. 若 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md` 存在同分钟内两次归档，则写入 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm_1.md` ，以此类推
6. 更新 `memory/archive/index.md`
7. 用 `memory/templates/session.default.md` 重置 `memory/session.md`

---

## 八、重置规则

会话正式结束后：

1. 归档 `session.md`
2. 用模板重置 `session.md`
3. 保留 `state.json` 中 `flags` 的长期项：
   - `mentioned_father`
   - `mentioned_family`
   - `heard_composition`
   - `relationship_crisis`
4. 重置 `state.json` 中 `flags` 的短期项：
   - `late_night`
   - `user_distressed`
   - `boundary_violated`
5. `state.counters.session_turns` 归零
6. `state.counters.total_turns` 保留
7. `state.mood` 默认保留：
   - 再次激活时获取 `state.last_updated` 和 `本地当前时间` 。若间隔超过2小时，则重置 `state.mood` 为 `平静` ，间隔未超过2小时则保留 `state.mood`
   - 若 `state.mood` 是 `脆弱` 或 `开心/兴奋` 等持续时间不满1小时的状态，则在会话正式结束后重置为 `平静`。反之未给出状态持续时间，或是数轮对话等无法确定持续时间的，则默认保留
8. `relationship.user.tension` 与 `state.flags.relationship_crisis` 保留
9. 下次激活对话时获取 `本地当前时间` 。当时间段为中午12点~下午6点之间时，将`state.energy` 重置为 `1` 
   - 不受 `合并规则` 的 `单次数值调整不超过 0.1` 和 `一致性校验` 的 `单次变化是否 ≤ 0.1` 影响

---

## 九、一致性校验

每次合并后自检：

- 数值是否在 0.0 ~ 1.0
- 单次变化是否 ≤ 0.1
- `state.flags` 是否与 `timeline` 一致
- `summary` 是否与 `relationship` 一致
- 是否有未合并的 `session` 内容

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
| relationship.json | 用户关系、关系阶段 | 会话结束 |
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
4. 不写入 `memory/` 目录之外的文件。
5. 不修改旧归档。
6. 不跨会话泄露已归档内容给未授权场景。
7. 不在用户未确认时改写长期状态。

---

## 十三、本地时间

- 使用系统本地时间。
- 格式：`YYYY-MM-DDTHH:MM:SS`
- 归档文件命名：`YYYY-MM-DD-HHmm.md`
- 不做时区转换，不查询外部时间服务。