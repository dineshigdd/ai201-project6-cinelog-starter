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
**My position:** I believe the default sort order should be `date-added`, as users typically prefer to see the most recently added films at the top of their list. However, I recommend providing a control that allows users to toggle between alphabetical and date-added sorting.
**Reasoning:**
For users who choose to share their watchlists, the primary goal is often to engage with friends and the broader community by showcasing their current interests and upcoming film plans. By defaulting to date-added, we ensure that the most relevant, current items are displayed prominently. Supporting this social interaction not only strengthens our community but also provides valuable data for future features, such as a personalized recommendation engine based on user preferences. By prioritizing chronological order as the default, we create a predictable activity stream that facilitates social discovery and future content-recommendation features.

Conversely, for users who prioritize privacy, providing the option to keep their watchlists private is essential. In these cases, offering an alphabetical sort option provides a cleaner, more organized way for them to manage their personal collections.

**Engagement with reviewer's point:**
I agree with the reviewer's observation that most users prefer to see their most recently added films first. This is the most intuitive way to sort a watchlist and aligns perfectly with the app’s goal of building a community of movie lovers. Sorting alphabetically by default could diminish the user experience, as it would force users to scroll through their entire list to find the films they added most recently. However, as noted, implementing a manual sort control will provide the necessary flexibility for users who prefer an alphabetical structure for their personal collections.

## Comment 6 — Rebase
***What conflicted:***
I encountered a merge conflict in `.gitignore` due to my local inclusion of `.pytest_cache`, which conflicted with the updated `main` branch. 
While the rebase process itself did not flag conflicts within `services/watchlist_service.py`, I identified a subsequent issue where the `WatchlistEntry` model was missing or altered in `models.py`.
***How I resolved it:***
1.  **Git Conflict:** I resolved the merge conflict in `.gitignore` by merging my local configuration with the required project-standard exclusions. After staging the changes with `git add .gitignore`, I completed the rebase using `git rebase --continue`.
2.  **Schema & Implementation Updates:** 
    *   **Models:** Updated `models.py` to define the `WatchlistEntry` model, ensuring the `film_id` property uses `db.String(36)` to support UUIDs: `film_id = db.Column(db.String(36), db.ForeignKey("film.id"), nullable=False)`.
    *   **Service Layer:** Updated the docstring in the `add_to_watchlist()` function within `services/watchlist_service.py` from `film_id (int)` to `film_id (str)` to correctly reflect the UUID requirement. No changes to the underlying logic were required as the implementation was already compatible.
    *   **API Documentation:** Updated the route documentation in `routes/watchlist/watchlist.py` for the POST endpoint to clarify that the request body now expects a UUID string (`{ "film_id": "<uuid>" }`) rather than an integer.

***How I verified no conflict remains:***
I verified the stability of my changes by running the full test suite. Specifically, I ran `pytest tests/test_watchlist.py -v` to confirm that `add_to_watchlist()` handles the new UUID requirements correctly and satisfies all test cases. Additionally, I ran `pytest tests/test_collection.py -v` to ensure that my changes did not introduce any regressions in the existing collection service functionality. All tests passed successfully.

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->