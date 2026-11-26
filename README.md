# Quote of the Day App (GreenFuture Hack - Project #113)

Menu-driven Python terminal app that fetches a quote from an API, lets you save favourites locally,
and gives you search, validation, summaries, charts and exports.

## Quick start
```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt  # only Matplotlib is needed
python main.py
```
Requires Python 3.10+. Only the standard library is used apart from Matplotlib (charts).

## Features (mapped to the brief)
| Brief requirement | Where |
|---|---|
| Interactive terminal menu, ANSI colours | `main.py`, `ui.py` |
| Defensive input validation (loops until valid) | `validation.py` |
| Fetch quote from API + save favourites | `api.py` (ZenQuotes, 5s timeout) + `main.py` |
| JSON persistence, survives restarts, corrupt-file recovery | `storage.py` |
| Keyword / category search | `analytics.search`, menu 4 |
| Executive summary (totals, averages, status, trends) | `analytics.py`, menu 6 |
| Matplotlib charts (category, rating, monthly trend) | `plotting.py`, menu 7 |
| Markdown / TXT audit log / CSV export | `export.py`, menu 8 |
| Threshold warnings (duplicates, long text, rating 1, no author) | `analytics.warnings` |
| Bulk CSV import with row-by-row error log | `storage.import_csv`, menu 9 |
| Offline fallback to local mock cache | `api.py` -> `data/mock_cache.json` |

## Demo script (3-4 minutes)
1. `python main.py` - show the branded welcome screen.
2. Menu **10** - load the 43-entry sample dataset (includes edge cases).
3. Menu **2** - add a quote; type blank text, a rating of `9` and `abc` to show validation catching errors.
4. Menu **1** - fetch today's quote (falls back to cache if offline) and save it.
5. Menu **4** - search `wilde`, or filter by category `humor`.
6. Menu **6** - summary with status indicator and warnings.
7. Menu **9** - import `data/legacy_import_sample.csv` (3 rows added, 5 rejected with reasons).
8. Menu **7** and **8** - charts and reports appear in `reports/`.
9. Exit, relaunch, menu **3** - data persisted.

## Project layout
```
main.py              menu loop and workflows
api.py               API client + offline fallback
storage.py           JSON load/save, CSV import/export
validation.py        input wrappers and record validation
analytics.py         summaries, warnings, search
plotting.py          Matplotlib charts
export.py            MD / TXT / CSV reports
ui.py                colours, tables, pagination
make_sample_data.py  regenerates the files in data/
data/                sample_quotes.json/.csv, mock_cache.json, legacy_import_sample.csv
tests/test_app.py    unit tests
```
Run tests: `python -m unittest discover -s tests`

## Notes
- Your saved quotes live in `data/quotes.json` (created on first run).
- Set `NO_COLOR=1` to disable ANSI colours.
- The API (zenquotes.io) is rate limited; the app silently falls back to the cache when it fails.
