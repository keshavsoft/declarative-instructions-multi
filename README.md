# declarative-instructions-multi

> Coordinated table features, applied as ordered instruction groups.

[![npm](https://img.shields.io/npm/v/declarative-instructions-multi?color=cb3837&label=npm)](https://www.npmjs.com/package/declarative-instructions-multi)
[![Documentation](https://img.shields.io/badge/docs-follow%20the%20table-a78bfa)](https://keshavsoft.github.io/declarative-instructions-multi/)
![License: MIT](https://img.shields.io/badge/license-MIT-34d399.svg)

## The table story

The tested V7 table sample in [`json-renderers-run-instructions-steps`](https://github.com/keshavsoft/json-renderers-run-instructions-steps/tree/main/samples/v7/table) combines this package with `declarative-instructions-single`. Single references build the table sections and toolbar. Multi references add features that touch several table sections together.

It begins with a cloned `spec.tableWithFooter` shell and three column definitions plus three sample records. The table structure is in place before the multi groups add the serial and action columns.

Each `{ type: "multi", key }` selects an ordered array from this package. The step runner applies every item in that array to the same cloned `tableWithFooter` tree before continuing to the next reference.

## The three groups used by the sample

| Multi key | Coordinated table changes |
| --- | --- |
| `set-serial` | Prepends the serial width, header cell, and footer cell. The sample follows it with the single `serial-show-serial` step to add serial cells to the body rows. |
| `options-show-button` | Adds the options width, header, one button cell per body row, and footer cell. |
| `options-show-edit-button` | Adds the edit width, header, one edit cell per body row, and footer cell. |

The sample has twelve references in total: nine single steps and these three multi groups. The groups expand into eleven coordinated insertions; together with the single steps, they assemble the complete table report.

## A group in context

The `set-serial` group handles the parts that share the same feature and destination order:

```js
const tableSteps = [
  { type: "multi", key: "set-serial" },
  { type: "single", key: "serial-show-serial" },
  { type: "multi", key: "options-show-button" },
  { type: "multi", key: "options-show-edit-button" }
];
```

`set-serial` adds the column width and the header and footer cells. The adjacent single step adds the serial value to each existing body row. Each options group then adds a full action column across the table.

## The full tested sequence

The sample first builds the table’s column group, header, body, and footer from `columns` and `data`. It then adds the search toolbar, serial feature, and action columns:

```js
import runInstructions from "json-renderers-run-instructions-steps";
import spec from "declarative-spec";

const base = structuredClone(spec.tableWithFooter);

const result = runInstructions({
  instructions: [
    { type: "single", key: "buildColgroup" },
    { type: "single", key: "buildHeader" },
    { type: "single", key: "buildBody" },
    { type: "single", key: "buildFooter" },
    { type: "single", key: "set-search" },
    { type: "single", key: "search-show-label" },
    { type: "single", key: "search-show-helper" },
    { type: "single", key: "search-show-input" },
    { type: "multi", key: "set-serial" },
    { type: "single", key: "serial-show-serial" },
    { type: "multi", key: "options-show-button" },
    { type: "multi", key: "options-show-edit-button" }
  ],
  base,
  data: { columns, data }
});
```

The runner applies each reference in sequence to the caller-provided canvas and returns the completed table AST. The sample’s final step hands that AST to `json-to-dom` for browser rendering.

## Package roles

- **`declarative-instructions-single`** defines one operation per key: build a table section, fill a search slot, or add serial values to body rows.
- **`declarative-instructions-multi`** groups the related changes that make a serial or action column complete.
- **`json-renderers-run-instructions-steps`** resolves `{ type, key }`, executes the selected operations, and mutates the cloned table tree.
- **`declarative-spec`** supplies the empty table shell and the templates referenced by the instructions.

## Install

For the integrated table flow, install the runner and spec package; the runner depends on both instruction packages:

```bash
npm install json-renderers-run-instructions-steps declarative-spec
```

Current multi-step table instructions: [`src/v3/instructions.json`](src/v3/instructions.json) · [Searchable table story](docs/index.html)

## License

MIT © KeshavSoft.
