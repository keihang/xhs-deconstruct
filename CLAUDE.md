# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目性质

这是一个 Claude Code skill 项目（小红书博主拆解），不是代码项目。核心文件是 `SKILL.md`（skill 定义）和 `references/`（模板和参考文档）。

## 核心文件

| 文件 | 作用 | 修改频率 |
|------|------|---------|
| `SKILL.md` | skill 主定义，包含工作流程、输出结构、注意事项 | 低 |
| `references/*.md` | 模板、反例清单、质量自检、FAQ | 中 |
| `README.md` | 安装说明、使用方式、命令参考 | 低 |

## 修改 SKILL.md 时

1. 保持在 **500 行以内**（当前 155 行），超了就把内容拆到 `references/`
2. 模板内容放 `references/`，SKILL.md 只引用路径
3. 修改后同步更新 `README.md` 的输出结构和参考资料表格

## 风控约束

- 每次 opencli 操作间隔 **15秒**
- `--limit 50` 获取博主帖子，默认 15 篇会遗漏
- URL 必须带 `xsec_token`，explore URL 会触发安全验证

## 测试方式

用 `/xhs-deconstruct 拆解 博主名` 触发，观察是否按 SKILL.md 定义的 9 个步骤执行。测试用例见 `test-prompts.json`。
