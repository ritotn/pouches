# The pouch format

A pouch is a JSON file of trips, ending in `.pouch`.

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

| Key | Type | Notes |
|---|---|---|
| `format` | string | required; `"pouch"` |
| `version` | integer | required; `1` |
| `trips` | array of trips | required |

## Trip

| Key | Type | Notes |
|---|---|---|
| `id` | string | required; UUID |
| `name` | string | required |
| `start`, `end` | string | required; `YYYY-MM-DD`, `end` ≥ `start` |
| `sections` | array of sections | required |

## Section

| Key | Type | Notes |
|---|---|---|
| `name` | string | required; unique in its trip, ignoring case |
| `items` | array of items | default `[]` |

## Item

| Key | Type | Notes |
|---|---|---|
| `id` | string | UUID, same across copies of a trip |
| `name` | string | required |
| `amount` | integer | ≥ 1, default `1` |
| `packed` | boolean | default `false` |
| `used` | boolean | missing: not reviewed |

Keys starting with `x-` hold app data, like `"x-pouches": { "pinned": true }`. Readers ignore unknown keys.
