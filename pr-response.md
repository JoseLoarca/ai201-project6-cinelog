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
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->