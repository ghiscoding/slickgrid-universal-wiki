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
  - [ ] Excel Export (**separate package**)
  - [ ] Export Text (**separate package**)
  - [x] Extension
  - [x] Filter
  - [ ] GraphQL (**separate package**)
  - [ ] OData (**separate package**)
  - [ ] Grid Event
  - [ ] Grid State
  - [x] Grouping & Col Span
  - [ ] Pagination
  - [ ] Resizer 
    - moved the Service to an Extension
  - [x] Shared
  - [x] Sort
- [ ] Others
  - [ ] Custom Footer
  - [ ] Dynamically Add Columns
  - [ ] Grid Presets
  - [ ] Pagination
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
  - [ ] Eventually add Unit Tests as a PreBundle task
- [ ] Remove any Deprecated code
  - [ ] Create a Migration Guide 
- [ ] Add Typings for Grid & DataView objects
- [ ] Add some bindings in the demo (e.g. pinned rows input)
- [ ] Can we change how SlickGrid Events are called and use same signature as SlickGrid instead of CustomEvent EventData signature?
