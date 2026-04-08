# Workflow: Manual Authentication Flow (Semi-Automated)

## Scope
Mandatory workflow for scraping targets with high authentication/anti-bot protection, aligned with the Jupyter + Scrapling + Playwright project PRD.

## Exact Execution Workflow
1. **Launch visual browser session**
   - Initialize the session using `StealthyFetcher`.
   - Ensure the browser runs with `headless=False`.

2. **Navigate to target entry URL**
   - Open the target login/entry point page (e.g., Semrush).

3. **Pause script for manual auth**
   - Print clear instructions in the notebook:
     - The user must log in manually in the opened browser.
     - Complete captcha/2FA if it appears.
     - Wait until the target page (dashboard/data table) is fully visible.
   - Pause execution with `input()`.

4. **User confirmation gate**
   - Explicit instruction: **"Press ENTER in the notebook after login succeeds and the data page is loaded."**

5. **Post-auth stabilization**
   - After ENTER, wait 3–5 seconds to ensure the final DOM has rendered.

6. **Proceed to extraction phase**
   - Continue parsing using robust structure/text-based selectors.
   - Apply `try-except` to keep the workflow resilient.

## Compliance Notes
- This flow **must not** be replaced with fully-automated login.
- The browser session must be maintained while the notebook runs across cells until extraction and export are complete.

## Strict Notebook Coding Rules

### RULE A (No Context Managers for Browser)
- NEVER use the `with` statement to initialize the browser in notebook cells (e.g., `with StealthyFetcher() as fetcher:`).
- The browser/fetcher MUST be instantiated as a global/top-level variable in an early setup cell (e.g., `fetcher = StealthyFetcher(headless=False)`) so the session persists into the next cells for manual authentication and extraction.
- The browser session MUST ONLY be closed manually in the final cleanup cell using `fetcher.close()`.

### RULE B (Jupyter Async Handling)
- Jupyter natively runs an asyncio event loop; NEVER use `asyncio.run()` inside the notebook.
- If async Scrapling methods are used, execute them with top-level `await` directly inside notebook cells.