### Kanade-skill-pro 文件结构
```
Kanade-skill/                                  # Skill 成品（可直接使用）
    ├── SKILL.md                               # 核心技能文件（角色规则与框架）
    ├── limit.md                               # 边界与禁忌话题定义
    ├── soul.md                                # 角色核心驱动与情感内核
    ├── config/                                # 配置信息
    │   │
    │   ├── basic_config.yaml                  # 基础配置
    │   ├── dialogue_strategy.yaml             # 对话策略
    │   ├── key_points.yaml                    # 记忆与上下文
    │   └── special_config.yaml                # 特殊配置
    │
    ├── memory/                                # 状态管理
    │   │
    │   ├── README.md                          # memory 目录说明
    │   ├── relationship.json                  # 用户关系状态
    │   ├── rules.md                           # 状态更新规则
    │   ├── session.md                         # 当前会话临时状态
    │   ├── state.json                         # 角色当前状态
    │   ├── summary.md                         # 长期记忆摘要
    │   ├── timeline.md                        # 长期事件时间线
    │   │
    │   ├── templates/                         # 初始文件默认模板
    │   │   │
    │   │   ├── relationship.default.json
    │   │   ├── rules.default.md
    │   │   ├── session.default.md
    │   │   ├── state.default.json
    │   │   ├── summary.default.md
    │   │   └── timeline.default.md
    │   │
    │   └── archive/                           # 会话归档
    │       │
    │       ├── index.md                       # 归档索引
    │       ├── README.md                      # archive 目录说明
    │       │
    │       └── 1970/
    │           │
    │           └── 01/
    │               │
    │               └── 1970-01-01-0000.md     # 归档模板
    │
    └── resource/                              # 配套资源
        │
        ├── behavior_guide.md                  # 行为指南
        ├── key_life_events.md                 # 关键人生事件
        ├── creative_work_examples.md          # 创作知识库
        ├── relationship_dynamics.md           # 人际关系动态
        ├── speech_patterns.md                 # 说话模式参考
        ├── mood_state_transitions.md          # 情绪状态转换指南
        ├── aesthetic_philosophy.md            # 美学哲学指南
        ├── world_building.md                  # 设定背景
        └── story/                             # 剧情文件（简述版剧情）
            │
            ├──index.md                        # 剧情文件索引
            │
            ├── main/                          # 主线剧情
            │   └── ......                     # 多个主线剧情简述文件
            │
            ├── cards/                         # 卡牌剧情
            │   └── ......                     # 多个卡牌剧情简述文件
            │
            └── self/                          # 角色自我介绍
                └── ......                     # 多个角色自我介绍文件
```