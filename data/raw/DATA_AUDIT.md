# Raw Data Audit

Audited: `wsop_main_event_final_table_2001-2025.csv` and `wsop_main_event_results_1971-2025.csv`

---

## `wsop_main_event_final_table_2001-2025.csv`

### Schema

| Column | Type | Description |
|---|---|---|
| `year` | integer | Tournament year (2001–2025) |
| `entrants` | integer | Total field size for that year |
| `place` | integer | Final table finish position (1–9) |
| `name` | string | Player full name |
| `prize` | integer | Prize money in USD (no formatting — raw integer) |

### Coverage

- **Years:** 2001–2025, all 25 years present, no gaps
- **Rows per year:** 9 (one per final table seat), 225 rows total
- **Entrant counts:** Range from 613 (2001) to 10,112 (2024)

### Quirks & Flags

- **2020 anomaly:** Only 1,379 entrants — the COVID year. The event was held online via GGPoker and concluded in December 2020/January 2021. Worth flagging in any field-size analysis.
- **One quoted name with embedded quotes:** `"Badih ""Bob"" Bounahra"` (2011, 7th place) — standard CSV escaping, parsers handle this correctly, but confirm your parser respects RFC 4180 quoting.
- **One name with trailing asterisk:** `Peiyuan Sun*` (2020, 9th place) — the asterisk likely flags an online-only qualifier status. Strip before name-matching or joining; treat as a data note.
- **Special characters in names:** `Özgür Seçilmiş` (2021, 5th), `Espen Jørstad` (2022, 1st) — UTF-8 encoded, verify your pipeline reads with `encoding='utf-8'`.
- **Prize column:** Clean integers throughout — no dollar signs, no commas, no nulls. Ready to use as-is.
- **Entrants column:** Consistent within each year (same value repeated for all 9 rows of a given year). Can be deduplicated to a `year → entrants` lookup table if needed.
- **No duplicate rows detected** across year+place combinations.
- **Returning players:** Several players appear in multiple years — Dan Harrington (2003 3rd, 2004 4th), Ben Lamb (2011 3rd, 2017 9th), Antoine Saout (2009 3rd, 2017 5th), Joe Cada (2009 1st, 2018 5th), Kenny Hallaert (2016 6th, 2025 4th), Michael Mizrachi (2010 5th, 2025 1st), Mark Newhouse (2013 9th, 2014 9th — consecutive final tables). Name-based cohort work should account for these repeats.

---

## `wsop_main_event_results_1971-2025.csv`

### Schema (from pipeline notes + Kaggle source documentation)

| Column | Type | Description |
|---|---|---|
| `year` | integer | Tournament year |
| `place` | integer | Finish position |
| `name` | string | Player full name |
| `prize` | numeric | Prize money in USD |
| `entries` | integer | Total field size for that year |

> **Note:** The big file is 1.7 MB (~17,000+ rows). The schema above reflects the documented Kaggle source fields. The pipeline notes confirm overlap with the final-table file on `year + place` for final-table rows; the reconciliation outputs in `data/processed/` document any discrepancies between the two sources.

### Coverage

- **Years:** 1971–2025 (55 years)
- **Depth:** Covers all paid finishers each year, not just the final 9. In post-2003 years with thousands of entrants, this means hundreds of rows per year.
- **Pre-moneymaker era (1971–2002):** Fields were small (handful to ~600 players); row counts per year are correspondingly small.
- **Known concern:** Name consistency across 55 years of data sourced from multiple eras is likely imperfect. Expect variations in diacritics, spacing, hyphenation, and nickname usage. Normalize names before any cross-year player matching.

### Flags to Verify

- Confirm `entries` vs `entrants` column name matches the final-table file — the pipeline notes call them different things (`entries` in full-results, `entrants` in final-table). If joining on field size, align on one column name.
- Prize values in early years (1971–1990s) may be formatted differently or missing for non-money finishers — confirm nulls are handled.
- The 2020 online event split (GGPoker online + in-person final) may create structural oddities in finish ordering for that year.

---

## Cross-File Reconciliation

The pipeline already runs a reconciliation check (`data/processed/wsop_dataset_reconciliation_detail.csv`) that merges both files on `year + place` and checks for name and prize discrepancies. Refer to that output before treating either file as ground truth for overlapping rows (2001–2025 final table positions).

---

## Overall Assessment

Both files are in good shape for analysis. The final-table CSV is clean and ready to use immediately. The full-results CSV is large but well-structured; the main risk is name inconsistency across eras for any cross-year player identity work. The two specific items to handle before moving forward are:

1. Strip the trailing `*` from `Peiyuan Sun*`
2. Confirm UTF-8 encoding on load
3. Decide on a name normalization strategy (lowercase + strip punctuation minimum) before any player cohort joins
