# Scraping Rules (Resilience-First)

## Selector Strategy (Mandatory)
For websites with dynamic classes (React/CSS modules/styled-components), selectors must prioritize:
1. **DOM structure-based** XPath/CSS
2. **Stable text/label-based** XPath/CSS

### Strictly Discouraged
- Do not rely on random/dynamic class names as the primary selector.
- Avoid fragile selectors that depend on easily changing element order.

## Error Handling Requirement
Each important extraction stage must be wrapped in `try-except` so that:
- the script does not fully crash when some elements cannot be found,
- the process still produces usable partial output,
- failures can be reported clearly (graceful degradation).

## Manual Auth Compatibility
- Scraping may proceed only after the manual authentication gate (`input()`) is passed.
- After the user presses ENTER, a 3–5 second render wait is mandatory before parsing the page.

## Robustness Principles
- Use fallback selectors if the primary selector fails.
- Validate node existence before accessing attributes/text.
- Log extraction status per section to simplify debugging when the layout changes.

## Compliance Checklist
- [ ] Structure/text-based selectors are used
- [ ] Does not rely on dynamic classes as the primary strategy
- [ ] `try-except` blocks are applied at failure-prone points
- [ ] Flow is compatible with manual authentication + DOM render delay