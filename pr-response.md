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