# Canadian tax parameters, 2026 (open data)

The federal, provincial and territorial income-tax parameters for tax year 2026, as one JSON file.
It is the table [Countworthy](https://countworthy.com)'s Canadian calculators compute with, exported
from the same engine the site runs, so the file and the calculators cannot disagree.

- **File:** [`ca-tax-2026.json`](ca-tax-2026.json)
- **Also served at:** https://countworthy.com/data/ca-tax-2026.json
- **Readable tables of the same figures:** https://countworthy.com/canada/tax-rates-2026
- **Licence:** [CC BY 4.0](LICENSE)

## What is in it

Everything sits under `parameters`. Rates are decimals (`0.205` is 20.5%), amounts are Canadian
dollars, and a bracket's `upTo` of `null` means no upper limit.

| Block | Contents |
|---|---|
| `federal` | Federal brackets, the basic personal amount and the income range over which it phases down, the Canada Employment Amount, the lowest rate (used for credits) and the Quebec abatement |
| `provinces` | All thirteen provinces and territories, keyed by two-letter code: brackets, basic personal amount and lowest rate. Manitoba carries its basic personal amount's phase-out range, Yukon its mirror of the federal amount and its own employment amount, Quebec its deduction for workers |
| `cpp`, `qpp` | Canada and Quebec Pension Plan: basic exemption, first and second earnings ceilings, base and enhanced rates, and the second-ceiling (CPP2/QPP2) rate |
| `ei`, `eiQC`, `qpip` | Employment Insurance (outside and inside Quebec) and the Quebec Parental Insurance Plan: maximum insurable earnings and employee rates |
| `rrsp` | The RRSP dollar limit and the percentage of earned income |
| `ohpBands`, `onSurtax`, `onReduction` | Ontario Health Premium bands, the two Ontario surtax tiers and the basic Ontario tax reduction |
| `bcReduction` | British Columbia's tax reduction: maximum, threshold and phase-out rate |
| `dividends` | Gross-up and federal and provincial dividend tax credit rates for eligible and non-eligible dividends (Ontario, BC, Alberta, Quebec) |
| `corporate` | Federal and provincial small-business and general corporate rates and the business limit (Ontario, BC, Alberta, Quebec) |

Where a rate changed part-way through 2026, the file holds the full-year figure the calculators
use. Ontario's small-business rate, for example, is the day-weighted blend of the rate before and
after its 1 July 2026 change, for a corporation with a calendar year.

The top level also carries `taxYear`, `verified` (the date the figures were last checked against
their sources), `sources` (the primary government pages they come from) and `licence`.

## How the figures are checked

Each figure traces to a primary source: the Canada Revenue Agency's payroll deductions formulas
(T4127) and rate pages, Revenu Québec's source-deduction formulas (TP-1015.F-V), and provincial
budget documents.
Countworthy's calculators carry the same citations, with the date each was last verified, and a
test suite re-derives the results independently. The method is described at
https://countworthy.com/methodology, and every source is listed at https://countworthy.com/sources.

A test in the site's own repository fails if this file ever differs from a fresh export of the
engine.

## Use it

```bash
curl -s https://countworthy.com/data/ca-tax-2026.json
```

```js
const res = await fetch('https://countworthy.com/data/ca-tax-2026.json');
const { taxYear, verified, parameters } = await res.json();
const ontario = parameters.provinces.ON;        // brackets, basic personal amount, lowest rate
const federalBrackets = parameters.federal.brackets;
```

To see the parameters applied, the calculators that use them are free and need no account:

- [Take-home pay calculator](https://countworthy.com/canada/take-home-pay-calculator), by province
- [Tax brackets and rates by province and territory](https://countworthy.com/canada/tax-rates-2026)
- [Take-home pay tables](https://countworthy.com/canada/ontario-take-home-pay-2026), one per province and territory
- [All Canadian calculators](https://countworthy.com/canada)

## Licence and attribution

Creative Commons Attribution 4.0 International. Use it for anything, including commercially, with
attribution:

> Tax data from Countworthy (https://countworthy.com), CC BY 4.0.

## Corrections

If a number here is wrong, it is wrong on the website too. Email hello@countworthy.com or open an
issue; both are fixed and republished together.

This is reference data, not tax advice. Payroll software applies these rules per pay period and can
differ from an annual calculation by a few dollars.
