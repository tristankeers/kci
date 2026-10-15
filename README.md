# KCI: buyout underwriting

**Live site: [tristankeers.github.io/kci](https://tristankeers.github.io/kci)**

KCI is a private equity underwriting tool. For a listed company it builds a leveraged buyout: what a fund would pay, how much it could borrow, how the company's cash repays that debt over five years, and what the fund earns when it sells. It also values the company with a DCF and peer multiples, stress-tests the case, and writes an investment committee paper that exports to PDF and Excel.

It runs across 100 US companies from the S&P 500, Dow Jones and Nasdaq, in software, cybersecurity, semiconductors, defence, energy, healthcare technology and quantum computing.

A personal project by Tristan Keers. Designed and tested by Tristan, built with AI-assisted coding. **Not investment advice.**

## Worked case: Dropbox

Built by hand from Dropbox's own filings (10-K, 10-Q and results releases), with every input sourced or explained.

| | |
|---|---|
| Share price (15 Oct 2026 close) | $36.55 |
| Offer at a 30% premium | $47.50 a share, 15.4x reported EBITDA |
| Debt | 5.5x EBITDA (term loan 4.5x, second lien 1.0x) |
| Fund's return (gross IRR) | **0.5% a year**, against a 20% target |
| Highest price that still earns 20% | $24.26 a share |
| Recommendation | **Pass** (do not invest) |

The reason is the price. The fund pays 15.4x EBITDA but can only assume an exit at today's market multiple of 12.6x, and that fall in the multiple costs more than five years of EBITDA growth adds. With no debt at all the deal would earn 2.8% a year, below the roughly 7% after-tax cost of the new debt, so each extra turn of leverage lowers the return rather than raising it. Two items the headline numbers hide are treated as real costs: $304m a year of stock-based pay, and about $157m a year of data-centre finance leases that behave like capital spending.

## How it works

- **Sources and uses**: cash-free, debt-free purchase; existing debt refinanced; transaction and financing fees.
- **Five-year model**: revenue, EBITDA, tax, capital spending and working capital give the cash available to repay debt; interest on opening balances (no circularity).
- **Debt waterfall**: scheduled amortisation, a revolver that protects minimum cash, then a cash sweep. Shortfalls are funded by the sponsor and count against returns.
- **Exit**: year-five EBITDA × exit multiple, less fees and net debt. IRR and money multiple are calculated on all equity put in.
- **Valuation**: comparable companies (with outlier flags), a DCF with perpetuity and exit-multiple terminal values, and sensitivity tables.
- **Scenarios**: downside, base and upside cases, an entry-versus-exit multiple grid and break-even analysis.

Companies without a hand-built model use deliberately conservative defaults: debt of 4.0x EBITDA capped at 60% of the price, margins held flat, growth fading to 3% by year five, the company's own D&A and capex, and an exit at the entry multiple. Each choice is explained on the site's Methodology page.

## Data and checks

Figures are trailing twelve months from StockAnalysis.com (S&P Global data), with closing share prices from 13–15 Oct 2026. Every figure records its source. Each company carries an evidence label:

- **Verified (5)**: key figures rebuilt from the company's own filings. This caught a real error: the data provider left $3.6bn of minority interests out of EQT's enterprise value.
- **Cross-checked (65)**: revenue, market value, enterprise value and EBITDA agree with a second provider (GuruFocus) within set tolerances.
- **Sourced (30)**: sourced, but at least one figure disagrees with the second provider; the disagreement is shown.

## Testing

- **53 automated engine tests**: hand-worked cases (for example, a business bought at 10x with no growth must return exactly 10% a year) and edge cases such as zero debt, negative cash flow and wiped-out equity.
- **Independent check**: 700 randomised buyouts and DCFs recalculated by a separate Python model, which agrees with the engine to at least ten decimal places.
- **Model checks on every run**: sources equal uses, every debt and cash balance rolls forward, cash never goes negative, and exit proceeds tie out.
- **Browser tests**: assumptions are changed and every page and export is confirmed to update.

## Limitations

- Prices are a snapshot, not live. The tool is built to connect to a market-data API, but that connection is not switched on in this version.
- Default assumptions are generic; only the worked case reflects company-specific judgement.
- Annual model; returns are gross of fund fees and carried interest.

## Files

- `index.html`: the whole application in one file (React 18, no build step needed to view it).
- `og.png`: the preview image shown when the link is shared.
