# gtm-website — a Claude Code skill

Describe your product in plain English; Claude Code builds, polishes, and launches a go-to-market website for it. No coding needed.

It walks you through: installing the design toolkit (impeccable, hallmark, motion-design, Higgsfield, Refero styles) → product interview → picking a look → Next.js + Tailwind + shadcn/ui + lucide icons → GitHub → Vercel → buying a domain and connecting it.

## Install (one time)

1. Install Claude Code: https://docs.claude.com/en/docs/claude-code/overview
2. In your terminal:
   ```bash
   npx skills add aankirz/gtm-website-skill -g -a claude-code -y
   ```
   (Don't have `npx`? Install Node.js from https://nodejs.org first.)
   No-terminal alternative: download `skills/gtm-website/SKILL.md` and put it at `~/.claude/skills/gtm-website/SKILL.md`.

## Use

```bash
mkdir ~/my-website && cd ~/my-website
claude
```
Then type:
```
/gtm-website I'm building <product>. It helps <who> do <what>. I want visitors to <join the waitlist / book a demo / buy>.
```
Claude will guide you step by step. If it asks you to restart, type `/exit`, run `claude` again in the same folder, and type `/gtm-website continue`.

## Costs
Free: GitHub, Vercel (hobby), all skills/libraries. Paid: your domain (~$10–70/yr), Higgsfield AI images/video (optional credits). Claude will always ask before anything costs money.
