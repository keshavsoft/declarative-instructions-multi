# declarative-instructions-multi

> **One feature, several ordered changes.** Declarative instruction bundles for assembling report features across a shared UI tree.

[![npm](https://img.shields.io/npm/v/declarative-instructions-multi?color=cb3837&label=npm)](https://www.npmjs.com/package/declarative-instructions-multi)
[![Documentation](https://img.shields.io/badge/docs-read%20the%20story-a78bfa)](https://keshavsoft.github.io/declarative-instructions-multi/)
![License: MIT](https://img.shields.io/badge/license-MIT-34d399.svg)

## The story

The original `declarative-instructions` report was split into two complementary reports. This package tells the **multi-step story**: a report feature often touches several places in the same tree. Adding a serial column, for example, can require a width, a header cell, body cells, and a footer cell.

Instead of scattering those changes across unrelated keys, each feature is a named array of steps. The renderer can apply the steps in order, while the feature key keeps the whole change easy to discover and enable as a unit.

## A feature bundle

The complete `serial` feature coordinates four destinations:

```json
{
  "serial": [
    {
      "narration": "Prepend the serial column width to the table colgroup.",
      "destinationPath": {
        "fileName": "base",
        "filePath": ["children"],
        "action": "unshift",
        "jsonPath": "$..*[?@.attributes['data-role'] == 'table-colgroup']"
      },
      "sourcePath": null,
      "templateKey": "colGroupForSerial",
      "func": null
    },
    {
      "narration": "Prepend the serial header cell to the table header row.",
      "destinationPath": {
        "fileName": "base",
        "filePath": ["children"],
        "action": "unshift",
        "jsonPath": "$..*[?@.attributes['data-role'] == 'table-header-row']"
      },
      "sourcePath": null,
      "templateKey": "headerSerialCell",
      "func": null
    },
    {
      "narration": "Prepend the serial number cell into each table body row.",
      "destinationPath": {
        "fileName": "base",
        "filePath": ["children"],
        "action": "unshiftSerialToRows",
        "jsonPath": "$..*[?@.attributes['data-role'] == 'table-body']"
      },
      "sourcePath": null,
      "templateKey": "buildBodyCell",
      "func": null
    },
    {
      "narration": "Prepend the serial footer cell to the table footer row.",
      "destinationPath": {
        "fileName": "base",
        "filePath": ["children"],
        "action": "unshift",
        "jsonPath": "$..*[?@.attributes['data-role'] == 'table-footer-row']"
      },
      "sourcePath": null,
      "templateKey": "headerSerialCell",
      "func": null
    }
  ]
}
```

Each step uses the same four-part instruction vocabulary as the single report, with an `action` describing how the targeted children are changed.

## Included feature groups

| Feature key | Ordered work |
| --- | --- |
| `set-serial` | Add serial width, header, and footer cells. |
| `serial` | Add serial width, header, body, and footer cells. |
| `search` | Insert the search row before the table. |
| `options-show-button` | Add an options width, header, row buttons, and footer cell. |
| `options-show-edit-button` | Add an edit width, header, row buttons, and footer cell. |
| `options-show-delete-button` | Add a delete width, header, row buttons, and footer cell. |

The v3 report contains six feature groups and twenty transformation steps. `set-serial` is the shorter serial variant; `serial` includes the body-row insertion as well.

## Install and use

```bash
npm install declarative-instructions-multi
```

```js
import instructions from "declarative-instructions-multi";

const serialSteps = instructions.serial;
for (const step of serialSteps) {
  console.log(step.destinationPath.jsonPath, step.templateKey);
}
```

This package supplies declarative contracts, not the DOM renderer. Apply the groups with a compatible instruction runner and templates from `declarative-spec`. When composing a report, apply a feature group once; its steps insert the requested pieces into their target locations.

## Versions and source

- Current instructions: [`src/v3/instructions.json`](src/v3/instructions.json)
- Versioned JSON reference: [`docs/json/v3/instructions.json`](docs/json/v3/instructions.json)
- Documentation story and searchable catalog: [`docs/index.html`](docs/index.html)
- Related single-operation report: [declarative-instructions-single](https://github.com/keshavsoft/declarative-instructions-single)

## License

MIT © KeshavSoft.
