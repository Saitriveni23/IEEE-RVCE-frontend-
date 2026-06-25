# RVCE Events — IEEE Student Branch Frontend

A single-page application for browsing, exploring, and bookmarking RVCE campus events.
Built for the **IEEE Student Branch RVCE Web Development Hiring Challenge 2026**.

## How to Run Locally

**Step 1 — Download the dataset:**
```bash
curl -o events.json "https://pub-d6db99c9b68842a5b6f527e86583f256.r2.dev/events.json"
```

**Step 2 — Start a local server:**
```bash
python3 -m http.server 4000
```

**Step 3 — Open in browser:**
```
http://127.0.0.1:4000
```

> The app first tries to load `events.json` from the local folder, then falls back to the remote CDN URL.

## Project Structure

```
rvce-events/
├── index.html     # Entire app — HTML + CSS + JS in one file
├── events.json    # Dataset (download separately, not committed)
└── README.md
```

## Features

### Core
- **Event Feed** — paginated grid (24/page) of all events
- **Event Detail** — full metadata, description, capacity bar, contact
- **Bookmarks** — saved events persisted in localStorage

### Additional
- Live search across title, category, location, organizer, description
- Category filter chips dynamically built from actual dataset
- Sort by date ascending, date descending, or A–Z
- Animated stat counters (total events, upcoming, categories)
- Registration capacity bar (turns amber when >85% full)
- Cancelled event detection and visual badge
- Responsive down to mobile
- Accessible — semantic HTML, aria attributes, keyboard navigation

## Messy Data Handling

| Issue | Fix |
|-------|-----|
| Nested location objects `{building, room_number, floor}` | `lo()` extracts and joins fields |
| Multiple date field names | Checked in order: `start_time → date → event_date → datetime` |
| Unix timestamps (seconds or ms) | Detected by magnitude and converted |
| `tags` array for category | First matching tag mapped to normalised category |
| `host_club` for organizer | Priority field chain covers all variants |
| Null / `"TBD"` / `"N/A"` strings | `ss()` strips them all |
| Duplicate IDs | Deduplicated via Set during parse |
| Invalid dates | `isNaN` guard → shown as "Date TBD" |
| Cancelled events | `is_cancelled` flag → greyed card + badge |

IEEE Student Branch RVCE · 2026
