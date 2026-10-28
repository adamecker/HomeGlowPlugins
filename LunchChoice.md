# Lunch Choice

A calendar week view where each person taps a day and chooses the
lunch their parents will pack. It is built for school lunches, so the
default week is Monday–Friday. Each person chooses independently — a
household with several kids gets one choice per kid per day, not one
choice for the house.

## Use it

- **Week view:** the current calendar week, Monday–Friday by default.
  Each day card shows every person's choice (or a dash if they haven't
  chosen). Tap a day to choose. Past days are shown greyed out and
  can't be changed. Today is highlighted when it is visible.
- **Week navigation:** ‹ and › move a week at a time; **Today** jumps
  back to the current week.
- **Today box:** the top of the week view shows today's choices at a
  glance, so packing in the morning is one look. On Saturday or Sunday
  with weekends hidden there is no school lunch to pack, so the Today
  box is hidden too.
- **Weekends (optional):** Manage has a "Show Saturday and Sunday"
  checkbox, off by default. Turning it on shows all seven days. It is
  a household-wide setting — every display sees the same week shape.
- **Weekend edge case:** when today is Saturday or Sunday and weekends
  are hidden, there is nothing pickable this week, so the week view
  (and the Today button) default to *next* week's Monday–Friday
  instead of showing an empty week.
- **Choose:** in the dialog, pick whose lunch you're setting (the
  person chips), then tap an option. The choice saves immediately; the
  dialog moves to the next person who hasn't chosen yet, if there is
  one. **Clear choice** removes the selected person's choice for that
  day. **Done** closes the dialog.
- **Manage:** add, edit, and delete people and lunch options. Deleting
  is two-tap (tap Delete, then Sure?). Deleting a person also removes
  their choices; deleting an option clears it from any day it was
  chosen on.

## One widget, or one kid per display

There are two supported ways to use the widget:

- **Mode A — one widget, everyone:** the default. Every person appears
  on every day card, and the choice dialog steps through the person
  chips with auto-advance. The wall usually runs this way.
- **Mode B — one display, some people:** Manage has an "On this
  display" section. Uncheck someone there and they disappear from
  *that display's* week view, Today box, and choice dialog — other
  displays are not affected. Checking everyone again (or "Show
  everyone on this display") restores everyone. This is how a display
  shows only one kid while the wall still shows all of them.

Mode B is stored on the display itself (the plugin's localStorage
shim), never in the shared household document — that is what makes it
per-display. A filter that ends up matching nobody falls back to
everyone, so a display can never go blank by filtering.

**Honest platform limit:** the same plugin cannot appear twice on one
tab with different filters. Dashboard assignments are per-plugin per
device (a widget name maps to a list of tab numbers), and the
localStorage shim is keyed by plugin filename per display — so two
copies on one tab would mirror each other, including the filter.
Per-kid views work across *different displays* (wall vs. tablet, for
example), not as two instances on one tab. Do not promise per-tab
instances; the supported path is per-display filtering.

## Setup (parents)

1. Open **Manage** and add the people who choose lunch: a name, a face
   emoji, and a color. Nothing is pre-filled — the first run is empty
   on purpose, so no example names ever look like real household data.
2. Add lunch options: a name, a symbol emoji, a color, and optionally
   a picture URL (see below).
3. If you pack weekend lunches too, turn on "Show Saturday and
   Sunday" in Manage. Otherwise leave it off — Monday–Friday is the
   default.
4. If a display should show only some people, use "On this display"
   in Manage on that display. The wall can keep showing everyone.
5. That's it. Kids tap a day and tap their lunch.

## Pictures and pre-readers

Every option shows a big symbol plus its name, so a child who can't
read yet can still choose: the emoji is the picture, and the text is
there for readers. Color helps too — give each option a distinct color
and the choice is recognizable at a glance. Emoji + text is the v1
answer, and it is enough.

An option can also show a remote picture: put an `http(s)` picture URL
in the option form and it renders instead of the emoji (with the emoji
as fallback if the picture fails to load). Only `http(s)` links are
accepted.

Uploading a picture file from inside the widget is **not** supported
in v1, and that is a platform limit, not an oversight: a plugin
frame's token only covers its own plugin storage, settings, Home
Assistant, and notify routes (no plugin-scoped upload route or SDK
upload method), and plugin storage caps each key at 64 KB, so full
photos stored as data URLs would not fit a whole household document
anyway. Real uploads would need a core change; no proposal is drafted
here — emoji + text (plus an optional remote URL) is the supported
path for now.

## Data model

Plugin storage key `data`:

```json
{
  "version": 1,
  "people": [{ "id": "...", "name": "...", "emoji": "🧒", "color": "#64b5f6" }],
  "options": [{ "id": "...", "name": "Sandwich", "emoji": "🥪", "color": "#ffb74d", "imageUrl": null }],
  "choices": { "2026-10-29": { "<personId>": "<optionId>" } },
  "showWeekends": false,
  "updatedAt": "2026-10-28T00:00:00.000Z"
}
```

Dates are local `YYYY-MM-DD`. The widget polls storage (default every
10 seconds, setting `pollSeconds`) so a choice made on one display
appears on the others; a refresh is skipped while any dialog is open,
and concurrent edits resolve last-write-wins.

**Migration:** documents saved before the week-shape change have no
`showWeekends` field and default to `false` (Monday–Friday), so
existing choices, people, and options are untouched. The per-display
people filter is not in this document at all — it lives in the
display's localStorage shim under `homeglowLunchChoice.displayPeople`
(an array of person ids; missing or empty means everyone).

## What it doesn't do

No prices, no reminders or due dates (those belong to chores), no
syncing with other apps, and no connection to any other plugin's data.
Plugin storage is isolated per plugin, so this widget cannot read or
share another plugin's notes — including a whiteboard plugin's daily
notes. If a household wants the lunch choice written somewhere else,
that is a separate integration, not part of this widget.
