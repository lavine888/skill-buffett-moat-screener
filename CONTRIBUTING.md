# 贡献指南

本仓库是一个**硬筛选** Skill：它做的是条件交集，不是软评分。任何改动都必须维持下面这几条不变量，否则不会被合并。

## 项目不变量

1. **Point-in-time**：财报、价格、行业和股票池都按决策日重建；不得引入未来信息。
2. **Fail closed**：缺失、冲突或非法证据返回 `insufficient_data`，**不得填零、不得猜测、不得用最近一期代替**。
3. **可审计**：输出保留公告日期、`if_adjusted`、TTM 组成日期和规则检查结果。
4. **确定性**：相同输入必须产生相同输出；不引入随机性、时间依赖或隐式网络调用。
5. **契约稳定**：JSON 与 Parquet 的输出字段属于公开契约，改动需要同步更新 `production/SKILL.md` 与 `CHANGELOG.md`。

## 欢迎的贡献

- 新的护城河硬规则（附口径说明与边界测试）。
- 现有规则的边界用例、反例和回归测试。
- PandaData 字段口径、缓存、节流与断点续跑行为的修正。
- `references/` 中的口径文档修订。
- 可复现的最小示例。

## 硬性规则

- 测试**不得**访问真实网络；PandaData 相关测试一律使用 fixture 或 mock。
- 不提交 API key、token、账号信息或任何私有数据。
- 不加入买卖指令、收益承诺或推荐性表述。
- 不引入读取环境变量中密钥的脚本。
- 新增依赖需要说明理由，并同步 `requirements.txt` / `requirements-dev.txt`。

## 本地校验

改动提交前请本地跑完整校验，CI 会用同样的命令：

```bash
python -m pip install -r requirements-dev.txt
python -m pytest -q
node scripts/validate-qsh-form.mjs SKILL.md
```

## 提交信息

沿用仓库现有风格（Conventional Commits，描述用中文）：

```text
feat: 发布巴菲特护城河筛选器 v1.2.0
ci: 修正 Linux 测试模块路径
docs: 添加录屏导演稿和发布打包脚本
chore(skill-template): initial template
```

## Pull Request 检查清单

- [ ] `python -m pytest -q` 全部通过。
- [ ] `node scripts/validate-qsh-form.mjs SKILL.md` 通过。
- [ ] 新增规则附边界测试；修改规则附回归测试。
- [ ] 影响输出契约时，同步更新 `production/SKILL.md`、`references/` 与 `CHANGELOG.md`。
- [ ] 没有网络调用、密钥、私有数据或投资建议类表述。

## 免责声明

本项目仅用于研究与教育。贡献内容同样不代表任何官方立场，也不构成投资建议。
