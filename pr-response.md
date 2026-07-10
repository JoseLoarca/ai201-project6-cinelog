# PR Response Doc — CineLog Watchlist Feature

## AI Usage
I used Claude for stress-testing the design argument on the default sort order. I wrote my position and reasoning, and
asked it identify any tradeoffs that I was not acknowledging. 

It's response helped me identify a big tradeoff I was not considering: for certain apps/features, ordering records
alphabetically allows for easier search. 

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
**My position:** Watchlists should default to date added (desc) order by default.

**Reasoning:** We humans are interested in fresh, up-to-date content. When you open your gallery, you see the latest 
pic first. I go back to my oldest pics occasionally, but I have zero interest in seeing older pics first. The same applies
to messages, emails, playlists, etc. By sorting films by date added (desc) by default, we guarantee that users will 
always be seeing the latest, most up-to-date content.

Imagine the following scenario where the app sorts watchlists alphabetically: You added a movie that starts with the 
letter 'R' to a watchlist that already contains more than 100 records. This movie is the most recent one you watched. 
Two weeks later, you are talking about movies with your friends, and you want to remember the name of this movie. 
You open the app and boom, you have to scroll through several records in order to find this movie. It takes you a while 
to finally find it, and by the time you do it the conversation topic has already changed. Bummer.

**Engagement with reviewer's point:** I agree that records in the watchlist should default to date added (desc) order
by default. This sorting order guarantees that watchlists will have a linear, chronological order. Alphabetical order
could potentially make watchlists look messy and confusing.

One tradeoff I'd like to acknowledge: alphabetical order would make it easier to find a specific film by title. If I
know I'm looking for 'Interstellar', for example, I have a better idea of where to scroll to (letter I). Ordering by date
does not give you this: I know I watched Interstellar, but I don't remember when, so I'll have to scroll until I find it.

In my opinion, the best long-term solution would be to: default order by date added (desc), let users configure
their preferred sort order (date vs title), and also allow users to change the sort order when browsing a watchlist.

## Comment 6 — Rebase
**What conflicted:** I only had conflicts with my `.gitignore` file.

**How I resolved it:** Updated my `.gitignore` to the version I was using before rebasing.

**How I verified no conflict remains:** I didn't have any other highlighted conflicts, I was also able to run `git rebase --continue`.

## Milestone #4: `git log --oneline` output
```terminaloutput
41904be (HEAD -> feature/watchlist) docs: add rebase section to pr-response.md
5c9cd10 docs: add sort order decisions section to pr-response.md
9587078 docs: add default visibility decisions section to pr-response.md
b9eeef4 docs: add test documentation section for nonexistent films in add_to_watchlist to pr-response.md
7c9e2c0 test: add test for nonexistent film id in add_to_watchlist
4d7cbca fix: add deduplication check on add_to_watchlist to prevent duplicate watchlist entries
78a6b54 fix: rename save_to_watchlist to add_to_watchlist per naming convention
08529b7 fix: update film retrieval method to use db.session.get in collection and watchlist services
b344fae feat: add view watchlist and add film to watchlist endpoints
bbe206c (origin/main, origin/HEAD) Merge pull request #2 from ascherj/chore/add-gitignore
718a9a8 chore: add .gitignore for generated files
07ca580 refactor: migrate film IDs from integer to UUID
014ae54 feat: initial CineLog API with film collection feature
```

<img src="/images/git.png" alt="git log output"/>

## PR Description
Added watchlist feature to the app. This feature allows users to keep track of movies they want to watch.

Functions related to watchlists:
* `add_to_watchlist(user_id, film_id)`: allows a user to add a film to the watchlist. A film can only exist in the user
watchlist once.
* `remove_from_watchlist(user_id, film_id)`: allows a user to remove a film from the watchlist. 
* `get_watchlist(user_id)`: allows a user to retrieve their watchlist.

By default: watchlists are public and their records are sorted by date added (desc).

To test this feature run `pytest tests/test_watchlist.py -v`. Tests included:
* `test_add_to_watchlist_duplicate_raises`: a film can only exist in a watchlist once. Adding a duplicate record
should raise `AlreadyInWatchlistError`.
* `test_add_to_watchlist_nonexistent_film_raises`: a film must exist in the database in order to be added to the
watchlist. Adding a nonexistent film should raise `FilmNotFoundError`.
* `test_remove_from_watchlist_film_not_in_watchlist_raises`: a film can only be removed from a watchlist if it already
exists in the watchlist. Removing a film from a watchlist that does not exist in the watchlist should raise `NotInWatchlistError`.

All new functions and tests follow the project's naming convention.