# Fatoorti (فاتورتي) - Jordanian electricity bill explainer

Team project for **Building LLM Systems: From Models to Production**.

**User:** Jordanian households who receive a monthly JEPCO or IDECO electricity bill and can't tell why the amount changed.
**What it does:** the user sends a photo of their bill and asks a question in Arabic. Fatoorti reads the bill, recalculates it with the official EMRC tariff, and explains the amount in plain Jordanian Arabic.

```
Bill photo ──► Qwen2.5-VL reads every line as JSON
           ──► consistency check (lines must add up to the printed total)
           ──► tariff calculator tool (EMRC tiers, fixed discount, fees)
           ──► Qwen explains in Arabic using only the calculator's numbers
```

## Repository structure

| Path | Contents |
|---|---|
| `data/bills.csv` | Answer key: every line of each bill, typed by hand (no personal data) |
| `data/bills/` | Redacted bill photos `B01.jpg` … `B10.jpg` |
| `data/sources/` | Official tariff documents used by the system |
| `data/raw/` | Original unredacted photos - **local only, never committed** (git-ignored) |
| `eval/seed_test_set.csv` | 30 test questions with expected answers, written before trying any model |
| `notebooks/bill_explainer.ipynb` | Calculator, bill reader (Qwen2.5-VL), explanation, evaluation |
| `docs/` | Phase submissions (proposal, data card) |
| `logs/` | One-page log per phase: what changed and what the numbers showed |

## How to run

1. Open `notebooks/bill_explainer.ipynb` in Google Colab (File → Open notebook → GitHub tab → paste this repo's URL).
2. Runtime → Change runtime type → **T4 GPU**.
3. In the "Get the project files" cell, set `GITHUB_USER`, then Runtime → Run all.

Section 2 (the calculator) needs no GPU and checks itself against EMRC's published examples and all bills in `data/bills.csv`.

## Data and privacy

- 10 household bills (9 JEPCO Amman, 1 IDECO Irbid; 4 households; Feb 2024 – Sep 2025), shared by team members' families with consent.
- Names, subscription / file / meter / invoice / reference numbers, barcodes and QR codes are blacked out, and photo metadata is stripped, before any image is committed.
- `data/bills.csv` labels households H1–H4 instead of names.

## Results so far

| Check | Result |
|---|---|
| Calculator vs EMRC published examples | 8 / 8 exact |
| Calculator vs real bills (every line) | 10 / 10 exact |
| Vision reader, early test (2 bills) | Top of bill read correctly; money table misread on 1 bill (rows shifted), repaired by the consistency check |

## Phases

| Phase | Weeks | Release tag | Status |
|---|---|---|---|
| 1. Problem and data | 2–3 | `v1-phase1` | ✅ |
| 2. Baseline and evaluation | 4–6 | `v2-phase2` | |
| 3. Core improvement (calculator tool + RAG) | 7–9 | `v3-phase3` | |
| 4. Hardening | 10–12 | `v4-phase4` | |
| 5. Release | 13 | `v5-release` | |

## Team

| Member | Phase 1 role |
|---|---|
| _Name_ | Product lead |
| _Name_ | Data lead |
| _Name_ | Evaluation lead |
| _Name_ | Engineering lead |
