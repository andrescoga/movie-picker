# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Movie Picker is a single-file web application for coordinating movie night selections among multiple users. Users can login with their name, view a curated list of movies, and vote on each movie using three options: "Yes", "Maybe", or "Veto". All data is stored client-side in localStorage.

## Architecture

This is a **single-page application** contained entirely in `index.html` with no build process, dependencies, or external tooling. The application consists of:

- **Inline CSS** (lines 9-275): All styles embedded in `<style>` tag
- **Inline JavaScript** (lines 326-463): All application logic embedded in `<script>` tag
- **No external dependencies**: Pure vanilla HTML/CSS/JS

### Key Components

**Authentication System** (lines 376-407):
- Simple localStorage-based user sessions
- No password or backend authentication
- Current user stored in `localStorage.currentUser`
- `init()` checks for existing session on page load

**Data Model** (lines 327-370):
- `movies` array is the hardcoded source of truth
- Movie objects contain: `id`, `title`, `year`, `poster`, `summary`
- Poster images sourced from TMDB API (`image.tmdb.org`)

**Vote Storage System** (lines 409-419):
- Votes stored per-user using key pattern: `votes_{username}`
- Vote data structure: `{ movieId: "yes"|"maybe"|"veto" }`
- Each user has independent vote state

**UI Flow**:
1. Login screen (`#loginScreen`) → Enter name → Login
2. Main app (`#app`) → Movie grid with vote badges
3. Modal (`#modal`) → Movie details and vote buttons

## Development

**Running the application**:
```bash
# Open directly in browser
open index.html

# Or serve with any static file server
python3 -m http.server 8000
# Then visit http://localhost:8000
```

**Testing changes**:
- Modify `index.html` and refresh browser
- Clear localStorage to reset user data: `localStorage.clear()` in browser console
- Test different users by logging out and logging in with different names

**No build, lint, or test commands** - this is a static HTML file with no tooling.

## Key Implementation Details

**localStorage Keys**:
- `currentUser`: Currently logged in username (string)
- `votes_{username}`: Per-user vote data (JSON object mapping movieId to vote choice)

**Movie Grid Rendering** (lines 421-439):
- Dynamically generated from `movies` array
- Reads current user's votes and displays badges
- Re-renders on vote changes

**Modal State Management**:
- `currentMovie` global holds active movie during modal display
- Modal opened via `openModal(movieId)` from grid click
- Voting saves to localStorage and re-renders grid

## Adding New Movies

To add movies to the picker, edit the `movies` array (lines 327-370):

```javascript
{
    id: 7,  // Must be unique
    title: "Movie Title",
    year: 2024,
    poster: "https://image.tmdb.org/t/p/w500/posterID.jpg",  // TMDB poster URL
    summary: "Movie description..."
}
```

**Note**: Poster URLs should use TMDB's image API format. Movie IDs must be unique integers.
