# Family Lists — user guide

One widget for all the household's shared lists. It lives on the
dashboard like any other widget: install it, add it to a tab, and every
display in the house sees the same lists.

## List types

Create a list from one of two templates. The template controls which
fields the add form shows:

- **Grocery** — quantities, units, store sections, and freeform store
  tags. This is the default first list.
- **Custom** — freeform checklist with an optional category.

Switch lists with the tabs at the top of the widget.

## Grocery sections

A grocery item belongs to one store section: Produce, Dairy, Meat &
Seafood, Bakery, Pantry, Frozen, Household, or Other. When you add an
item without picking a section, the widget guesses from the name
("chicken thighs" → Meat & Seafood, "oat milk" → Dairy); anything it
cannot place lands in Other. Guessing prefers multi-word phrases
("ice cream" → Frozen), then the head noun of the phrase, so
"avocado oil" lands in Pantry and "apple crisps" in Pantry rather
than Produce. A wrong guess is fixed by hand with the
section picker in the item's edit dialog.

- **Group by section** (Manage view, grocery lists only): an optional
  per-list toggle. When it is on, items render under their section
  headers, in store-walk order; when it is off, the list shows flat
  in its current order. The toggle is saved with the list.
- **Entry order** is the default sort: items show in the order they
  were added, or the order drag-reorder left them in.
  A–Z and Recently added are also available; when grouping is on,
  they apply within each section.
- **Drag to reorder** (Manage view): grab the ⋮⋮ handle on a row to
  move it. Dropping within a section reorders within it; dropping on
  a row in another section (grouped view only) moves the item into
  that section. Drops in a flat list reorder only. Dragging is
  undoable. (Touch displays do not support HTML5 drag; use the
  section picker in the edit dialog to re-section an item there.)

Custom lists have no sections: they sort
by entry order, A–Z, or Recently added.

## Adding and managing items

These actions live in Manage view; Simple view offers check-off and
quick-add only (see Simple view and Manage view below).

- **Add:** in Manage view, tap + Add item to open the add dialog, type in the name
  box, and press Add item. Quantity parsing works from the name
  alone: `2x milk`, `2 lbs chicken`, `2 cups of flour` all split
  into quantity, unit, and name. Fill in Qty / Unit / Section /
  Store explicitly when you want finer control (grocery), or
  Category (custom). Cancel, Esc, or tapping outside the dialog
  discards the draft. In Simple view, tap + Add for the minimal
  quick-add dialog (name box with the same quantity parsing, plus a
  quantity field only); grocery sections are guessed from the name.
- **Check / uncheck:** tap the checkbox. Checking the last unchecked
  item celebrates the finished list (if celebrations are on).
- **Undo:** after checking, unchecking, deleting checked items,
  removing, adding, editing, renaming, deleting a list, organizing,
  or drag-reordering, an Undo bar appears for a few seconds — one
  tap restores the list to before that change (last change only,
  same last-write-wins rule as any other edit).
- **Uncheck all:** unchecks every checked item in the current list
  without deleting anything. Use it when a batch of items got checked
  by mistake.
- **Edit:** tap the pencil to open the same dialog prefilled with
  the item, change it, and Save changes (or Cancel). The section
  picker shows the section the item currently renders under.
  Switching tabs cancels an in-progress edit.
- **Rename list:** tap ✎ Rename list, edit the name in place, and
  press Enter (or Save name) to save, Esc (or Cancel) to keep the old
  name. Only the name changes — the list keeps its identity, so its
  items, filters, and history are untouched. Empty names are ignored.
- **Remove:** tap ✕ twice — the first tap asks for confirmation, the
  second removes the item.
- **Delete checked items:** two-tap confirm, permanently deletes every
  checked item. It does not uncheck — for that, use Uncheck all. Undo
  covers a mistaken delete.
- **Delete list:** two-tap confirm. You always retain at least one list;
  deleting the only list just tells you so.
- **Bulk add:** inside the add dialog — one item per line, same
  quantity parsing as single add; grocery sections are guessed.
- **Copy list:** copies the unchecked items as plain text, handy for
  pasting into a message. If copying fails (some displays block
  clipboard access from widgets), the button says so instead of
  claiming success.

## Finding things

These controls live in Manage view; Simple view has none of them.

- **Search** covers item names, categories, and stores.
- **Sort** by entry order (the default), A–Z, or recently added.
- **Store filter** lists only the stores already used on that list.
  Stores are freeform tags you type yourself — there is no preset
  retailer list, and a store typed once becomes
  filterable and suggested from then on.

## Simple view and Manage view

The widget has two views. The dashboard widget is always Simple;
Manage is a separate view reached from the widget's settings gear.

- **Simple view** (the dashboard widget): a title row — "Family Lists"
  on the left, + Add and a settings gear upper right — with the list
  tabs on their own row below, and condensed
  one-line rows
  (quantity + name, plus a small store pill when a store is set).
  The header title is always plain "Family Lists" — the 🛒 cart emoji
  appears after the grocery list's own tab name instead ("Groceries 🛒"),
  never in the header, and no tab carries a type suffix: grocery tabs
  read "<Name> 🛒", custom tabs read just "<Name>". A new grocery
  list defaults to the name "Grocery" (editable in the dialog, and
  existing lists are never renamed).
  Simple shows neither a progress bar nor stats at any width —
  those live in Manage view only.
  Checkboxes work here — tap one to check or uncheck, with the usual
  Undo bar. Checked items stay visible, struck through. There is no
  search, sort, store filter, or any delete/rename/new-list control
  in Simple: it is for using the list, not managing it. + Add opens
  a minimal quick-add dialog: name box (quantity parsing still
  applies, so `2 lbs chicken` splits quantity + unit) plus a
  quantity field only. New grocery items get their section guessed
  from the name. Tapping a row name does nothing — there is no edit
  on the wall. When Group by section is on, Simple shows the section
  headers too, compact.
- **Manage view**: the full UI described above — all header controls,
  the full add/edit dialog (every field per template), bulk add,
  the Group-by-section toggle, drag handles, rename, copy,
  sort, filters, and delete flows — plus a progress bar and stats
  (total item count and checked count). The header button becomes
  a back chevron (←) that returns to Simple.

Tap the settings gear (upper right of the widget) to open Manage
view; in Manage view that same header button is a back chevron (←)
that returns to Simple. A `?view=manage` widget URL opens Manage for that load, and
a `?view=simple` URL param forces Simple the same way. Every fresh
load shows Simple unless a view param is present — no view is
remembered, so a stale saved preference can never flip the
dashboard to Manage.

The widget also reflows with its dashboard size: narrow widgets get
compact rows, wide widgets get roomier rows and larger touch
targets, so the same widget reads well at any dashboard size.

## Settings (household)

- **How often to check for changes (seconds)** — default 10, minimum 5.

## How syncing works

The widget polls its stored document every few seconds, so a change
made on another display appears without reloading. Polling never
clobbers an edit you are typing. If two displays edit at the same
time, the last write wins — there is no merge, so avoid editing the
same list on two screens at once.

## Storage contract (for tinkerers and sync authors)

The widget uses the HomeGlow plugin storage API, key `lists`:

```json
{
  "version": 1,
  "lists": [
    {
      "id": "…", "name": "Groceries", "type": "grocery",
      "groupSections": false,
      "items": [
        {
          "id": "…", "name": "Example item", "qty": 1, "unit": "",
          "category": "Produce",
          "store": "",
          "checked": false, "checkedAt": null,
          "createdAt": "…", "source": "manual"
        }
      ]
    }
  ],
  "updatedAt": "…"
}
```

Item `source` is a short tag naming where the item came from (for example `manual` or `import`).
Anything writing to this key must use this same document shape — never a
private format — and must set `updatedAt` when it writes. The widget
never auto-saves a default document over a failed storage read: if
storage is unreachable at boot, the default list stays in memory
only until storage recovers. Unknown fields on read (from older
versions or other writers) are ignored, and a list whose template no
longer exists renders as a custom list.

## Installing

- **From the store (once merged upstream):** Admin Panel → Browse and
  contribute plugins → install Family Lists, then add it to a tab.
- **Manually:** upload `FamilyLists.html` via the Admin Panel's widget
  upload. The widget needs no server changes and declares
  `"storage": true` in its manifest, which is what allows it to save
  lists.
