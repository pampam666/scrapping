# Project Brief: Authenticated Web Scraper to CSV via Jupyter Notebook

## Project Goal
Build a workspace for an interactive **Jupyter Notebook**-based scraping project that can retrieve data from websites with strong authentication/anti-bot protection (example: Semrush), then export the results to a **CSV** file.

The required approach is **semi-automated**:
- The browser is opened in visual mode (not headless).
- The user performs manual login and handles captcha if needed.
- The notebook waits for user confirmation via `input()` before proceeding to data extraction.

## Architectural Direction (PRD-Aligned)
The notebook architecture must be modular per cell so the flow is easy to control:
1. **Environment Setup** – install dependencies (`scrapling`, `playwright`).
2. **Imports & Initialization** – initialize modules/helper functions.
3. **Browser Launch & Authentication** – run a visual browser with `StealthyFetcher` and pause for manual login.
4. **Extraction Phase** – parse the page after login, focusing on robust selectors.
5. **CSV Export & Cleanup** – save data via built-in `csv`, then close browser resources.

## Key Technical Principles
- Must use `StealthyFetcher` with `headless=False`.
- Element extraction must prioritize **structure/text-based XPath/CSS**, not random dynamic classes.
- `try-except` is mandatory for graceful degradation when DOM structure changes.
- Data export must only use the built-in `csv` module with:
  - `encoding='utf-8'`
  - `newline=''`

## Explicit Constraints
- **Pandas must not be used** in this iteration.
- After the user presses ENTER post-login, add a DOM render delay (`time.sleep(3)` to `5` seconds) before extraction.
- The browser session must be managed so it is not interrupted across notebook cells.

## Skill Hierarchy (Authority Rules)
- `.agents/skills/scrapling-official/` is strictly for API/syntax/examples reference ONLY.
- For architectural and workflow decisions (notebook cell split, manual authentication flow, browser lifecycle persistence, CSV export behavior), the agent MUST follow:
  - `.agents/skills/jupyter-scrapper-dev/`
  - `.clinerules/` (including scraping rules, data export rules, and workflow rules)
- If any guidance conflicts, `jupyter-scrapper-dev` + `.clinerules` take precedence over `scrapling-official`.

## Definition of Done (High-Level)
- Notebook runs locally without errors.
- Visual browser appears (non-headless).
- Manual login succeeds without script interference.
- After ENTER, data from the authenticated page is extracted successfully.
- The scraping CSV file is generated cleanly, UTF-8 encoded, without excessive blank lines.

## Final Workspace Structure (Approved)
- `/.agents/` — AI Skills
- `/.clinerules/` — Project Rules
- `/memory-bank/` — AI Context
- `/data/` — CSV Outputs (`hasil_scraping.csv` target location)
- `semrush_scraper.ipynb` — Main script notebook (starter scaffold)
- `requirements.txt` — Python dependencies (`jupyter`, `scrapling`, `playwright`)