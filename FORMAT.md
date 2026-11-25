# The pouch format

A pouch is a JSON file of trips. Files end in `.pouch`.

```json
{
  "format": "pouch",
  "version": 1,
  "trips": [
    {
      "id": "6F1C2B0E-4A7D-4E55-9C1B-2D3E4F5A6B7C",
      "name": "NYC",
      "start": "2026-06-04",
      "end": "2026-06-07",
      "sections": [
        {
          "name": "Carry-on",
          "items": [
            { "id": "522FBC2F-123B-429B-9186-3DEC475EA68C", "name": "Socks", "amount": 3, "packed": true, "used": true },
            { "id": "348EBB21-51C1-48BC-8B63-681D8EF0860E", "name": "Tripod", "packed": true, "used": false }
          ]
        },
        { "name": "Worn", "items": [] }
      ],
      "x-pouches": { "pinned": true }
    }
  ]
}
```

## File

| Key | Type | |
|---|---|---|
| `format` | `"pouch"` | required |
| `version` | number | required, `1` |
| `trips` | list of trips | required |

## Trip

| Key | Type | |
|---|---|---|
| `id` | UUID | required |
| `name` | text | required |
| `start`, `end` | date, `"2026-06-04"` | required; `end` is on or after `start` |
| `sections` | list of sections | required, in order |

## Section

| Key | Type | |
|---|---|---|
| `name` | text | required; no two in a trip share a name, ignoring case |
| `items` | list of items | in order; can be empty |

## Item

| Key | Type | Default | |
|---|---|---|---|
| `id` | UUID | new | |
| `name` | text | required | |
| `amount` | whole number, 1 or more | `1` | |
| `packed` | true / false | `false` | in the bag right now |
| `used` | true / false | missing | `false` didn't use, `true` used, missing: not reviewed |

Dates have no time or time zone. UUIDs can be upper- or lowercase.

An item keeps its `id` from trip to trip: copying a trip copies its items' ids and gives the trip a new one.

A trip is reviewed once any of its items has `used`.

## History

An item's history is every earlier reviewed trip that has it, most recent first. Its unused streak counts
back from the latest of those while it was unused, skipping trips where it has no `used`. History isn't stored.

## Extra keys

Data this format doesn't cover, like Pouches' pinned trips, goes in an object whose name starts with `x-`:
`"x-pouches": { "pinned": true }`. Unknown keys are ignored and kept when possible.

## Reading

- A trip with a missing name, a bad date, or `end` before `start` is skipped; the rest still read.
- A repeated trip `id` keeps the first. A repeated item `id` in a trip gets a new one.
- An `amount` below 1 reads as 1.
- Two sections that differ only in case are merged into the first.
- A `version` other than 1 isn't read.

Importing keeps ids and skips trips that are already there. Opening someone else's trip starts a new one
with new ids.

## Sharing

A shared pouch leaves out ids, exact dates, and `packed`, and gives each trip `month` (`"2026-06"`) and `days`
(its length) instead.

## Changes

New optional keys keep version 1. Anything that would make an older app misread a file gets a new version.
