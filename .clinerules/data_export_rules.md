# Data Export Rules (Strict)

## Primary Rule
Scraped data export **must only** use Python’s built-in module: `csv`.

## Mandatory CSV Writer Settings
Every CSV writing process must use the following parameters:
- `encoding='utf-8'`
- `newline=''`

## Rationale
- `utf-8` ensures non-ASCII characters (e.g., currency symbols/special characters) are stored correctly.
- `newline=''` prevents extra blank lines in CSV output, especially on Windows.

## Prohibited Approaches
- **Pandas is prohibited** at this stage.
- Changing the export pipeline to another format is prohibited when the deliverable requirement is CSV.

## Output Quality Requirements
- CSV files must have clear and consistent headers.
- Column order must remain stable according to the target data extraction.
- Empty/None data must be handled explicitly so row structure is not corrupted.

## Compliance Checklist
- [ ] Use Python built-in `csv`
- [ ] Set `encoding='utf-8'`
- [ ] Set `newline=''`
- [ ] Do not use Pandas