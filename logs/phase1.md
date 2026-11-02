# Phase 1 log — Problem and data

**Release:** `v1-phase1`

## What we did
- Chose the problem: households can't explain their monthly electricity bill.
- Collected 10 real bills (9 JEPCO Amman, 1 IDECO Irbid; 4 households; Feb 2024 – Sep 2025) and typed every line into `data/bills.csv`.
- Collected the official EMRC tariff documents (`data/sources/`).
- Wrote 30 test questions with expected answers before trying any model (`eval/seed_test_set.csv`): 13 easy, 9 medium, 5 hard, 3 refusal.
- Built a tariff calculator and an early bill reader with Qwen2.5-VL-7B (4-bit, Colab T4).

## What the numbers showed
| Check | Result |
|---|---|
| Calculator vs EMRC published examples | 8 / 8 exact |
| Calculator vs real bills, every line | 10 / 10 exact |
| Reader, first bill (B01) | 6 / 10 fields, then 9 / 10 after reading dinar and fils separately |
| Reader, second bill (B08) | Rows shifted in the money table; consistency check repaired the values |

## What we learned
- Bills carry four fees on top of the tariff: meter rent 0.200, rural fils 1 fils/kWh, TV fee 1.000, waste fee (city-specific).
- JEPCO bills a longer reading period as 30 days (the محتسبة reading).
- The vision model reads the top of the bill well and struggles with the dinar/fils money table.

## Next (Phase 2)
- Grow the test set to 50+ questions; collect more bills (EDCO, < 50 kWh, > 1000 kWh, unsubsidized).
- Build the baseline (model alone, no calculator) and the evaluation script.
