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
**What I did:** I implemented the `test_add_to_watchlist_nonexistent_film_raises()` test case in `tests/test_watchlist.py`. This verifies that the `add_to_watchlist()` function correctly raises a `FilmNotFoundError` exception when an attempt is made to add a film that does not exist in the database. This test is the functional equivalent of the `test_add_to_collection_nonexistent_film_raises()` test found in `test_collection.py`.
**How I verified:** I followed the same fixture and assertion pattern used in `test_add_to_collection_nonexistent_film_raises()` to ensure consistency across the service layer. After implementing the test, I executed `pytest tests/test_watchlist.py -v`, which passed successfully. Finally, I ran the full test suite using `pytest tests/ -v` to confirm that all tests pass and no regressions were introduced.

## Comment 4 — Default visibility
**My position:** I believe the default visibility should be set to private (public=False). Although CineLog is a community-driven film tracking app, user privacy remains paramount. We must develop the platform according to industry best practices and adhere to data protection regulations. Therefore, I recommend a hybrid approach: keep the default visibility private, while providing users with an easy way to toggle their lists to public. This approach strikes a balance between protecting user privacy and empowering social users to share their lists, ultimately supporting our goal of building a community for movie lovers.
**Reasoning:** The primary reason for defaulting the visibility to private is to prioritize user privacy and ensure adherence to data protection regulations (ex: GDPR or CCPA ) regarding the collection, storage, and processing of personal information. Furthermore, a "private by default" approach encourages user trust, reduces the friction associated with adding sensitive content to a watchlist, and empowers users by giving them granular control over their data visibility.
**Tradeoff acknowledged:**
Making the default visibility private may contradict the primary goal of this application, as CineLog is designed as a community film-tracking app. This approach could potentially diminish the sense of community among users, as it discourages the sharing of interests. Consequently, this may negatively impact social interaction within the platform.

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