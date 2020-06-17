Welcome to the slickgrid-universal wiki!

## TODO
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
- [ ] Services
  - [x] Collection
  - [x] Excel Export (**separate package**)
  - [x] Export Text (**separate package**)
  - [x] Extension
  - [x] Filter
  - [ ] GraphQL (**separate package**)
  - [ ] OData (**separate package**)
  - [x] Grid Event
  - [x] Grid State
  - [x] Grouping & Col Span
  - [x] Pagination
  - [ ] Resizer 
    - moved the Service to an Extension
  - [x] Shared
  - [x] Sort
- [ ] Others
  - [x] Custom Footer
  - [ ] Dynamically Add Columns
  - [ ] Grid Presets
  - [ ] Local Pagination
  - [ ] Tree View
    - [ ] Search Filter on any Column
    - [ ] Sorting from any Column

#### Other Todos
- [x] VScode Chrome Debugger
- [x] Jest Debugger
- [x] Add Multiple Example Demos with Vanilla implementation
  - [x] Add GitHub Demo website
- [x] Add CI/CD (CircleCI or GitHub Actions)
  - [x] Add Jest Unit tests
  - [ ] Add Cypress E2E tests
  - [x] Add Code Coverage (codecov)
  - [x] Build and run on every PR
- [x] Bundle Creation (vanilla bundle)
  - [ ] Eventually add Unit Tests as a Pre-Bundle task
- [ ] Remove any Deprecated code
  - [ ] Create a [Migration Guide](https://github.com/ghiscoding/slickgrid-universal/wiki/Migration-for-Angular-Aurelia-Slickgrid) for Angular/Aurelia
- [x] Add simple input bindings in the demo (e.g. pinned rows input)
- [x] Add possibility to use SVG instead of Font Family
- [x] Add Typings for Grid & DataView objects
- [ ] Cannot copy text from cell since it's not selectable
- [ ] Remove all Service init 2nd argument (we can get DataView from the Grid object)
