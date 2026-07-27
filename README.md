# dist-revisorschemas

Public [revisor](https://github.com/ttab/revisor) schemas for the Elephant
content **distribution** service. They are the distribution-facing counterpart
to the internal repository schemas in
[`revisorschemas`](https://github.com/ttab/revisorschemas): the same document
model, but pruned to what is exposed to external consumers (for example
internal notes are dropped) and adapted to the distributed shape (image blocks
carry signed asset-CDN renditions rather than raw repository URLs).

## Schemas

| File | Name | Extends |
| --- | --- | --- |
| `se.ecms.dist.json` | `se.ecms.dist` | base content model |
| `se.ecms.dist.planning.json` | `se.ecms.dist.planning` | planning documents |
| `se.tt.dist.json` | `se.tt.dist` | TT customisations of `se.ecms.dist` |
| `se.tt.dist.planning.json` | `se.tt.dist.planning` | TT planning customisations |

The `se.tt.dist` set extends `se.ecms.dist`; both TT sets must be loaded
together with their `se.ecms.dist` bases. In particular `se.tt.dist` extends
the `core://image-types` enum with `tt/picture` and `tt/graphic`, so a
`core/image` block whose image link points at the TT image archive validates
only when `se.tt.dist` is loaded alongside `se.ecms.dist`.

## Usage

```go
import distrevisorschemas "github.com/ttab/dist-revisorschemas"

constraints, err := revisor.DecodeConstraintSetsFS(
	distrevisorschemas.Files(),
	"se.ecms.dist.json", "se.tt.dist.json",
)
```

`Files()` returns the embedded schema filesystem and `Version()` reports the
module version the schemas were built from.

## Tests

```bash
go test ./...
```

`testdata/valid-docs` must validate cleanly and `testdata/invalid-docs` must
produce the recorded errors (regenerate the golden error files with
`REGENERATE=true go test ./...`).
