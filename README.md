# QuikSim3 Examples

A curated library of example programs for [QuikSim3](https://github.com/DSI-JBenway/QuikSim3),
Dynamic Systems Inc.'s Gleeble simulation client.

These examples are distributed as a separate repository so new examples can
be added, corrected, and tagged more frequently than QuikSim3 itself.
QuikSim3 fetches the latest release from this repo on startup and makes the
examples available read-only from the **Examples** panel — users save their
own copies into a project via **File → Save As**.

## Repository layout

```
QuikSim3-Examples/
├── README.md              # this file
├── manifest.json          # generated index of all examples (see tools/)
├── examples/
│   ├── <example-id>/
│   │   ├── meta.json      # user-facing metadata (name, category, tags, …)
│   │   ├── program.qhd    # the program file (.qhd / .qst / .qhz)
│   │   └── README.md      # description + key parameters + how to use
│   └── …
├── tools/
│   └── generate_manifest.py   # scans examples/ and writes manifest.json
└── .github/workflows/
    └── release.yml        # packages examples for a GitHub Release on tag push
```

## Adding a new example

1. Pick an `example-id` (kebab-case, short).
2. Create `examples/<example-id>/` and drop in:
   - the program file, named `program.qhd`, `program.qst`, or `program.qhz`
     depending on the document type;
   - a `README.md` describing what it does and how to use it;
   - a `meta.json` with the fields described below.
3. Regenerate the manifest: `python tools/generate_manifest.py`.
4. Commit everything and open a PR.

### `meta.json` schema

```json
{
  "id": "tensile-basic",
  "name": "Basic Tensile Test",
  "category": "Tensile",
  "description": "A short user-facing description.",
  "tags": ["tensile", "basic"],
  "difficulty": "beginner",
  "program": "program.qhd",
  "min_quiksim3": "3.0.0"
}
```

- `min_quiksim3` is the oldest QuikSim3 version that can open the program.
  Bump it whenever an example relies on a newer `schemaVersion`. The QuikSim3
  client filters the manifest by this field so stale installs only see
  examples they can actually load.

## Releases

Tagged commits (`v*`) trigger `.github/workflows/release.yml`, which:

1. Regenerates `manifest.json` so the release is self-consistent.
2. Packs the `examples/` tree and `manifest.json` into `examples.zip`.
3. Uploads `examples.zip` and `manifest.json` as assets on a new GitHub
   Release with the same tag.

QuikSim3 fetches
`https://github.com/DSI-JBenway/QuikSim3-Examples/releases/latest/download/examples.zip`
on startup and unpacks it under `%LOCALAPPDATA%\QuikSim3\examples\<tag>\`.

To cut a new release:

```
git tag v0.2.0
git push origin v0.2.0
```

## License

These examples are provided as-is for educational use. See individual example
READMEs for any specific attribution.
