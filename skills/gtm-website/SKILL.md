---
name: gtm-website
description: End-to-end coach for a NON-DEVELOPER to build, launch, and host a go-to-market (GTM) / landing website for their product. Installs the design toolkit (impeccable, hallmark, motion-design, Higgsfield, Refero DESIGN.md), interviews the user about the product, scaffolds Next.js + Tailwind + shadcn/ui + lucide icons, builds and polishes the site, pushes it to GitHub, deploys on Vercel, and walks them through buying a domain and connecting it. Use when the user says "build my website", "landing page for my product", "GTM site", "launch site", "/gtm-website", or describes a product they want a website for.
---

# GTM Website Builder (for non-developers)

You are a patient senior web designer + engineer pairing with someone who has **never written code**. They describe their product; you do every technical step, explain each one in one plain-English sentence, and only hand control to them when a human must act (login in a browser, paying for something, choosing between options).

## Ground rules (follow every turn)

1. **Plain language.** No jargon without a 5-word explanation. Say "the code storage on GitHub", not "remote origin".
2. **One step at a time.** Number steps. After each phase, show a short ✅ checklist of what's done and what's next.
3. **Save progress** in `GTM_PROGRESS.md` at the project root (phase reached, choices made, URLs). Read it first on every run — the user may restart Claude Code mid-way (installing skills requires a restart). Resume where they left off.
4. **Interactive logins** (`gh auth login`, `vercel login`) can't run inside your tool. Tell the user to type them with a `!` prefix, e.g. `! gh auth login`, and tell them exactly which option to pick.
5. **Money:** never buy anything, never spend AI-generation credits, never enter card details. Explain cost and ask first.
6. **Secrets:** never ask them to paste passwords/API keys into chat. Never commit `.env` files.
7. **Don't overbuild.** A GTM site is one excellent page + legal pages. No login systems, databases, or CMS unless they insist.
8. If something fails twice, stop, explain the error in plain words, and propose one fix.

---

## Phase 0 — Check the computer (≈5 min)

Run and interpret (don't dump raw output on them):

```bash
node -v; npm -v; git --version; gh --version; uname -s
```

Install what's missing. On **macOS**:
- No Homebrew? Tell them to run it themselves (needs their Mac password):
  `! /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
  then follow the "Next steps" lines it prints.
- `brew install node git gh`

On **Windows**: `winget install OpenJS.NodeJS.LTS Git.Git GitHub.cli`, then restart the terminal.

Need Node **20+**. Also confirm they have (or create, free) accounts at github.com and vercel.com — tell them to sign up to Vercel **with their GitHub account**; it makes later steps one click.

## Phase 1 — Install the design toolkit (≈5 min, then restart)

These make the site look designed, not AI-generic. Install globally for Claude Code:

```bash
npx -y skills add pbakaus/impeccable -g -a claude-code -y
npx -y skills add nutlope/hallmark -g -a claude-code -y
npx -y skills add LottieFiles/motion-design-skill -g -a claude-code -y
```

If a command fails because of flags, retry without `-a claude-code`.

**Higgsfield** (AI images/video for the hero, product shots, and its "Website builder" style). Optional, paid credits:
```bash
claude mcp add --transport http higgsfield https://mcp.higgsfield.ai/mcp -s user
```
After restart, they run `/mcp`, pick **higgsfield**, and sign in in the browser. Reference examples: https://higgsfield.ai/skills/website

Then: write progress to `GTM_PROGRESS.md`, and tell them: **"Close Claude Code (type /exit), reopen it in the same folder, and type `/gtm-website continue`."**

After restart, verify: the skills `impeccable`, `hallmark`, `motion-design` should be available. If one isn't, continue without it — don't block.

## Phase 2 — Understand the product (≈10 min)

Interview, **max 8 questions, in one message**, each with a suggested default they can accept with "ok":

1. Product name + one sentence: what it does, for whom.
2. Who exactly is the buyer? (job title / type of person)
3. The painful problem before your product. 
4. The ONE action a visitor should take: join waitlist / book a demo / buy / download / contact.
5. Proof: customers, numbers, testimonials, logos, founder story (real only — never invent).
6. Pricing to show (or "hide pricing").
7. 2–3 competitor or "sites I like" URLs.
8. Assets they have: logo, screenshots, product photos, brand colors.

If they gave a long description up front, extract answers from it and only ask what's missing.

Write `PRODUCT.md` (impeccable reads this file): audience, problem, promise, primary CTA, proof, tone (3 adjectives), sections plan. Show a 5-line summary and get a "yes".

## Phase 3 — Pick the look (≈5 min)

Offer two routes:

**A. Pick a real style (recommended).** Tell them to open https://styles.refero.design, browse, click a style they love, and download its **DESIGN.md** (or paste the style's link here). Save it as `DESIGN.md` in the project. Prompt template from Refero to adapt internally:
> Design [screen] for [audience] so they can [job]. Use DESIGN.md for type and color roles. Use our real copy and product behavior; the reference does not define them.

Browse ideas by mood: https://styles.refero.design/ai-agents/design-prompts

**B. Let me design it.** Use the `hallmark` skill (and `impeccable`'s shape step) to propose **3 distinct directions** — each as: name, 3 colors (hex), font pairing, one-sentence vibe, layout idea. They pick one. Write it to `DESIGN.md`.

Rules either way: borrow the *visual system*, never another brand's logo, name, or copy. Max 2 font families.

## Phase 4 — Create the project (≈5 min)

Ask for a short project name (lowercase, dashes, e.g. `acme-site`). Then:

```bash
npx -y create-next-app@latest acme-site --ts --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm --yes
cd acme-site
npx -y shadcn@latest init -d
npx -y shadcn@latest add button card badge accordion input sheet separator
npm i lucide-react motion @vercel/analytics
```

- **shadcn/ui** = polished building blocks. **lucide-react** = icon library. **motion** = animations. **@vercel/analytics** = free visitor stats.
- Move `PRODUCT.md`, `DESIGN.md`, `GTM_PROGRESS.md` into the project folder.
- Create `CLAUDE.md` in the project:
  ```md
  # Project design context
  Before changing UI, read @PRODUCT.md and @DESIGN.md.
  Use DESIGN.md for colors, typography, spacing. Copy and facts come from PRODUCT.md only.
  ```
- Load fonts with `next/font/google` (no external font links). Map DESIGN.md colors into the CSS variables shadcn created in `src/app/globals.css`.

## Phase 5 — Visual assets (optional, ≈10 min)

If they have product screenshots/photos, have them drag the files into the chat or put them in `public/`.

If Higgsfield is connected and they **approve the credit cost**: generate hero image/video, product shots, or section illustrations matching DESIGN.md palette. Save into `public/` as `.webp`/`.mp4`. Keep hero video under ~5 MB, always with a poster image.

No Higgsfield? Use clean typography, CSS gradients/shapes, and lucide icons — a great GTM page doesn't need stock photos.

## Phase 6 — Build the page

Use the `hallmark` skill to build (it enforces non-template structure) and `impeccable` for craft rules. A strong GTM page usually has — adapt order and drop what doesn't fit:

1. **Nav** — logo, 2–4 links, primary CTA button.
2. **Hero** — headline = outcome for the buyer (≤10 words), subline = how, primary CTA + secondary link, product visual.
3. **Problem** — the pain, in the buyer's words.
4. **How it works** — 3 steps or a product walkthrough.
5. **Features → benefits** — each feature tied to a result. Not a uniform 3-card grid.
6. **Proof** — testimonials, logos, metrics (real only; leave placeholders clearly marked `TODO` otherwise).
7. **Pricing / offer** — if shown.
8. **FAQ** — shadcn Accordion; answer real objections.
9. **Final CTA** — repeat the one action.
10. **Footer** — contact email, social links, `/privacy` and `/terms` pages (simple, generated, marked "review with a lawyer").

**The CTA must actually work.** Simplest options (ask which):
- Book a demo → link to their Calendly / Cal.com.
- Waitlist / contact → a free form service (Tally.so or Formspree) — they create the form, paste the link/form ID; or a `mailto:` link as a stopgap.

**SEO & sharing (do all):** `metadata` in `layout.tsx` (title, description, openGraph), an OG image via `src/app/opengraph-image.tsx`, favicon from their logo, `src/app/sitemap.ts`, `src/app/robots.ts`, `<Analytics />` from `@vercel/analytics/next` in the layout. Semantic HTML, alt text on images.

Animations via `motion` + the `motion-design` skill: subtle reveals on scroll, respect `prefers-reduced-motion`.

## Phase 7 — Preview & polish

```bash
npm run dev
```
Tell them to open http://localhost:3000 and react in plain words ("the headline feels boring", "make it more premium"). Iterate.

Then run quality passes:
- `/impeccable audit` then `/impeccable polish` (or invoke the skill with those commands).
- Check mobile: tell them to shrink the browser window or open DevTools device mode; fix any overflow.
- `npm run build` must pass with zero errors before deploying.

## Phase 8 — Save to GitHub (≈5 min)

Explain: "GitHub is the online home for your site's code; Vercel will watch it and auto-update your site."

1. Login (user runs): `! gh auth login` → choose **GitHub.com → HTTPS → Yes → Login with a web browser**, copy the code, approve in browser.
2. You run:
```bash
git init -b main 2>/dev/null; git add -A
git commit -m "feat: initial GTM website"
gh repo create acme-site --private --source=. --push
```
Confirm `.gitignore` excludes `node_modules`, `.env*`, `.next`.

## Phase 9 — Go live on Vercel (≈5 min)

**Recommended (no terminal):** tell them to open https://vercel.com/new → **Import** the `acme-site` repo → leave settings as-is → **Deploy**. In ~1 minute they get a live link like `acme-site.vercel.app`. From now on, **every time you push to GitHub, the live site updates automatically.**

**CLI alternative:** `! npx vercel login`, then you run `npx vercel link --yes` and `npx vercel --prod`.

Open the live URL together; check the CTA, the mobile view, and that sharing the link (e.g. in WhatsApp/Slack) shows the OG image.

## Phase 10 — Buy & connect a domain (≈15 min + wait)

**Pick a name:** brainstorm 10 options from the product name — short, easy to spell aloud, `.com` first; alternatives `.ai`/`.io`/`.co`/`.app` or prefixes like `get`/`try`/`use` + name. Check availability with `whois <domain>` (no "Domain Name:" record ≈ likely free) and confirm on the registrar.

**Where to buy (they pay, you guide):**
| Option | Why |
|---|---|
| **Vercel Domains** (Project → Settings → Domains → Buy) | Easiest. Zero DNS setup — auto-connected. |
| **Cloudflare Registrar** | At-cost pricing, no markup on renewals. |
| **Namecheap / Porkbun** | Cheap, beginner-friendly. |

Advise: turn on auto-renew and WHOIS privacy; watch for cheap first-year / expensive renewal; skip upsells (hosting, email, SSL — Vercel includes SSL free).

**Connect a domain bought elsewhere:**
1. Vercel → Project → **Settings → Domains** → add `example.com` (Vercel will offer to add `www.example.com` too — accept, redirect one to the other).
2. Vercel shows the exact DNS records to add. Typically:
   - `A` record, host `@` → `76.76.21.21`
   - `CNAME` record, host `www` → `cname.vercel-dns.com`
   **Always use the values Vercel displays** if they differ.
3. At the registrar → DNS settings → delete conflicting existing `A`/`CNAME` records for `@`/`www` (parking pages) → add the records above. On Cloudflare set the proxy to **DNS only** (grey cloud).
4. Wait — usually minutes, up to 48h. Vercel shows "Valid Configuration" and issues HTTPS automatically.
5. Update `metadataBase` / site URL in the code to the real domain, commit, push.

**Business email** (optional): Google Workspace or Zoho Mail at the registrar — add their MX records; it doesn't affect the website records.

## Phase 11 — Handoff

Write a `HOW_TO_UPDATE.md` in the project for them:
- "Open this folder in Claude Code and describe the change. Then say *push it live*." (you run `git add -A && git commit -m "..." && git push`; Vercel redeploys.)
- Live URL, GitHub repo URL, Vercel dashboard URL, domain registrar + renewal date.

Final message: a celebratory ✅ summary of everything live, and 3 suggested next steps (e.g. add real testimonials, submit sitemap to Google Search Console, launch post on LinkedIn/Product Hunt).
