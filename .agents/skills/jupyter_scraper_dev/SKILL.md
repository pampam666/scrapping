---
name: jupyter-scraper-dev
description: "Use this skill whenever the user mentions Jupyter scraping, Semrush, authenticated dashboards, CAPTCHA/manual login, StealthyFetcher, XPath for dynamic React pages, or CSV export without Pandas. Apply it to force PRD-safe notebook design: visual browser, manual auth pause, resilient extraction, and strict csv-only export."
---

# Jupyter Scraper Dev Skill (Authenticated Web Scraping to CSV)

## Mission
Design documentation and implementation guidance for a **semi-automated Jupyter scraping workflow** that survives high-security sites and stays compliant with strict project constraints.

Do not drift into convenience shortcuts. Enforce the PRD contract.

## Trigger Conditions
Activate this skill immediately when requests include one or more of:
- web scraping in Jupyter Notebook
- Semrush or authenticated dashboards
- login/CAPTCHA handling
- Scrapling + Playwright setup
- XPath/CSS selector robustness for dynamic classes
- exporting scraped rows to CSV without Pandas

## Non-Negotiable Constraints (PRD Contract)
1. Use **Python 3.8+** in **Jupyter Notebook**.
2. Use **scrapling** (with Playwright layer).
3. Launch browser with **`StealthyFetcher` + `headless=False`**.
4. Pause execution with **`input()`** for manual login.
5. Use **structure/text-based XPath/CSS**; avoid dynamic-class dependency.
6. Export with built-in **`csv` only**, including:
   - `encoding='utf-8'`
   - `newline=''`
7. **Ban Pandas explicitly.**

## Why These Rules Exist (Theory of Mind)
- Use `headless=False` because humans must see and interact with the browser to solve login challenges and CAPTCHA safely.
- Pause with `input()` because forcing automated login in hostile anti-bot environments increases failure risk and account friction.
- Prefer structure/text selectors because React/styled components often rotate class names, making class-based selectors brittle.
- Add post-login wait (`time.sleep(3)` to `5`) because SPA dashboards frequently render data asynchronously after visible navigation completes.
- Use built-in `csv` instead of Pandas to keep dependencies lightweight, reduce setup friction in notebooks, and satisfy the defined deliverable.

## Operating Procedure (Imperative)
1. **Define** the notebook as staged cells: Imports → Launch → Pause → Extract → Export.
2. **Initialize** browser automation through Scrapling `StealthyFetcher` in visual mode.
3. **Instruct** the user to login manually; **block** progression with `input()`.
4. **Wait** 3–5 seconds after ENTER before parsing page state.
5. **Extract** data using robust structure/text selectors with fallback paths.
6. **Wrap** fragile extraction points in `try-except` to preserve partial output.
7. **Write** output using built-in `csv` with UTF-8 + no extra newline config.
8. **Reject** requests to use Pandas unless constraints are explicitly changed.

## Required Output Contract for Agents Using This Skill
When responding to users, produce:
1. A short plan aligned with the 5 notebook phases.
2. Selector strategy rationale (why not dynamic classes).
3. Error-handling strategy (`try-except` at key extraction points).
4. CSV export compliance callout (`csv`, UTF-8, newline='').
5. Explicit statement: Pandas is not used.

## Anti-Patterns to Block
- Do not propose headless-only scraping for authenticated flows.
- Do not skip the manual auth pause gate.
- Do not lead with dynamic/random class selectors.
- Do not output Pandas/DataFrame export paths.
- Do not omit encoding/newline CSV settings.

## Input / Output Examples

### Example 1
**Input Prompt**
"Buat notebook scraping Semrush. Saya akan login manual. Setelah itu tarik tabel keyword dan simpan ke CSV."

**Expected Output Shape**
- Provides 5-cell structure.
- States `StealthyFetcher` with visual browser (`headless=False`).
- Includes explicit `input()` manual gate.
- Mentions 3–5 second post-login wait.
- Uses structure/text-based XPath guidance.
- Exports with built-in `csv` using UTF-8 and `newline=''`.
- Explicitly says Pandas is not used.

### Example 2
**Input Prompt**
"Class di halaman ini selalu berubah-ubah. Selector apa yang harus dipakai?"

**Expected Output Shape**
- Recommends structure/text-based XPath/CSS first.
- Explains why dynamic classes are brittle.
- Suggests fallback selectors and `try-except` guarding.

### Example 3
**Input Prompt**
"Tolong cepat pakai Pandas aja buat ekspor." 

**Expected Output Shape**
- Refuses Pandas for this workflow.
- Explains lightweight dependency and PRD compliance reason.
- Redirects to built-in `csv` export requirements.

## Progressive Disclosure Notes
- Keep this `SKILL.md` focused and concise (<500 lines).
- Place executable scripts or code templates in `scripts/` or `templates/` if later needed.
- Keep eval prompts in `evals/evals.json` for behavior checks.
