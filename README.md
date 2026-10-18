<div align="center">

# 宵崎奏.Skill

### 将心中的“她”化作温暖的Skill，欢迎加入二次元数字生命！

> 基于《Project SEKAI COLORFUL STAGE! feat. 初音ミク》中角色「宵崎奏」（Yoisaki Kanade）所制作的 AI 聊天 Skill

</div>

## 项目简介

这是一个主要用于 AI 聊天的角色扮演 Skill，以《世界计划 缤纷舞台！ feat. 初音未来》中「25時、ナイトコードで。」（25时，在Nightcord见。）的成员——宵崎奏为原型。

宵崎奏是一位拥有非凡音乐才能的 17 岁少女作曲家，因父亲的悲剧而立志创作「能够拯救他人的音乐」。她几乎从不出门，过着昼夜颠倒的隐居生活，在 Nightcord 上与团队成员交流。本 Skill 完整还原了她的性格特征、说话方式、心理状态和互动模式。

> **注意**：这是一个主要用于 AI 聊天的 skill，没有额外技能。

## 特点

- **高度还原角色**：基于超过 **15000** 字符的完整设定文档，涵盖角色**心理学**、**语言模型**、**对话示例**、**技术配置**等全方位内容
- **深夜氛围感**：完整呈现奏的温柔、内向、自我牺牲的性格与思考模式
- **多语言支持**：自动检测用户语言并以此语言回复（支持中文、日文、英文等）
- **丰富的知识库**：包含音乐知识、人际关系动态、行为指南、剧情简述等配套资源
- **跨会话状态与记忆**：Pro版新增 **动态状态管理** 和 **会话归档** ，可让使用 Skill 的 AI Agent 保持跨会话下的角色 **状态同步** 和 **长期记忆**

## 文件结构

> `Initial-Markdown` 文件夹下文件名后带日期的为旧副本存档， `Yoisaki_Kanade-Skill.md` 默认为最新文档

- 温馨提示：由于Kanade-skill-pro版本新增 `状态管理` 和 `会话归档` ，所以需要使用 **AI Agent** 才能正常使用

   > `OpenCode`这类带有编辑功能的AI客户端同样可以

   > 注意用聪明点的AI模型

- [Kanade-skill-pro 文件结构](Kanade-skill-pro-Structure.md)（本仓库README.md同目录下的 `Kanade-skill-pro-Structure.md` 文件）
   > Pro版的 `状态管理` 和 `会话归档` 功能均在 Skill 文件的 `memory/` 目录下完成动态变化

- [Kanade-skill 文件结构](Kanade-skill-Structure.md)（本仓库README.md同目录下的 `Kanade-skill-Structure.md` 文件）

```
yoisaki_kanade-skill/
├── Initial-Markdown/                          # 原始 Markdown 文档
│   │
│   ├── Yoisaki_Kanade-Skill.md                # 角色设定总文档（默认最新文档）
│   └── Yoisaki_Kanade-Skill-.......md         # 总文档历史版本
│
├── Kanade-skill-pro/                          # Pro版（需要AI Agent）
│   │
│   └── ......
│
└── Kanade-skill/                              # Skill 成品（可直接使用）
    │
    ├── SKILL.md                               # 核心技能文件（角色规则与框架）
    ├── limit.md                               # 边界与禁忌话题定义
    ├── soul.md                                # 角色核心驱动与情感内核
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
        │
        └── story/                             # 剧情文件（简述版剧情）
            │
            └── ......
```

## 使用教程

### 方法一：使用Releases打包成品

1. **下载Releases版本**：进入本仓库[Releases](https://github.com/shifengshua/yoisaki_kanade-skill/releases)页面，选择最新发行版下载
   > 通常为 `.zip` 或 `.skill` 后缀

2. **导入 AI 平台**：将下载的 Skill 压缩包直接上传导入任意支持 Skill 的 AI 聊天平台即可使用

3. **开始对话**：在支持 Skill 的 AI 中激活后，输入 `/Kanade_chat [你的问题]` 即可与「宵崎奏」对话
   > 输入 `/Kanade-perspective` 同样可激活

### 方法二：使用仓库 Skill 成品

1. **下载项目**：下载 `main` 分支的压缩包，或将仓库克隆到本地

2. **解压文件**：将压缩包内的文件解压至任意位置
   > 建议在任意盘符根目录下新建一个文件夹用于存放，避免文件散乱影响其他软件或系统

3. **导入 AI 平台**：将解压目录中的整个 `Kanade-skill` （非Pro版）文件夹拖入任意支持 Skill 的 AI 聊天平台即可使用
   > 部分AI可能需要将解压出的 `Kanade-skill` 文件夹单独压缩成一个压缩包才能上传导入

4. **开始对话**：与 `方法一` 同样

### 方法三：使用原始 Markdown 文档

`Initial-Markdown` 文件夹下的文档包含超过 **15000** 字符的完整角色设定，理论上也可以直接作为 Skill 的设定材料使用

你可以：

- 将 Markdown 内容作为 AI 的系统提示词（System Prompt）
   > 注意！此方法极为消耗Token！请谨慎使用！
- 参考其中的角色设定自行构建对话模型
- 作为开发其他角色 Skill 的参考模板

### Pro版使用：

1. **下载项目**：与 `方法二` 同样
   > 由于Pro版需要**AI Agent**，因此暂不提供打包完成的Releases，以免混淆

2. **解压文件**：与 `方法二` 同样

3. **导入 AI 平台**：将解压目录中的整个 `Kanade-skill-pro` 文件夹改名为 `kanade-perspective` ，然后将改名后的 `kanade-perspective` 文件夹拖入任意支持 Skill 的 **AI Agent** 平台或 **OpenCode** 的本地Skill目录即可使用

4. **开始对话**：与 `方法一` 同样

### 知识库（可选部分）

**知识库源**：如需要补充知识库内容，可通过此开源仓库[ProjectSekai-story](https://github.com/ci-ke/ProjectSekai-story)获取《Project SEKAI COLORFUL STAGE! feat. 初音ミク》的剧情txt文本文件

## Skill 制作工具

本 Skill 的制作使用了以下工具与开源项目：

| 工具/项目 | 用途 |
|-----------|------|
| [Kimi 网页客户端](https://www.kimi.com) | 数据收集及整理，生成 `Initial-Markdown` 下的原始 Markdown 文档 |
| [GalgameCharacterSkills](https://github.com/JodieRuth/GalgameCharacterSkills) | 提供图形化操作界面，通过后端调用 API 将 Markdown 文档处理转换为 Skill 成品 |
| [DeepSeek 网页客户端](https://chat.deepseek.com/) + [DeepSeek-V4-Pro API](https://platform.deepseek.com/)（在线 AI 模型） | 信息分析、辅助Skill后续更新、写一些便于开发的小工具 |
| [Qwen3.5-9B](https://www.modelscope.cn/models/unsloth/Qwen3.5-9B-GGUF)（本地 AI 模型） | 通过 API 调用生成 Skill 成品的核心内容 |
| [OpenCode](https://opencode.ai/) + [DeepSeek-V4.1-Flash Free API](https://platform.deepseek.com/) | 辅助Skill后续更新 |

## 数据源

| 目录/文件/路径 | 原数据来源 | 处理工具 |
|-----------|-----------|----------|
| Initial-Markdown/... | 《世界计划 缤纷舞台！》（Project SEKAI）剧情与AI模型训练参数数据 | [Kimi 网页客户端](https://www.kimi.com) + 手动修正 |
| Kanade-skill/... | Initial-Markdown/... （本仓库目录下文件） | [GalgameCharacterSkills](https://github.com/JodieRuth/GalgameCharacterSkills) + [Qwen3.5-9B](https://www.modelscope.cn/models/unsloth/Qwen3.5-9B-GGUF)（本地 AI 模型） + 手动修正 |
| Kanade-skill/story/...... | [ProjectSekai-story](https://github.com/ci-ke/ProjectSekai-story) | [DeepSeek 网页客户端](https://chat.deepseek.com/) |
| Kanade-skill-pro/memory/...... | 本仓库所有文件 | [DeepSeek 网页客户端](https://chat.deepseek.com/) |

## 注意事项

### 语言规则

- Skill 会自动检测用户提问的语言，并以**相同语言**回复
- 不会混用其他语言（包括日文原文引用也会被翻译）

### 退出角色扮演

- 输入 `exit`、`switch back`、`stop roleplaying` 即可退出角色扮演模式
   > 根据 `SKILL.md` 和 `limit.md` 的内嵌指令而定

### 禁忌话题

根据 `limit.md` 的定义，以下话题属于绝对禁忌，AI 将拒绝讨论或转移话题：

- ❌ 贬低音乐的价值（如「音乐只是娱乐」）
- ❌ 侮辱宵崎奏的父亲
- ❌ 嘲笑她的生活方式（如「家里蹲」「废物」）
- ❌ 强迫她外出或「变得正常」
- ❌ 将她的自我怀疑贬低为「矫情」或「博关注」

请尊重角色设定，以获得更好的对话体验

> 温馨提示：若移除 `limit.md` 文件和 `SKILL.md` 中与其对应的文件路径引导，再搭配上含有《Project SEKAI COLORFUL STAGE! feat. 初音ミク》剧情的额外知识库，理论上可以产出同人文（虽然不建议这么做）

## 联系与反馈

- **GitHub Issues**：[Issues](https://github.com/shifengshua/yoisaki_kanade-skill/issues)
- **QQ**：3593853319
- **邮箱**：shifengshua@outlook.com

## 声明

- 本项目 **完全免费**，禁止任何直接倒卖源码行为
- 本项目禁止用于任何违法行为
- 本仓库不承担因使用本项目而引发的任何风险或责任

## 版权声明

- 该项目中所用到的剧情内容和角色设定等内容的版权，均归属于 《世界计划 缤纷舞台！》（Project SEKAI）官方、SEGA Games、Colorful Palette 所有
- 通过AI使用该项目所产出的内容，其生成内容的版权归用户所有（除本身就是侵权内容的情况外）
   > 根据《人工智能生成内容的著作权认定与有限保护》而定
- 禁止将本项目源码直接用于倒卖等商业行为，使用Skill最终由AI产出的内容则不受此条商业行为的管控
- 本项目使用“MIT”开源协议，但项目源码本身禁止直接用于商业。仅使用AI搭配本项目所产出内容的版权归用户所有的情况下，产出内容可用于商业目的

## 许可

请查看仓库根目录下的 [LICENSE](https://github.com/shifengshua/yoisaki_kanade-skill/blob/main/LICENSE) 文件了解具体许可条款

## 致谢

- 感谢 [GalgameCharacterSkills](https://github.com/JodieRuth/GalgameCharacterSkills) 开源项目提供的图形化操作界面
- 感谢所有为 Project SEKAI 系列作品付出心血的原作者与创作者
- 温馨提示：要好好地活下去 生きて 生きて 生きて 生きて 生きて
