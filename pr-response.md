# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** I renamed the `save_to_watchlist()` function to `add_to_watchlist()` in `services/watchlist_service.py`.
**How I verified:** I identified all call sites by highlighting the function name and pressing `Shift + Alt + F12` in VS Code. This allowed me to locate and update every reference throughout the project to ensure consistency.

## Comment 2 — Deduplication
**What I did:** I implemented deduplication logic in the `add_to_watchlist()` function within `services/watchlist_service.py`. This ensures that a user cannot add the same film to their watchlist more than once. If a user attempts to add an existing film, the function now raises an `AlreadyInWatchlistError` exception.
**How I verified:** I followed the pattern established in `add_to_collection()`, which queries the database for an existing entry and raises an `AlreadyInWatchlistError` if a duplicate is found. I verified this by running my test case to confirm that adding a duplicate film now correctly triggers the expected error rather than creating a new database record.

## Comment 3 — Missing test
**What I did:**
**How I verified:** 

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->