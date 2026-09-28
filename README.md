# 🏫 Free Class Locator

A zero-dependency web tool that tells students which classrooms and labs are empty right now, so they can find a quiet place to work without walking floor to floor.

It reads the SEEE (School of Electrical and Electronics Engineering) class timetables at SRM Institute of Science and Technology, Tiruchirappalli, works out when each room is occupied, and shows what is free.

## Features

- **Floor Grid:** every room is listed floor by floor (ground to 7th). Free rooms are green and show *free until*; busy rooms are red and show *free from*.
- **Time control:** choose any weekday and time, or press **Now**. Outside class hours it shows the next working day at 9:00 AM.
- **Smart-Search Room Finder:** type a plain sentence such as:
  > I need an AC room on the first floor for me and my team for the next 2 hours

  The finder pulls out the AC/non-AC preference, floor, lab or classroom, duration, start time and day. It then lists only the rooms that stay free for the whole window, sorted by how long they remain free. If nothing matches, it says so and shows the closest free rooms.
- **3D Building Map:** stacked floors with color-coded rooms, a rotate slider and a flat-view toggle.
- **Live countdown:** click a room to see the time left before its next class (or until it frees up).
- **Call the Squad:** claim a free room and open a pre-filled WhatsApp message such as "📍 Heading to IST 602. It's free until 10:45 AM. Come fast!"
- Light and dark theme, mobile friendly, no build step.

## How it works

1. Each timetable is stored as a compact grid: one string per weekday, one character per period. A letter means a theory class in the section's home room, `-` is lunch, and `.` is no class.
2. Classes held elsewhere (labs, CDC 625, TB 106, workshops and so on) are stored as extra entries: room, day, periods.
3. At startup these are turned into busy time intervals for each room, using each section's own period timings.
4. A room is free for a request if none of its busy intervals overlaps the requested window.

## Dataset

11 timetables:

| Year | Sections | Term |
|------|----------|------|
| IV | ECE-A | Odd sem 2026-27 |
| III | ECE-B, ECE-DS, BME | Odd sem 2026-27 |
| II | ECE-DS A, ECE-DS B, BME | Odd sem 2026-27 |
| I | ECE-A, ECE-B/EEE, ECE-DS, Biotech-B (BME) | 2024-25 sheets |

## Run locally

No install needed. Either open `index.html` in a browser, or serve it:

```bash
python -m http.server 8000
# then open http://localhost:8000/index.html
```

## Assumptions and limitations

- **Floors** are taken from room numbers (for example IST 416 is on the 4th floor). Workshop rooms 20 and 21 are treated as ground floor.
- **AC status is not in the timetables.** Labs, CDC 625 and TB 106 are assumed to be AC and the classrooms and workshops non-AC. Edit the `META` object in the page source to correct this.
- The first-year sheets are from 2024-25 and use different period timings. Some of their rooms overlap with the 2026-27 sheets (for example IST 602).
- Periods with no room named (Chemistry Lab, Yoga, some CDC periods) are not counted.
- The room finder parses text with rules that run in the browser. It does not call a language model.
- Only rooms that appear in the 11 timetables are known; other rooms are not tracked.

## Project structure

```
index.html   # entire app: data, logic, UI
README.md
```

## Roadmap

- Language-model query parsing for free-form requests
- Timetable data in a separate JSON file
- Room capacity and team-size filtering
- Import for new timetable PDFs

## Tech

HTML, CSS and vanilla JavaScript. No frameworks or dependencies.
