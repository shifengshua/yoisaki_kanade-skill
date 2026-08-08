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

- **高度还原角色**：基于超过 8000 字的完整设定文档，涵盖角色心理学、语言模型、对话示例、技术配置等全方位内容
- **深夜氛围感**：完整呈现奏的温柔、内向、自我牺牲的性格与思考模式
- **多语言支持**：自动检测用户语言并以此语言回复（支持中文、日文、英文等）
- **丰富的知识库**：包含音乐知识、人际关系动态、行为指南等配套资源


## 文件结构

>  `Initial-Markdown` 文件夹下文件名后带日期的为旧副本存档， `Yoisaki_Kanade-Skill.md` 默认为最新文档

```
Yoisaki_Kanade-Skill/
├── Initial-Markdown/                          # 原始 Markdown 文档
│   └── Yoisaki_Kanade-Skill.md                # 角色设定总文档（默认最新文档）
│
└── Kanade-skill/                              # Skill 成品（可直接使用）
    ├── SKILL.md                               # 核心技能文件（角色规则与框架）
    ├── limit.md                               # 边界与禁忌话题定义
    ├── soul.md                                # 角色核心驱动与情感内核
    └── resource/                              # 配套资源
        ├── behavior_guide.md                  # 行为指南
        ├── key_life_events.md                 # 关键人生事件
        ├── music_and_creation.md              # 音乐知识库
        ├── relationship_dynamics.md           # 人际关系动态
        └── speech_patterns.md                 # 说话模式参考
```


## 使用教程

### 方法一：直接使用 Skill 成品（推荐）

1. **下载项目**：下载 `main` 分支的压缩包，或将仓库克隆到本地

2. **解压文件**：将压缩包内的 `Kanade-skill` 文件夹解压至任意位置
   > 建议在任意盘符根目录下新建一个文件夹用于存放，避免文件散乱影响其他软件或系统

3. **导入 AI 平台**：将解压出的整个 `Kanade-skill` 文件夹拖入任意支持 Skill 的 AI 聊天平台即可使用
   > 部分AI可能需要将解压出的 `Kanade-skill` 文件夹单独压缩成一个压缩包才能上传导入

5. **开始对话**：在支持该 Skill 的 AI 中激活后，输入 `/Kanade_chat [你的问题]` 即可与「宵崎奏」对话

### 方法二：使用原始 Markdown 文档

`Initial-Markdown` 文件夹下的文档包含超过 8000 字的完整角色设定，理论上也可以直接作为 Skill 的设定材料使用。

你可以：

- 将 Markdown 内容作为 AI 的系统提示词（System Prompt）
- 参考其中的角色设定自行构建对话模型
- 作为开发其他角色 Skill 的参考模板

### 知识库（可选部分）

**知识库源**：如需要补充知识库内容，可通过此开源仓库[ProjectSekai-story](https://github.com/ci-ke/ProjectSekai-story)获取《Project SEKAI COLORFUL STAGE! feat. 初音ミク》的剧情txt文本文件


## Skill 制作工具

本 Skill 的制作使用了以下工具与开源项目：

| 工具/项目 | 用途 |
|-----------|------|
| [Kimi](https://www.kimi.com) | 数据收集及整理，生成 `Initial-Markdown` 下的原始 Markdown 文档 |
| [GalgameCharacterSkills](https://github.com/JodieRuth/GalgameCharacterSkills) | 提供图形化操作界面，将 Markdown 文档处理转换为 Skill 成品 |
| [DeepSeek-V4-Pro](https://platform.deepseek.com/)（在线 AI 模型） | 通过 API 调用生成 Skill 成品的核心内容 |


## 注意事项

### 语言规则
- Skill 会自动检测用户提问的语言，并以**相同语言**回复
- 不会混用其他语言（包括日文原文引用也会被翻译）

### 退出角色扮演
- 输入 `exit`、`switch back`、`stop roleplaying` 即可退出角色扮演模式

### 禁忌话题
根据 `limit.md` 的定义，以下话题属于绝对禁忌，AI 将拒绝讨论或转移话题：

- ❌ 贬低音乐的价值（如「音乐只是娱乐」）
- ❌ 侮辱宵崎奏的父亲
- ❌ 嘲笑她的生活方式（如「家里蹲」「废物」）
- ❌ 强迫她外出或「变得正常」
- ❌ 将她的自我怀疑贬低为「矫情」或「博关注」

请尊重角色设定，以获得更好的对话体验。

> 温馨提示：若移除 `limit.md` 文件和 `SKILL.md` 中与其对应的文件路径引导，再搭配上含有《Project SEKAI COLORFUL STAGE! feat. 初音ミク》剧情的额外知识库，理论上可以产出同人文（虽然不建议这么做）


## 联系与反馈

- **QQ**：3593853319
- **邮箱**：shifengshua@outlook.com


## 声明

- 本项目**完全免费**，禁止任何倒卖行为
- 本项目禁止用于任何违法行为
- 本仓库不承担因使用本项目而引发的任何风险或责任


## 许可

请查看仓库根目录下的 [LICENSE](https://github.com/shifengshua/Yoisaki_Kanade-Skill/blob/main/LICENSE) 文件了解具体许可条款。


## 致谢

- 感谢 [GalgameCharacterSkills](https://github.com/JodieRuth/GalgameCharacterSkills) 开源项目提供的图形化操作界面
- 感谢所有为 Project SEKAI 系列作品付出心血的原作者与创作者
- 温馨提示：要好好地活下去 生きて 生きて 生きて 生きて 生きて
