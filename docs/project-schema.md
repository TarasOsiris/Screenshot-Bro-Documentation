# Project File Schema

Screenshot Bro stores each project as a single `project.json` file inside `~/Library/Application Support/screenshot/projects/<uuid>/`. The format is a well-defined JSON structure derived directly from the app's Swift `Codable` models.

The full JSON Schema (Draft 2020-12) is published at:

```
https://screenshotbro.app/project-schema.json
```

## Using the schema with AI

You can give the schema URL to any AI assistant that can fetch URLs (Claude, ChatGPT, Gemini, etc.) and ask it to generate or modify project files. This is useful for:

- Generating a complete project from a description
- Batch-creating projects for multiple languages or products
- Transforming an existing project (adding rows, changing colors)
- Validating a hand-crafted project file before importing

### Example prompt — generate a project

```
Fetch the JSON Schema at https://screenshotbro.app/project-schema.json and use it to generate a valid project.json for Screenshot Bro.

Requirements:
- Two rows: one for iPhone 17 (1290×2796) and one for iPad Pro 13" (2064×2752)
- Three templates per row (three color variants)
- Each template has a text shape with the headline "Focus on what matters" centered at the top
- Each template has a device shape showing an iPhone 17 / iPad Pro frame
- Background: a blue-to-purple linear gradient

Return only the raw JSON with no explanation.
```

### Example prompt — edit a project

```
Here is my Screenshot Bro project.json (attached). The schema is at https://screenshotbro.app/project-schema.json.

Change all text shapes whose `txt` field contains "Download now" to "Try it free".
Keep everything else unchanged.
Return only the updated JSON.
```

### Using the schema in Claude Projects or a system prompt

For repeated use, add the schema to a Claude Project or paste it into a system prompt so the AI always has it available:

```
You are a Screenshot Bro project file generator. The project file format is
defined by this JSON Schema:

[paste contents of https://screenshotbro.app/project-schema.json here]

When the user describes a set of screenshots, produce a valid project.json.
Use UUIDs for all `id` fields. Use Swift reference-date timestamps
(seconds since 2001-01-01) for the `m` field.
```

## File structure overview

A project file has three top-level fields:

| Key | Type | Description |
|-----|------|-------------|
| `r` | array | Ordered list of screenshot rows |
| `ls` | object | Locale state (locales, active locale, per-shape overrides) |
| `m` | number | Last-modified timestamp (seconds since 2001-01-01) |

Each **row** (`r[]`) defines a group of templates at a fixed canvas size (e.g. iPhone 17 = 1290×2796). A row contains:

- `tp` — templates (columns), one per color/variant, each with its own background override
- `s` — shapes that span across all templates, positioned by x-coordinate

**Shapes** (`s[]`) are typed by the `t` field: `text`, `device`, `image`, `rectangle`, `circle`, `star`, or `svg`.

## Key conventions

- **Short keys.** All JSON field names are abbreviated (e.g. `w` = width, `fs` = fontSize, `bgc` = backgroundColor). The schema description for each field names the full Swift property.
- **Colors.** Encoded as hex strings: `#RRGGBB` (opaque) or `#RRGGBBAA` (with alpha).
- **Omitted defaults.** Many fields are optional and omitted when they equal the default (e.g. `opacity` is omitted when 1.0, `rotation` when 0).
- **UUIDs.** Every `id` field must be a standard UUID string (e.g. `550e8400-e29b-41d4-a716-446655440000`).
- **Coordinates.** All x/y/w/h values are in model-space points at the template's native resolution. Shapes that span multiple templates use the combined canvas width (`templateWidth × templateCount`).

## Minimal example

```json
{
  "r": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "l": "iPhone 17",
      "tw": 1290,
      "th": 2796,
      "bgc": "#FFFFFF",
      "ddc": "iphone",
      "tp": [
        { "id": "550e8400-e29b-41d4-a716-446655440002", "bgc": "#FFFFFF" }
      ],
      "s": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440003",
          "t": "text",
          "x": 100, "y": 150, "w": 1090, "h": 200,
          "c": "#1A1A1A",
          "txt": "Your App Headline",
          "fs": 72,
          "fw": 700,
          "ta": "center"
        }
      ]
    }
  ]
}
```

## Validation

You can validate a project file against the schema using any JSON Schema validator. With the [ajv](https://github.com/ajv-validator/ajv) CLI:

```bash
npx ajv validate -s https://screenshotbro.app/project-schema.json -d project.json --spec=draft2020
```

Or in Python with [jsonschema](https://github.com/python-jsonschema/jsonschema):

```python
import json, urllib.request, jsonschema

schema = json.loads(urllib.request.urlopen("https://screenshotbro.app/project-schema.json").read())
project = json.load(open("project.json"))
jsonschema.validate(project, schema)
```
