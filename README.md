# Assessed Invoice Builder

A single-file, browser-only web app that reads repair **estimates** (a main estimate plus any number of **supplementary** estimates) and a workshop **invoice** with the Google Gemini API, matches them line by line, and exports the assessed sheet as CSV.

## What it produces

Two CSVs that mirror the columns of `Assessed Invoice.xlsx`:

**Parts (Sheet 1)** — `Sl · Part · Est. Sl. No · Reference · Est. Amount · Ins Amount · % · GST · GST amt · Total · Difference · Liable · Status`

| Column | Derivation |
| --- | --- |
| GST amount | `Ins Amount × GST rate` |
| Total | `Ins Amount + GST amount` |
| Difference | `Est. Amount − Total` |
| Liable | `Est. Amount ÷ (1 + GST rate)` when the invoice overruns the estimate, otherwise `0` |

**Labour (Sheet 2)** — `Sl · Operation · Est no · Reference · Est Amount · Amount · Type · Material · Labour · Allowed`

Painting lines (type `P`) split into a material and a labour share (25 / 75 by default). Types are `R` (removal/refit), `D` (denting), `P` (painting).

Both files reproduce the source workbook's layout row for row — header rows, the *"Excluding GST on the Estimate."* note, and the column totals — so they paste straight into an existing template.

## Multiple estimates

Each estimate gets a tag. Parts matched to the **main** estimate carry that estimate's line number in *Est. Sl. No*; parts matched to a **supplementary** carry the tag as a prefix, so supplementary line 1 becomes `SUP-1`. Add as many supplementaries as the claim needs — tags are editable, so a second supplementary can be `SUP2`, `SUP-B`, or whatever the file already uses. Unmatched lines are `Nil`.

## Liability status

Gemini proposes a status per line and every cell stays editable afterwards:

- `OK` — matched to an estimate line and payable
- `Consumable` — shop consumables with no estimate line
- `Wear and Tear` — age-related rather than accident-related
- `No Estitmation` — a genuine repair part with no estimate line *(spelling kept to match the source workbook)*
- `No Liable` — outside the scope of the claim

## Usage

1. Open `index.html` in any modern browser, or host it anywhere static.
2. Get a free API key at **[aistudio.google.com/apikey](https://aistudio.google.com/apikey)** and paste it in.
3. Drop in the estimates and the invoice, set the claim number and GST rate, then **Read and assess**.
4. Correct anything in the table — every edit re-costs the sheet live — then copy or download the CSVs.

Everything runs client-side. Your key and documents go directly to Google's API and never touch another server.

## Model

Defaults to **`gemini-2.5-flash`**. Flash-Lite (faster) and Pro (most accurate on dense invoices) are also selectable.
