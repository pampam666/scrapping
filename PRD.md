# Product Requirements Document (PRD)
**Project Name:** Authenticated Web Scraper to CSV via Jupyter Notebook
**Target Platform:** Jupyter Notebook (.ipynb)
**Primary Target Website:** Semrush (or similar high-security websites)

---

## 1. Project Overview
This project aims to develop an interactive Jupyter Notebook-based web scraping script capable of extracting data from web pages protected by strict authentication and *anti-bot* systems. The final output of data extraction must be saved in `.csv` file format.

Due to the strict security of the target, a *fully-automated login* approach is not used. Instead, the system will use a *semi-automated* approach where the script opens the browser, pauses execution to allow the *user* to log in manually and complete Captcha, then proceeds with the DOM extraction process.

## 2. Technology Stack
The AI agent (Cline.bot) is required to use the following technology stack:
* **Environment:** Jupyter Notebook (`.ipynb`)
* **Language:** Python 3.8+
* **Core Scraping Library:** `scrapling` (GitHub: D4Vinci/Scrapling)
* **Browser Automation:** `playwright` (as a dependency of Scrapling)
* **Data Export:** `csv` (Python built-in module)

## 3. Functional Requirements

### 3.1. Notebook Structure (Cell Layout)
The Jupyter Notebook must be divided into logical *cells* so users can control the execution flow:
* **Cell 1: Environment Setup.** Install dependencies (`!pip install scrapling`, `!playwright install`).
* **Cell 2: Module Imports & Initialization.** Import libraries and define utility functions.
* **Cell 3: Browser Launch & Authentication Phase.** Open the browser in visual mode and pause execution while waiting for user input.
* **Cell 4: Data Extraction Phase.** Perform HTML *parsing* (using XPath/CSS) after the user confirms login status.
* **Cell 5: CSV Export & Cleanup.** Save data to disk and close the browser *instance*.

### 3.2. Browser & Stealth Configuration
* Must use the `StealthyFetcher` class from the `scrapling` library.
* Browser parameters **must** be set to visual mode: `headless=False`. This is crucial so users can see the web interface and perform login.

### 3.3. Manual Authentication Handling
* The system must navigate to the initial target URL.
* Use Python's `input()` function to temporarily pause cell execution. The script must print clear on-screen instructions asking the user to log in manually in the opened browser, and press `ENTER` in the notebook after the data page (dashboard) has fully loaded.

### 3.4. Data Extraction & Resilience
* The system must handle *dynamic class names* (e.g., CSS *modules* / *styled-components* that generate random classes such as `sc-abc kXyZ`).
* Prioritize using **XPath** that locates elements by structure or text (example: `//div[contains(text(), 'Target')]`), instead of relying on class names that may change at any time.
* The system must include `try-except` blocks to prevent script *crashes* (*graceful degradation*) if the web table structure partially changes.

### 3.5. Data Export (CSV)
* Extracted data must be saved using Python's built-in `csv` module.
* Ensure the file is configured with `encoding='utf-8'` and `newline=''` to avoid strange characters (for example in currency symbols or *intent keywords*) and prevent extra blank lines on Windows.
* Data Structure (Example Target Table Data):
    * Column 1: Keyword
    * Column 2: Position
    * Column 3: Volume
    * Column 4: KD (Keyword Difficulty)

## 4. Non-Functional Requirements
* **State Management:** Ensure the `StealthyFetcher` *instance* is declared globally or managed in such a way that the browser session does not close when moving between Jupyter Notebook *cells*.
* **Rate Limiting:** If the script is designed to visit many pages (pagination), it must include randomized delays (`time.sleep()`) between *requests* to avoid being blocked by *anti-bot* systems.

## 5. Acceptance Criteria (Definition of Done)
1.  Jupyter Notebook runs successfully without errors in the local environment.
2.  The execution cell successfully opens a Chromium browser (not headless).
3.  The user can complete manual login without script interference.
4.  After the user presses ENTER, the script successfully extracts HTML *nodes* from the *auth*-protected page.
5.  The `hasil_scraping.csv` file is created in the local directory with correct, clean *header* and row formatting (UTF-8 compatible).

## 6. Developer Notes for Cline.bot
* *Do not use Pandas for this iteration.* Keep dependencies lightweight by using the built-in `csv` module.
* Target web uses React.js. Ensure you add `time.sleep(3)` to `5` seconds after the user presses ENTER to allow the DOM to fully render the final state before extracting the page source with Scrapling.
* Assume the user is targeting a table component. Provide a robust, generic XPath selector example in Cell 4 that the user can easily swap out with their specific target.

***