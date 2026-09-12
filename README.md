# Command Center

A single-file dashboard for ADHD: task matrix, care routines, focus timers, and a retrospective that reads your history.

**Live:** https://anuragmardia.github.io/command-center

It syncs across devices through a private GitHub gist you own. No server, no accounts, no analytics. Your data stays in your gist, which only your token can read.

---

## The day in five moves

1. **Commit 3** — Hit *Choose focus*. Pick up to three from *Do now*. Empty slots are fine. This is the whole plan.
2. **Start one** — Press ▶ on a focus card. A 25-minute block runs. Keep browsing; the timer keeps its own time.
3. **When you stick** — Hit *I'm stuck* or *Body double 5 min*. The coach narrows to one 60-second action.
4. **Drift?** — Tap *I drifted*. It shows what you were doing. No lecture, no streak lost, no penalty.
5. **Close the day** — Hit *Enough for today*. The page dims. Tomorrow starts clean.

The whole loop: three tasks, one 25-minute block, close the day. Everything else is optional on top of that.

---

## The top bar

| Icon | What it does |
|---|---|
| 📅 **Calendar** | Flip to any day. Dots mean something is due. |
| 🕐 **Clock** | Local time, anchored without leaving the page. |
| 🌤️ **Weather** | Tap to set up once. Seven-day forecast after. |
| **Quick add** | Natural language: `"Dentist tomorrow 3pm"`, `"Pay rent 15 sep"`, `"Gym friday"`. Parses date, time, links. Lands in **Inbox**. |
| ↩ **I drifted** | Shows your committed tasks and which is active. Built for returning, not scolding. |
| 🔔 **Check-ins** | Hourly nudge: *"No wrong answer here. Want to come back, or stop for the day?"* Silence any time. |
| ☁ **Sync** | Green means this device and the gist agree. |

---

## The five tabs

### Today
Reframe card, timeline, focus row, coach, strategy tip. Start here.

### Matrix
Inbox strip on top, then Do now · Schedule · Delegate · Parking lot. Fits one screen. Groups are collapsed by default — expand when you want to look. Double-click a task to open it. Drag a task onto another to group them into a project.

**Imports from TickTick route by priority:**
- Priority `5` (High) → Do now
- Priority `3` (Medium) → Schedule
- Priority `1` (Low) → Batch / delegate
- Priority `0` (None) → Inbox

### Follow up
Tasks where your part is done. Three tiers: set a reminder · nudge them · let it go.

### Projects
Upgrade a big task, then hit **✨ Break down** to generate small actions. Kept for outcomes, not lists.

### History
30-day care rhythm chart, category breakdown (Morning / Day / Night), day-by-day list, retrospective, export and restore. Read rarely.

The **retrospective** extrapolates what consistency bought you: calories avoided, minutes of sleep gained, hours of attention rebuilt. It's honest math, not encouragement.

---

## Things it does differently

- **Timers that tell the truth.** Every countdown is anchored to a wall-clock end time. Switch tabs, close the laptop — the timer is still right.
- **Drift is expected.** There's a whole button for it. No streak, no chain, no daily reset that erases yesterday.
- **Stopping is a supported action.** "Enough for today" is a first-class button, not the absence of activity.
- **Every routine has a `why?`** Tap it. It shows the actual reason and what happens if you skip. Built for a brain that needs the logic before it moves.
- **Crisis routing.** If you type something about self-harm, the coach stops coaching and shows real numbers. That's not a feature — it's the floor.

---

## Sync

Syncs through a private gist you own. Data is a JSON snapshot. Last write wins, keyed on `_updatedAt`.

### One-time setup

1. Create a **secret gist** at gist.github.com with a file named `command-center.json` containing `{}`.
2. Create a **classic token** at github.com/settings/tokens with only the `gist` scope ticked.
3. Open the dashboard. Click **☁ Sync** top-right. Paste token and gist ID. Tick **Enable sync**. Click **Test**, then **Save**.

Repeat on every device with the same token and gist ID.

### Button states

| State | Meaning |
|---|---|
| ☁ **Sync** (plain) | Off. Nothing is saving to cloud. |
| ↻ **Syncing** | Pushing right now. Momentary. |
| ☁ **Synced** (green) | This device and the gist match. |
| ⚠ **Sync error** (red) | Click it. Usually token or gist ID. |

### What lives where

- The **HTML file** is public code. Anyone with the URL sees the dashboard.
- Your **data** lives in the secret gist. Only a token with `gist` scope can read it.
- The **token** sits in this browser's `localStorage`. Anyone with unlocked access to a device can read every gist in your account. Scope it to `gist` only.

---

## When it goes sideways

- **Can't start** — Tap *Make it smaller*. It gives you the first 60 seconds of a specific task, not a pep talk.
- **Overwhelmed** — Tap *Slow breaths*. 4-6-8 protocol, six rounds. Stop any time.
- **Task won't move** — Open it → *Upgrade to project* → *Break into steps*. Eight small actions beat one big one.
- **Bigger than tasks** — Type what's real into the coach. If it's crisis language, it stops coaching and shows real numbers.

---

## What this is not

It's a scaffold for time-blindness and task-initiation. Not treatment, not therapy, not a substitute for either. If what you're carrying is bigger than task management, the coach will say so, and it will point you toward real help.

The Pixel Watch cannot run this. Nothing browser-based will. If you want your Top 3 on your wrist, the realistic path is mirroring into Google Tasks or Todoist.

**One rule:** the tool works when you open it, and doesn't when you don't. That's not a moral failure — it's the actual constraint. If three days go by without opening it, open it today. Nothing is lost, nothing is behind.

---

## Development

Single HTML file. No build step. No framework. FullCalendar is fetched from a CDN at load.

To modify: edit the HTML, commit to the repo, refresh. Every device picks up the change on next open. Your data is untouched because it lives in the gist, not the code.

**Structure inside the file:**
- `routines[]` — the care board items, each with `why` and `skip` fields
- `ROUTINE_GAINS{}` — the retrospective lookup table
- `ADHD_REFRAMES[]` / `ADHD_TIPS[]` — the rotation content
- `parseTickTickCSV()` — priority-to-quadrant mapping
- `parseQuickAdd()` — natural-language date/time
- Sync block near the bottom: `gistPull`, `gistPush`, `bootSync`
