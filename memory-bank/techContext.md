# Technical Context (Strict Stack & Constraints)

## Mandatory Technology Stack
This project **must** follow the stack below according to the PRD:

1. **Environment**: Jupyter Notebook (`.ipynb`)
2. **Language**: Python 3.8+
3. **Core Scraping Library**: `scrapling` (D4Vinci/Scrapling)
4. **Browser Automation Layer**: `playwright` (an important dependency for browser control)
5. **Data Export Module**: Python built-in `csv`

## Hard Requirements (Non-Negotiable)
- The browser must run in visual mode using `StealthyFetcher` with `headless=False`.
- The authentication flow is semi-manual: the script must pause using `input()` so the user can log in themselves.
- After the user proceeds, a DOM render delay (`time.sleep(3)` to `5`) is recommended before parsing.
- The extraction strategy must be resilient to dynamic class changes (use structure/text-based XPath/CSS).
- Error handling must use `try-except` so the script does not crash immediately when the target structure partially changes.

## Data Export Constraints
- Only the built-in `csv` module may be used.
- CSV file writing must include:
  - `encoding='utf-8'`
  - `newline=''`
- Goal: special character compatibility (UTF-8) and prevention of extra blank lines (especially on Windows).

## Explicit Prohibitions
- **Using Pandas is prohibited** in this project iteration.

## Runtime/Execution Notes
- Because the target tends to be high-security (e.g., Semrush), the flow must accommodate manual login and potential captcha.
- The browser/session state must be kept active across notebook cells until extraction & export are complete.