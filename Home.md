### Welcome to the Slickgrid-Universal lib!

To get started follow any of these instruction Wikis depending on your choice of Framework.

| Framework | Wiki |
| --------- | ---- |
| Angular | [Wiki - HOWTO (Step by Step)](https://github.com/ghiscoding/angular-slickgrid/wiki/HOWTO---Step-by-Step) |
| Aurelia | [Wiki - HOWTO (Step by Step)](https://github.com/ghiscoding/aurelia-slickgrid/wiki/HOWTO--Step-by-Step) |
| Salesforce | [Installation](/ghiscoding/slickgrid-universal/wiki/Installation---Salesforce-(LWC)) |

### TODO
#### Code
##### Legend: (E) must be extended in the external framework
- [x] Aggregators (4)
- [x] Editors (11)
- [x] Filters (17)
  - [x] Autocomplete, Single & Multiple Select
    - (E) each framework must implement the `collectionAsync` 
- [x] Formatters (31)
- [ ] Extensions
  - [x] AutoTooltip
  - [x] Cell External Copy Manager
  - [x] Cell Menu
  - [x] Checkbox Selector
  - [x] Context Menu
  - [x] Draggable Grouping
  - [x] Grid Menu
  - [x] Header Button
  - [x] Header Menu
  - [ ] (E) Row Detail
  - [x] Row Move
  - [x] Row Selection
- [x] Grouping Formatters (12)
- [x] Sorters (5)
- [x] Services
  - [x] Collection
  - [x] Excel Export (**separate package**)
  - [x] Export Text (**separate package**)
  - [x] Extension
  - [x] Filter
  - [x] GraphQL (**separate package**)
  - [x] OData (**separate package**)
  - [x] Grid Event
  - [x] Grid Service (helper)
  - [x] Grid State
  - [x] Grouping & Col Span
  - [x] Pagination
  - [x] Resizer
    - [x] moved the Service to an Extension
  - [x] Shared
  - [x] Sort
- [ ] Others
  - [x] Custom Footer
  - [ ] Dynamically Add Columns
  - [x] Grid Presets
  - [x] Local Pagination
  - [x] Tree View
    - [x] Search Filter on any Column
    - [x] Sorting from any Column

#### Other Todos
- [x] VScode Chrome Debugger
- [x] Jest Debugger
- [x] Add Multiple Example Demos with Vanilla implementation
  - [x] Add GitHub Demo website
- [x] Add CI/CD (CircleCI or GitHub Actions)
  - [x] Add Cypress E2E tests
  - [x] Add Jest Unit tests
  - [x] Add Jest Code Coverage (codecov)
  - [x] Build and run on every PR
  - [x] Add full bundler (all types) build step in CircleCI build
- [x] Bundle Creation (vanilla bundle)
  - [ ] Eventually add Unit Tests as a Pre-Bundle task
- [x] Remove any Deprecated code
  - [ ] Create a [Migration Guide](https://github.com/ghiscoding/slickgrid-universal/wiki/Migration-for-Angular-Aurelia-Slickgrid) for Angular/Aurelia
- [x] Add simple input bindings in the demo (e.g. pinned rows input)
- [x] Add possibility to use SVG instead of Font Family
- [x] Add Typings (interfaces) for Slick Grid & DataView objects
  - [x] Add interfaces to all SlickGrid core lib classes & plugins (basically add Types to everything)
- [x] Copy text from cell doesn't work in SF
- [ ] Remove all Services init method 2nd argument (we can get DataView directly from the Grid object)
  - [x] Build and run on every PR
- [x] Bundle Creation (vanilla bundle)
  - [ ] Eventually add Unit Tests as a Pre-Bundle task
- [x] Remove any Deprecated code
  - [ ] Create a [Migration Guide](https://github.com/ghiscoding/slickgrid-universal/wiki/Migration-for-Angular-Aurelia-Slickgrid) for Angular/Aurelia
- [x] Add simple input bindings in the demo (e.g. pinned rows input)
- [x] Add possibility to use SVG instead of Font Family
- [x] Add Typings for Grid & DataView objects
- [x] Cannot copy text from cell since it's not selectable
- [ ] Remove all Service init 2nd argument (we can get DataView from the Grid object)
