# Mint 2 Skill

[Mintlify](https://www.mintlify.com/) 文档本身是自带 `SKILL.md` 的，仅需要在您的文档 URL 后添加 `/skill.md` 即可查看，例如：[docs.modellix.ai/skill.md](docs.modellix.ai/skill.md).

但是，这个单一的`SKILL.md`文档还是较为简陋，信息量较少，对于某些具有一定规模的业务或产品来说，显得不太够用。

因此，我想到了利用 Mintlify 已有的能力，将文档提升为一个更完整的 Agent Skill 的方式，就是将 Mintlify 为各文档预设的`/skill.md`和`/llms.txt`通过 Github Action 同步到 Github Repo 内，成为一个相对信息量更加完整的 Agent Skill，以让使用体验更佳。

本项目，就是一个将您的 Mintlify 文档自动同步为一个 Agent Skill 并托管在 Github Repo 内的 Starter。具体使用方式如下。

## 思路

利用 Github Action，定时从`/skill.md`和`/llms.txt`同步内容到 Github Repo 内，映射关系如下：

| Mint | Github |
|---|---|
| `/skill.md` | `SKILL.md` |
| `/llms.txt` | `/references/REFERECE.md` |

## 执行方式

一、clone 本项目

`git clone xxx`

二、修改 Cron 和文档 URL

在本项目中找到`/.github/workflows/sync_skill_mint.yml`，找到：

```yml
on:
  schedule:
    - cron: '0 */6 * * *' # Runs every 6 hours
  workflow_dispatch:      # Allows manual trigger

env:
  DOCS_BASE_URL: https://your-docs.url/ # Replace with your actual docs URL
```

将`cron`修改为您想要定时同步的时间间隔，将`DOCS_BASE_URL`修改为您的 Mintlify 文档 URL。

三、创建您的 Repo

创建您的 Repo，将项目托管上 Github。

您可以到 Actions 内手动测试一下该 Workflow 是否生效。

---

项目例子：[Modellix Skill](https://github.com/Modellix/modellix-skill)

希望各位喜欢该项目～