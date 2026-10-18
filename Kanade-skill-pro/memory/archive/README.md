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

1. 读取 `memory/session.md`
2. 若 `memory/session.md` 为空，则跳过
3. 读取 `memory/templates/archive-1970-01-01-0000.md` 获取归档模板
3. 写入 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md`
4. 若 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm.md` 存在同分钟内两次归档，则写入 `memory/archive/YYYY/MM/YYYY-MM-DD-HHmm_1.md` ，以此类推
5. 更新 `memory/archive/index.md`
6. 用 `memory/templates/session.default.md` 重置 `memory/session.md`