# Block World Platform

A single-file AI planning competition platform built for classrooms. Students learn the Block World problem (BFS, A\*), practice interactively, and compete live — all synced in real time via Firebase.

---

## Quick Start

1. Open `block_world_platform_v5.html` in any modern browser.
2. Sign in as the organizer or register a student account.
3. That's it — no build step, no server, no dependencies to install.

---

## Default Organizer Login

| Field    | Value                  |
|----------|------------------------|
| Email    | `dssh4861@gmail.com`   |
| Password | `organizer123`         |

> To change these, edit the `ORGANIZER` constant near the top of the `<script>` block.

---

## Firebase Setup (Real-Time Sync)

Without Firebase the platform runs in **local-only mode** — everything works but data is stored in `localStorage` and won't sync across devices.

To enable cross-device real time sync:

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a project.
2. Add a **Web App** and copy the config object.
3. In the Firebase console → **Build → Realtime Database → Create database**.
4. Set the security rules to:
   ```json
   { "rules": { ".read": true, ".write": true } }
   ```
5. In the HTML file, replace the `firebaseConfig` block (lines 8–16) with your own values.

---

## Organizer Access Code

New organizer accounts created through Sign Up require an access code:

```
ORG-2024
```

If the correct code is entered the account is approved instantly. Otherwise it enters a pending queue visible on the organizer dashboard.

> To change the code, edit `const ORG_ACCESS_CODE = 'ORG-2024'` in the script.

---

## Roles

### Organizer
- View dashboard stats (students, pending, competitions, live count).
- Add / remove students manually.
- Approve or reject organizer sign-up requests.
- Create competitions with a name and difficulty level.
- Start / stop competitions — students see a live popup the moment one goes live.
- View **per-competition rankings** (📊 Ranks button on each competition row).
- View all-time top scorers with animated podium.

### Student
- Register with name, email, department, and Student ID.
- Complete 8 guided lessons (see Curriculum below).
- Interactive block simulator with step-by-step feedback.
- Join live competitions — each student gets a **different puzzle variant** seeded by their email so answers can't be shared.
- Submit a move sequence and get an instant score.
- View live leaderboard during the competition.

---

## Departments

Students choose one of the following during registration:

| Short Code | Full Name |
|------------|-----------|
| NwC | Networking with Communication |
| Cintel | Computational Intelligence |
| Ctech | Computational Technology |
| DSBS | Data Science & Business Systems |

---

## Curriculum (8 Lessons)

| # | Title | Type |
|---|-------|------|
| 1 | What is Block World? | Info + block viz |
| 2 | States & Rules | Precondition reference cards |
| 3 | Interactive Practice | Live block simulator |
| 4 | BFS Planning | Step-by-step animated explanation |
| 5 | BFS Practice Puzzle | Student solves an easy puzzle |
| 6 | A\* Search | Step-by-step animated explanation |
| 7 | A\* Practice Puzzle | Student solves a medium puzzle |
| 8 | Knowledge Quiz | 3-question MCQ, unlocks Competition Mode |

---

## Competition System

### Difficulty Levels & Scoring

| Difficulty | Blocks | Optimal Moves | Timer Bonus | Move Penalty |
|------------|--------|--------------|-------------|--------------|
| 🟢 Easy    | 2      | 2            | 200 pts     | −15 per extra move |
| 🟡 Medium  | 3      | 3–5          | 250 pts     | −20 per extra move |
| 🔴 Hard    | 4      | 7–9          | 300 pts     | −25 per extra move |

**Score formula:**
```
score = max(0, timerBonus − time_seconds) + max(0, optimalBonus − (moves − optimal) × penalty)
```

### Unique Puzzles Per Student

Each student is assigned a puzzle variant deterministically from the pool using a hash of their email + competition ID. Students in the same competition see different puzzles so they cannot copy each other's move sequence.

### Puzzle Pool

Each difficulty has 3 variants:

- **Easy** — 2-block problems (stack / unstack)
- **Medium** — 3-block rearrangements
- **Hard** — 4-block full reversals and interleaved stacks

---

## Actions & Preconditions Reference

| Action | Preconditions |
|--------|---------------|
| `PICKUP(X)` | X is on the table, X is clear, arm is empty |
| `PUTDOWN(X)` | Arm is holding X |
| `STACK(X, Y)` | Arm is holding X, Y is clear |
| `UNSTACK(X, Y)` | X is directly on Y, X is clear, arm is empty |

---

## Key Files

```
block_world_platform_v5.html   ← entire platform (single file)
README.md                      ← this file
```

---

## Browser Support

Works in any modern browser (Chrome, Firefox, Edge, Safari). No internet connection required after the initial page load — unless Firebase sync is enabled.

---

## Customisation

| What | Where in the HTML |
|------|-------------------|
| Organizer credentials | `const ORGANIZER = { … }` |
| Access code | `const ORG_ACCESS_CODE` |
| Firebase config | `const firebaseConfig = { … }` |
| Add puzzle variants | `const PUZZLE_POOL` |
| Add practice puzzles | `const PRACTICE_PUZZLES` |
| Simulated competitor names | `const FAKE_NAMES` |
| Scoring formula | `function calcScore(…)` |
| Difficulty config | `function getDifficultyConfig(…)` |
