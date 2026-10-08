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
| `ei`, `eiQC`, `qpip` | Employment Insurance (outside and inside Quebec) and the Quebec Parental Insurance Plan: maximum insurable earnings and employee rates. `qpip.employerRate` is the employer's own QPIP premium on the same earnings |
| `rrsp` | The RRSP dollar limit and the percentage of earned income. `dollarLimit` is the 2026 limit, which caps a 2026 deduction; `dollarLimit2027` is the 2027 limit, which caps the room that 2026 earned income creates |
| `bonusFlat` | Flat income-tax withholding on a bonus when annual pay is low. `federal`: when the employee's total remuneration for the year, bonus included, is `limit` or less, the employer withholds `rate` of the bonus (`rateQC` in Quebec, where it is the federal share only). `qc`: Revenu Québec's rate on a bonus when estimated pay for the year, bonus included, does not exceed `limit`, which is Quebec's basic personal amount. Each `limit` tests the year's total pay, not the bonus |
| `payrollTax` | The payroll tax the Northwest Territories and Nunavut levy on employees, keyed by territory code: `rate` is applied to gross employment remuneration (not to taxable income, and not reduced by an RRSP contribution). It is not income tax and is separate from every other block; no other province or territory has an entry |
| `hsf` | Quebec's Health Services Fund, two separate charges: `employer`, the corporation's contribution on the wages it pays, and `individual`, a person's own contribution on income other than employment income (see below) |
| `ohpBands`, `onSurtax`, `onReduction` | Ontario Health Premium bands, the two Ontario surtax tiers and the basic Ontario tax reduction |
| `bcReduction` | British Columbia's tax reduction: maximum, threshold and phase-out rate |
| `dividends` | Gross-up and federal and provincial dividend tax credit rates for eligible and non-eligible dividends (Ontario, BC, Alberta, Quebec) |
| `corporate` | Federal and provincial small-business and general corporate rates and the business limit (Ontario, BC, Alberta, Quebec). `generalRateFactor` (0.72) is the share of income taxed at the general rate that can be paid out as eligible dividends. Quebec's entry carries `sbdTest: true`: its small-business rate is conditional (see below) |

Where a rate changed part-way through 2026, the file holds the full-year figure the calculators
use. Ontario's small-business rate, for example, is the day-weighted blend of the rate before and
after its 1 July 2026 change, for a corporation with a calendar year.

The top level also carries `taxYear`, `verified` (the date the figures were last checked against
their sources), `sources` (the primary government pages they come from) and `licence`.

`verified` is 8 October 2026: the federal, provincial and territorial tables were compared with their
sources again that day. Two blocks were added after the first check, on 3 September 2026.

The `bonusFlat` block was first read on 6 October 2026, from the Canada Revenue Agency's guide T4001
and Revenu Québec's Guide for Employers (TP-1015.G-V, section 9.5).

The `payrollTax` block was first read on 7 October 2026, from each territory's Payroll Tax Act
(section 3(1): 2% of the remuneration paid to the employee in the year). The cost of living tax
credits that residents of the two territories claim on their returns are not in this file.

Four parameters were added on 8 October 2026, each read from its source that day:

- `qpip.employerRate`: the employer's QPIP premium, 0.602% of salary up to `qpip.mie` (Revenu Québec,
  Guide for Employers TP-1015.G-V (2026-01), section 5.1). `qpip.rate` is still the employee's.
- `rrsp.dollarLimit2027`: $35,390 (Income Tax Act s. 146(1); the Canada Revenue Agency's limits table).
- `corporate.generalRateFactor`: 0.72, the general rate factor of Income Tax Act s. 89(1).
- `corporate.provinces.QC.sbdTest`: Quebec's `sbd` rate applies only to a corporation in the primary
  and manufacturing sectors or one whose employees were paid for at least 5,500 hours (Finances
  Québec, Information Bulletin 2026-3). A corporation that does not qualify pays Quebec's `general`
  rate within the business limit too. The file does not carry the partial deduction between 5,000 and
  5,500 hours.

The `hsf` block was also added on 8 October 2026 and read from its sources that day: the Act respecting
the Régie de l'assurance maladie du Québec (chapter R-5, ss. 33, 34 and 34.1.1 to 34.1.6.1), Revenu
Québec's Guide for Employers TP-1015.G-V (2026-01), section 6.1, and its rate page, Schedule F
(TP-1.D.F-V, 2025-12), and Finances Québec's Parameters of the Personal Income Tax System for 2026
with Information Bulletin 2025-8. Rates are fractions (0.0165 is 1.65%); `a` and `b` are in percentage
points, as the Act and the guide print them.

- `hsf.employer`: the contribution of an employer outside the primary and manufacturing sectors.
  `flatRate` where total payroll is `floor` or less; between `floor` and `threshold`, `a` + `b` × total
  payroll ÷ 1,000,000 per cent, kept to two decimals with the second raised by one when the third is
  more than 4; `maxRate` from `threshold`. The rate applies to all the wages. The file does not carry the
  primary and manufacturing schedule, or the contribution holiday announced for 2026 and 2027 for
  agriculture, forestry and fishing employers, which the Act as consolidated to 12 August 2026 does
  not contain.
- `hsf.individual`: paid on the Quebec return (line 446). Nothing up to `threshold1`; `rate` of the
  excess to a maximum of `max1` up to `threshold2`; above it, `max1` plus `rate` of the excess over
  `threshold2`, to a maximum of `max2`. Only the two thresholds are indexed, each January. The base is
  income other than employment income, with a dividend at its actual amount, not grossed up.

The same day, the calculators that use the `dividends` and `corporate` blocks were corrected: Ontario's
surtax is computed before the dividend tax credit (Taxation Act, 2007, s. 16(2)). No figure in those two
blocks changed.

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

This is reference data, not tax advice. Payroll software applies these rules to each pay period, so a
single pay stub can differ from an annual calculation spread evenly over the year: by a few dollars
below the CPP and EI ceilings, and by more above them, where contributions come off at the full rate
until the year's maximum is reached.
