# Published pages

Everything in this folder is served straight to the in-game **Page Downloader**
through raw.githubusercontent.com. Nothing here is compiled into the mod.

## Adding a page

1. Create `<Your Page Name>.json` in this folder. Spaces are fine - the id in the
   manifest and the file name must match.
2. Add an entry to `pages-manifest.json` in the repository root:

   ```json
   { "id": "Your Page Name", "label": "Your Page Name", "kind": "external",
     "description": "One line shown under the title in the store.",
     "url": "https://raw.githubusercontent.com/Spankypluh/SpankyVRC/refs/heads/main/pages/Your%20Page%20Name.json" }
   ```

   If you leave `url` out, the downloader builds it from the id, which means the
   file has to be called exactly `<id>.json` here.
3. Commit and push. The store picks it up on the next **Refresh from GitHub**.

Entries with `"kind": "builtin"` are pages already compiled into SpankyVRC - they
only get enabled or disabled, never downloaded.

## Document format

```json
{
  "label": "Shown as the tile name and page title",
  "description": "Shown under the title.",
  "rows": [ ... ]
}
```

A page with no non-empty `rows` array is rejected by the downloader.

### Row types

| type | fields | notes |
| --- | --- | --- |
| `section` | `text` | Large heading. |
| `note` | `text`, `color` | Small muted line, `color` is a hex value like `#9AA5B1`. |
| `description` | `text`, `description`, `fontSize` | Main line plus a smaller line under it. |
| `text` | `text`, `fontSize`, `color` | Plain line. Any unknown type falls back to this. |
| `skip` | | Vertical gap. |
| `button` | `text`, `gradient`, `action` | `gradient`: `green`, `red`, `cyan`, `blue`, `yellow`, `gray`, or omit for default. |
| `toggle` | `text`, `key`, `description`, `gradient` | State is saved per page. |
| `types` | `text`, `key`, `values` | Left/right cycler over a list of strings. |

### Button actions

```json
"action": { "kind": "url",  "value": "https://example.com" }
"action": { "kind": "page", "value": "Settings" }
"action": { "kind": "home", "value": "home" }
```

`value` must always be present or the action is ignored. `page` accepts a config
name (`Room History`), a home label, or a page class name.

### Limits

200 rows per page, 500 characters per text field.
