# Mint 2 Skill

[Mintlify](https://www.mintlify.com/) docs come with a built-in `SKILL.md` — simply append `/skill.md` to your docs URL to view it, e.g. [docs.modellix.ai/skill.md](docs.modellix.ai/skill.md).

However, this single `SKILL.md` file is fairly basic and contains limited information, which may not be sufficient for businesses or products of a certain scale.

To address this, I came up with a way to leverage Mintlify's existing capabilities to turn your docs into a more complete Agent Skill. The idea is to sync the `/skill.md` and `/llms.txt` endpoints that Mintlify provides for each docs site into a GitHub repo via GitHub Actions, resulting in a more information-rich Agent Skill for a better experience.

This project is a starter template that automatically syncs your Mintlify docs into an Agent Skill hosted in a GitHub repo. Usage instructions are below.

## Approach

A GitHub Action periodically syncs content from `/skill.md` and `/llms.txt` into the GitHub repo. The mapping is as follows:

| Mint | GitHub |
|---|---|
| `/skill.md` | `SKILL.md` |
| `/llms.txt` | `/references/REFERENCE.md` |

## Getting Started

### 1. Clone this project

`git clone https://github.com/alen-hh/mint-2-skill.git`

### 2. Configure the cron schedule and docs URL

In this project, locate `/.github/workflows/sync_skill_mint.yml` and find:

```yml
on:
  schedule:
    - cron: '0 */6 * * *' # Runs every 6 hours
  workflow_dispatch:      # Allows manual trigger

env:
  DOCS_BASE_URL: https://your-docs.url/ # Replace with your actual docs URL
```

Change the `cron` value to your desired sync interval, and replace `DOCS_BASE_URL` with your Mintlify docs URL.

### 3. Create your repo

Create your own repo and push the project to GitHub.

You can go to the Actions tab to manually test whether the workflow is working correctly.

---

Example project: [Modellix Skill](https://github.com/Modellix/modellix-skill)

Hope you enjoy this project!