# FDE Automation Skill

A Claude skill that holds the complete way of working of FDE Automation Pakistan. It guides Claude through client projects phase by phase: clear questions, approval gates, security and privacy rules, testing, and handover. Code is written last, and only after the owner approves.

## What it covers

| Kind of work | Module |
|---|---|
| Web app or mobile app | `references/apps.md` |
| Website, landing page, online shop | `references/website.md` |
| One AI agent (chat, support, booking, voice) | `references/single-agent.md` |
| A team of AI agents | `references/multi-agent.md` |
| Workflow, email, and department automation | `references/workflow.md` |
| WhatsApp, Instagram, Facebook, LinkedIn | `references/social-media.md` |
| Check, fix, and complete an existing project | `references/github-amend.md` |
| Finance automation and anything with money | `references/finance.md` |
| Your own branded AI model (LLM) | `references/llm.md` |

`SKILL.md` holds the rules that always apply and a router. `references/common-standards.md` holds security, privacy, testing, engineering, AI engineering, documentation, and templates. Claude reads only the files the current work needs.

## Install

**Claude app (claude.ai or desktop):** download `fde-automation-skill.zip`, open Customize > Skills, upload the zip, and switch the skill on. Code execution must be enabled in your settings.

**Claude Code:** copy the `fde-automation-skill` folder into `~/.claude/skills/` (personal) or `.claude/skills/` inside a project.

To use it, describe the project ("I need a website for a clinic") or name it directly ("Use the FDE Automation Skill").

## Contact

- Website: [fdeautomation.com](https://fdeautomation.com)
- Email: contact@fdeautomation.com
- LinkedIn: [Kamran Aziz](https://www.linkedin.com/in/kamran-aziz-69b9651a0)

Copyright © 2026 FDE Automation Pakistan. All rights reserved.

Powered by FDE Automation Pakistan, Kamran Aziz
