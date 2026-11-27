# declarative-instructions-multi

Ordered instruction groups for features that update several parts of a JSON UI specification together.

## The story

A table feature such as a serial or action column is made of related edits: a width, a header cell, body-row cells, and sometimes a footer cell. This package keeps each feature’s edits together as one named group. A consumer can select the group by key and apply its steps in order.

The V4 catalog contains six groups:

| Key | What the group assembles |
| --- | --- |
| <code>options-serial</code> | Serial width, header, numbered body cells, and footer cell. |
| <code>options-options-showButton</code> | Options width, header, row buttons, and footer cell. |
| <code>options-options-showEditButton</code> | Edit width, header, row buttons, and footer cell. |
| <code>options-options-showDeleteButton</code> | Delete width, header, row buttons, and footer cell. |
| <code>card-with-footer</code> | Card header, body, and footer content from data. |
| <code>table-search-toolbar</code> | Search toolbar, label, helper, and input. |

A group is a JSON array, not an executor. Each entry describes one write or transformation; the step runner supplies the current state and applies each entry sequentially.

## Inspect a group

~~~~js
import multiInstructions from "declarative-instructions-multi";

const serialColumnSteps = multiInstructions["options-serial"];
console.log(serialColumnSteps);
~~~~

A step runner can reference a group with a descriptor such as:

~~~~js
{ type: "multi", key: "options-serial" }
~~~~

The group’s destinations and templates are defined in the instruction data. The consumer provides the base specification and input data.

## Source and documentation

- [V4 multi-step catalog](src/v4/instructions.json)
- [Documentation site source](docs/index.html)
- [Package entry point](src/index.js)

## Install

~~~~bash
npm install declarative-instructions-multi
~~~~

## License

MIT © KeshavSoft.

