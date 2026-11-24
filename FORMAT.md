# Pouches export format

Export Trips saves every trip in one UTF-8 JSON file, and Import Trips reads it back.

```json
{
  "version": 1,
  "trips": [
    {
      "id": "6F1C2B0E-4A7D-4E55-9C1B-2D3E4F5A6B7C",
      "name": "NYC",
      "start": "2026-06-04",
      "end": "2026-06-07",
      "sections": ["Worn", "Carry-on", "Personal Item"],
      "pinned": false,
      "items": [
        { "name": "Socks", "amount": 3, "section": "Carry-on", "packed": true, "unused": false }
      ]
    }
  ]
}
```

## Trip

| Key | Type | Default |
|---|---|---|
| `id` | UUID | new |
| `name` | string | required |
| `start`, `end` | `"YYYY-MM-DD"` | required |
| `sections` | [string] | required |
| `pinned` | bool | `false` |
| `items` | [Item] | required |

## Item

| Key | Type | Default |
|---|---|---|
| `id` | UUID | new |
| `name` | string | required |
| `amount` | integer ≥ 1 | `1` |
| `section` | string | `"Other"` |
| `packed` | bool | `false` |
| `unused` | bool | `false` |
| `unusedStreak` | integer ≥ 0 | `0` |

`unused` is marked Didn't Use. `unusedStreak` is how many trips in a row it went unused.

## Importing

- A trip already in Pouches is skipped: same `id`, or for trips without one, same `name` and `start`.
- Unknown keys are ignored.
