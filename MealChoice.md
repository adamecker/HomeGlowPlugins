# Meal Choice

A calendar week view where each person taps a day and chooses the
meals their parents will pack. It is built for school days, so the
default week is Monday–Friday. Each person chooses independently — a
household with several kids gets one choice per kid per day per meal,
not one choice for the house.

Four meals exist — Breakfast, Snack, Lunch, Dinner — each toggleable
from Manage. Lunch is on by default; the others are off until turned
on. Turning a meal off hides it everywhere but keeps its saved picks,
so turning it back on restores them.

## Use it

- **Week view:** the current calendar week, Monday–Friday by default.
  Each day card shows every selected person's choice per enabled meal
  (or a dash if they haven't chosen). Tap a day to choose. Past days
  are shown greyed out and can't be changed. Today is highlighted
  when it is visible. The week view fits its container: it starts at
  the next meal to pick (tomorrow, or the next school day when
  tomorrow is not one) and shows as many days as fit — a small widget
  shows only that next day, a large one shows the full week. When
  this week's meals are done (for example Friday with weekends
  hidden), the view moves to next week, since that holds the next
  meal to pick.
- **Week navigation:** ‹ and › move a week at a time; **Today** jumps
  back to the current week.
- **Today box:** the top of the week view shows today's choices at a
  glance, so packing in the morning is one look. On Saturday or Sunday
  with weekends hidden there is no school meal to pack, so the Today
  box is hidden too.
- **Weekends (optional):** Manage has a "Show Saturday and Sunday"
  checkbox, off by default. Turning it on shows all seven days. It is
  a household-wide setting — every display sees the same week shape.
- **Weekend edge case:** when today is Saturday or Sunday and weekends
  are hidden, there is nothing pickable this week, so the week view
  (and the Today button) default to *next* week's Monday–Friday
  instead of showing an empty week.
- **Choose:** in the dialog, pick whose meal you're setting (the
  person chips), then tap an option. The choice saves immediately; the
  dialog moves to the next person who hasn't chosen yet, if there is
  one. **Clear choice** removes the selected person's choice for that
  day and meal. **Done** closes the dialog. When more than one meal
  is enabled, the dialog has a tab per enabled meal; with a single
  meal enabled there are no tabs.
- **Meals (optional):** Manage has a Meals list — Breakfast, Snack,
  Lunch, Dinner — each with its own checkbox. Lunch is on by default;
  the others are off. Turning a meal on adds a pick row for it to each
  day, alongside the other enabled meals, and a tab for it in the
  choice dialog. Picks are per day per person per meal, and never
  affect other meals' picks. Like weekends, it is a household-wide
  setting — every display sees the same meal rows. Turning a meal off
  hides its picks but keeps them saved, so turning it back on
  restores them. At least one meal stays on.
- **Manage:** select the household users who choose meals, and add,
  edit, and delete options. Each option has a Meals tag set: check
  any combination of Breakfast, Snack, Lunch, Dinner; Manage shows a
  badge per option (for example Lunch, Snack, or Lunch + Snack). When
  adding an option, typing a name that closely matches a known food
  (banana, sandwich, yogurt, and so on) pre-fills that food's emoji
  as the default — a default only: tap a different emoji anytime and
  your pick sticks, and a name with no close match leaves the emoji
  unset (saving then uses 🍽️, as before). Editing an existing option
  never changes its emoji. An
  option appears in the picker of every meal it is tagged with, and
  never in a meal it isn't tagged with. There is no create-person
  form: Manage lists the household's HomeGlow users (read from the
  household user list, which the plugin's frame token is allowed to
  read) and parents check who chooses meals. Deleting an option is
  two-tap (tap Delete, then Sure?) and clears it from any day and
  meal it was chosen on. Unselecting a user hides them from the week
  view; their past choices stay in the document but are not shown.

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

1. Open **Manage** and select the household users who choose meals.
   The list comes from the household's HomeGlow users — add or
   rename users in Admin, not here. Nothing is pre-selected: the
   first run has nobody selected on purpose.
2. Add meal options: a name, a symbol emoji, and a color, plus the
   meals each option applies to (check Breakfast, Snack, Lunch,
   Dinner as needed — an option tagged with several meals appears in
   each of those pickers). The emoji auto-fills for known foods as
   you type the name; change it if you like.
3. If you pack weekend meals too, turn on "Show Saturday and
   Sunday" in Manage. Otherwise leave it off — Monday–Friday is the
   default.
4. If kids pick more than lunch, turn on the extra meals in
   Manage's Meals list (for example Snack) and make sure options are
   tagged with those meals — or tag an existing option with another
   meal so it appears in that picker too. Leave a meal off and
   nothing about it appears.
5. If a display should show only some people, use "On this display"
   in Manage on that display. The wall can keep showing everyone.
6. That's it. Kids tap a day and tap their meals.

## Pictures and pre-readers

Every option shows a big symbol plus its name, so a child who can't
read yet can still choose: the emoji is the picture, and the text is
there for readers. Color helps too — give each option a distinct color
and the choice is recognizable at a glance. Emoji + text is the v1
answer, and it is enough.

Uploading a picture file from inside the widget is **not** supported
in v1, and that is a platform limit, not an oversight: a plugin
frame's token only covers its own plugin storage, settings, Home
Assistant, and notify routes (no plugin-scoped upload route or SDK
upload method), and plugin storage caps each key at 64 KB, so full
photos stored as data URLs would not fit a whole household document
anyway. Real uploads would need a core change; no proposal is drafted
here — emoji + text is the supported path for now.

## Data model

Plugin storage key `data`:

```json
{
  "version": 1,
  "selectedUserIds": [1, 2],
  "options": [{ "id": "...", "name": "Sandwich", "emoji": "🥪", "color": "#ffb74d", "meals": ["lunch"] }],
  "picks": {
    "lunch": { "2026-10-29": { "u1": "<optionId>" } },
    "snack": { "2026-10-29": { "u1": "<optionId>" } }
  },
  "enabledMeals": ["lunch"],
  "showWeekends": false,
  "updatedAt": "2026-10-28T00:00:00.000Z"
}
```

Picks are keyed by `"u" + <household user id>`, nested under the
meal id. Dates are local `YYYY-MM-DD`. An option's `meals` is the
configurable set of meals it appears in (any non-empty subset of
`breakfast`, `snack`, `lunch`, `dinner`). Only enabled meals render
rows and tabs, and only enabled meals are written; a disabled meal's
saved picks stay in the document untouched. The widget polls storage
(default every 10 seconds, setting `pollSeconds`) so a choice made
on one display appears on the others; a refresh is skipped while any
dialog is open, and concurrent edits resolve last-write-wins.

**Migration:** documents saved before the week-shape change have no
`showWeekends` field and default to `false` (Monday–Friday), so
existing choices and options are untouched. Documents saved before
meals existed carry the snack-era shape instead — `serves` on options
(`"lunch"`, `"snack"`, or `"both"`), `choices` (lunch picks),
`snackChoices`, and `snackChoicesEnabled` — and migrate on load with
no pick loss: `serves "lunch"` becomes `["lunch"]`, `"snack"`
becomes `["snack"]`, `"both"` becomes `["lunch", "snack"]` (both
predates breakfast/dinner, so it never means all four), and options
with no field become `["lunch"]`; `choices` becomes `picks.lunch`
and `snackChoices` becomes `picks.snack`; `snackChoicesEnabled: true`
becomes `enabledMeals: ["lunch", "snack"]`, while absent or false
becomes `["lunch"]`. The next save persists the meals shape. The
plugin was renamed from `LunchChoice.html` (storage key
`lunch-choice`) to `MealChoice.html` (storage key `meal-choice`):
the document is copied across under the new key with every field
intact, so existing documents, options, and picks carry straight
over. The per-display filter key was renamed with it, so a display
that had filtered people shows everyone until it is re-filtered.
Documents saved before the household-users change carry a legacy
`people` array and choices keyed by custom person ids; those are
preserved in the document untouched on save but are no longer
rendered — the meal set is now `selectedUserIds`, and picks render
only for selected household users. The per-display people filter is
not in this document at all — it lives in the display's localStorage
shim under `homeglowMealChoice.displayPeople` (an array of
`"u" + <user id>` strings; missing or empty means everyone selected).

## What it doesn't do

No prices, no reminders or due dates (those belong to chores), no
syncing with other apps, and no connection to any other plugin's data.
Plugin storage is isolated per plugin, so this widget cannot read or
share another plugin's notes — including a whiteboard plugin's daily
notes. If a household wants the meal choice written somewhere else,
that is a separate integration, not part of this widget.
