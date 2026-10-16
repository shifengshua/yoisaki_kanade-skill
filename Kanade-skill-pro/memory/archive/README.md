# archive 目录说明

本目录用于存放已结束会话的本地归档。

## 命名规则

YYYY-MM-DD-HHmm.md

## 目录规则

按 年/月/ 存放。

## 归档触发

1. 会话显式结束
2. session.md 非空且达到轮次阈值
3. 日期变更
4. 手动归档

## 归档流程

1. 读取 memory/session.md
2. 若为空则跳过
3. 写入 archive/YYYY/MM/YYYY-MM-DD-HHmm.md
4. 更新 archive/index.md
5. 合并状态到 state.json、relationship.json、summary.md、timeline.md
6. 用 templates/session.default.md 重置 session.md