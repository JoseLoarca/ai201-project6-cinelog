# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in order to follow the project's naming convention.

**How I verified:** I used a project-wide search to confirm that all calls to `save_to_watchlist()` were updated accordingly.

## Comment 2 — Deduplication
**What I did:** I added deduplication logic to `add_to_watchlist()` in `services/watchlist_service.py`.
The way this works is that before adding a film to the user's watchlist, a check is made to make sure the film doesn't
exist already in the watchlist. If it does, an `AlreadyInWatchlistError` exception is raised. If it doesn't exist already,
the film is added to the watchlist.

**How I verified:** I wrote a unit test that makes sure an `AlreadyInWatchlistError` exception is raised if someone
attempts to add the same film to a watchlist more than one time.

## Comment 3 — Missing test
**What I did:** I added a `test_add_to_watchlist_nonexistent_film_raises` test to `tests/test_watchlist.py`. This test
validates that when adding a non-existent film to a watchlist a `FilmNotFoundError` exception is raised.

**How I verified:** I ran the test with `pytest tests/test_watchlist.py -v` and confirmed it passed.

## Comment 4 — Default visibility
**My position:** I believe watchlists should default to `public=True`. 

**Reasoning:** **CineLog** is described as a community film-tracking app. A community is built around people with shared 
interests who interact with one another. Because of this, **_watchlists should be public by default_**, as they encourage 
users to share their film interests, discover new movies, and engage with others. While some users may only want to use 
CineLog for its film tracking features, the app’s primary focus on community suggests that most users will benefit 
from public watchlists.

**Tradeoff acknowledged:** Currently, the app does not support changing the visibility of watchlists. Users who are 
interested in using CineLog only for its film tracking features might lose interest in the app once they find out that 
watchlists are public by default and can't be updated. 

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