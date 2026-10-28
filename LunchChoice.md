# Lunch Choice

A week view where each person taps a day and chooses the lunch their
parents will pack. Today is highlighted, days ahead are shown, and each
person chooses independently — so a household with several kids gets one
choice per kid per day, not one choice for the house.

## Use it

- **Week view:** the current 7 days, starting today. Each day card shows
  every person's choice (or a dash if they haven't chosen). Tap a day to
  choose. Past days are shown greyed out and can't be changed.
- **Choose:** in the dialog, pick whose lunch you're setting (the person
  chips), then tap an option. The choice saves immediately; the dialog
  moves to the next person who hasn't chosen yet, if there is one.
  **Clear choice** removes the selected person's choice for that day.
  **Done** closes the dialog.
- **Today box:** the top of the week view always shows today's choices
  at a glance, so packing in the morning is one look.
- **Week navigation:** ‹ and › move a week at a time; **Today** jumps
  back to the current week.
- **Manage:** add, edit, and delete people and lunch options. Deleting
  is two-tap (tap Delete, then Sure?). Deleting a person also removes
  their choices; deleting an option clears it from any day it was
  chosen on.

## Setup (parents)

1. Open **Manage** and add the people who choose lunch: a name, a face
   emoji, and a color. Nothing is pre-filled — the first run is empty
   on purpose, so no example names ever look like real household data.
2. Add lunch options: a name, a symbol emoji, a color, and optionally
   a picture URL (see below).
3. That's it. Kids tap a day and tap their lunch.

## Pictures and pre-readers

Every option shows a big symbol, so a child who can't read yet can
still choose: the emoji is the picture. Color helps too — give each
option a distinct color and the choice is recognizable at a glance.

An option can also show a remote picture: put an `http(s)` picture URL
in the option form and it renders instead of the emoji (with the emoji
as fallback if the picture fails to load). Only `http(s)` links are
accepted.

Uploading a picture file from inside the widget is **not** supported
in v1, and that is a platform limit, not an oversight:

- A plugin frame's token only covers its own plugin storage, settings,
  Home Assistant, and notify routes. There is no plugin-scoped upload
  route, and the SDK exposes storage (plus events and celebrations) —
  no upload method.
- The existing upload routes are core routes (sounds, user pictures),
  not reachable with a plugin token.
- Plugin storage caps each key at 64 KB, so full photos stored as data
  URLs would not fit a whole household document anyway.

What picture uploads would need (follow-up, needs a core change): a
plugin-scoped upload route or SDK media method, a per-plugin uploads
namespace served under the existing `/Uploads/` static root, and a
size/count policy. Until then, emoji + color is the supported
pre-reader path, and a remote picture URL is the supported photo path.

## Data model

Plugin storage key `data`:

```json
{
  "version": 1,
  "people": [{ "id": "...", "name": "...", "emoji": "🧒", "color": "#64b5f6" }],
  "options": [{ "id": "...", "name": "Sandwich", "emoji": "🥪", "color": "#ffb74d", "imageUrl": null }],
  "choices": { "2026-10-29": { "<personId>": "<optionId>" } },
  "updatedAt": "2026-10-28T00:00:00.000Z"
}
```

Dates are local `YYYY-MM-DD`. The widget polls storage (default every
10 seconds, setting `pollSeconds`) so a choice made on one display
appears on the others; a refresh is skipped while any dialog is open,
and concurrent edits resolve last-write-wins.

## What it doesn't do

No prices, no reminders or due dates (those belong to chores), no
syncing with other apps, and no connection to any other plugin's data.
Plugin storage is isolated per plugin, so this widget cannot read or
share another plugin's notes — including a whiteboard plugin's daily
notes. If a household wants the lunch choice written somewhere else,
that is a separate integration, not part of this widget.
