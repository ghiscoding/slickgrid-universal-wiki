SlickGrid has a nice amount of [Grid Events](https://github.com/6pac/SlickGrid/wiki/Grid-Events) or [DataView Events](https://github.com/6pac/SlickGrid/wiki/Dataview-Events) which you can use by simply hook a `subscribe` to them (the `subscribe` are a custom `SlickGrid Event` and are **NOT** an `RxJS Observable` type but they very similar). You can access them in Slickgrid-Universal by following the documentation below

##### View
```html
<div class="grid1">
</div>
```

##### View - SalesForce (ES6)
For SalesForce it's nearly the same, the only difference is that we add our events in the View

```html
<div class="grid-container slds-p-horizontal">
    <div class="grid1" 
            onvalidationerror={handleOnValidationError} 
            oncellchange={handleOnCellChange}
            onclick={handleOnCellClick} 
            onbeforeeditcell={handleOnBeforeEditVerifyCellIsEditable}
            onslickergridcreated={handleOnSlickerGridCreated}>
    </div>
</div>
```

##### ViewModel
Hook yourself to the Changed event of the bindable grid object.

```javascript
export class GridExample {
  slickerGridInstance;
  gridObj: any;
  dataViewObj: any;

  attached() {
    const dataset = this.initializeGrid();
    const gridContainerElm = document.querySelector<HTMLDivElement>(`.grid4`);

    gridContainerElm.addEventListener('oncellclicked', this.handleOnCellClicked.bind(this));
    gridContainerElm.addEventListener('onslickergridcreated', this.handleOnSlickerGridCreated.bind(this));
    this.slickgridLwc = new Slicker.GridBundle(gridContainerElm, this.columnDefinitions, this.gridOptions, dataset);
  }

  handleOnCellClicked(e, args) {
    // do something
  }

  onCellChanged(e, args) {
    this.updatedObject = args.item;
    this.aureliaGrid.resizerService.resizeGrid(10);
  }

  onMouseEntered(e, args) {
    // do something
  }
}
```

#### Available Internal Dispatched Events

| Dispatched Events             | Event Aggregator (deprecated) |
|-------------------------------|-------------------------------|
| asg-on-before-export-to-excel | excelExportService:onBeforeExportToExcel |
| asg-on-after-export-to-excel | excelExportService:onAfterExportToExcel |
| asg-on-before-export-to-file | exportService:onBeforeExportToFile |
| asg-on-after-export-to-file | exportService:onAfterExportToFile |
| asg-on-grid-state-changed | gridStateService:changed |
| asg-on-item-added | gridService:onItemAdded |
| asg-on-item-deleted | gridService:onItemDeleted |
| asg-on-item-updated | gridService:onItemUpdated |
| asg-on-item-upserted | gridService:onItemUpserted |
| asg-on-before-resize | resizerService:onBeforeResize |
| asg-on-after-resize | resizerService:onAfterResize |

### 2. Example with Bindable Grid/Dataview
##### View
Bind `dataview.bind` and `grid.bind`
```html
<aurelia-slickgrid 
  gridId="grid2" 
  dataview.bind="dataviewObj" 
  grid.bind="gridObj"
  column-definitions.bind="columnDefinitions" 
  grid-options.bind="gridOptions" 
  dataset.bind="dataset">
</aurelia-slickgrid>
```

##### ViewModel
Hook yourself to the `Changed` event of the bindable `grid` object.
```javascript
export class GridEditorComponent {
  gridObjChanged(grid) {
    this.gridObj = grid;
  }
}
```

#### How to use Grid/Dataview Events
Once the `Grid` and `DataView` are ready (via `changed` bindable events), you can subscribe to any [SlickGrid Events (click to see the full list)](https://github.com/6pac/SlickGrid/wiki/Grid-Events). See below for the `gridChanged(grid)` and `dataviewChanged(dataview)` functions. 
- The `GridExtraUtils` is to bring easy access to common functionality like getting a `column` from it's `row` and `cell` index.
- The example shown below is subscribing to `onClick` and ask the user to confirm a delete, then will delete it from the `DataView`. 
- Technically, the `Grid` and `DataView` are created at the same time by `Aurelia-Slickgrid`, so it's ok to call the `dataViewObj` within some code of the `gridObjChanged()` function since `DataView` object will already be available at that time.

**Note** The example below is demonstrated with `bind` with event `Changed` hook on the `grid` and `dataview` objects. However you can also use the `EventAggregator` as shown earlier. It's really up to you to choose the way you want to call these objects.

##### ViewModel
```javascript
import { inject, bindable } from 'aurelia-framework';
import { Editors, Formatters, GridExtraUtils } from 'aurelia-slickgrid';

export class GridEditorComponent {
  @bindable() gridObj: any;
  @bindable() dataviewObj: any;
  columnDefinitions: Column[];
  gridOptions: GridOption;
  dataset: any[];  
  dataviewObj: any;
  onCellChangeSubscriber: any;
  onCellClickSubscriber: any;

  constructor(private controlService: ControlAndPluginService) {
    // define the grid options & columns and then create the grid itself
    this.defineGrid();
  }

  detached() {
    // don't forget to unsubscribe to the Slick Grid Events
    this.onCellChangeSubscriber.unsubscribe();
    this.onCellClickSubscriber.unsubscribe();
  }

  defineGrid() {
    this.columnDefinitions = [
      { id: 'delete', field: 'id', formatter: Formatters.deleteIcon, maxWidth: 30 }
      // ...
    ];

    this.gridOptions = {
      editable: true,
      enableCellNavigation: true,
      autoEdit: true
    };
  }

  // with bindable Dataview and Changed event
  dataviewObjChanged(dataview) {
    this.dataviewObj = dataview;
  }

  // with bindable Grid and Changed event
  gridObjChanged(grid) {
    this.gridObj = grid;
    this.subscribeToSomeGridEvents(grid);
  }

  subscribeToSomeGridEvents(grid) {
    this.onCellChangeSubscriber = grid.onCellChange.subscribe((e, args) => {
      console.log('onCellChange', args);
      // for example, CRUD with WebAPI calls
    });

    this.onCellClickSubscriber = grid.onClick.subscribe((e, args) => {
      const column = GridExtraUtils.getColumnDefinitionAndData(args);

      if (column.columnDef.id === 'delete') {
        if (confirm('Are you sure?')) {
          this.dataviewObj.deleteItem(column.dataContext.id);
          this.dataviewObj.refresh();
        }
      }
    });
  }
}
```

### 3. Example with Event Aggregators
Aurelia-Slickgrid (starting with version `1.3.x`) have the following Events that you can subscribe to with an Event Aggregator:
- `onDataviewCreated`
- `onGridCreated`
- `onBeforeGridCreate`
- `onBeforeGridDestroy`
- `onAfterGridDestroyed`

##### ViewModel
```javascript
gridCreatedSubscriber: Subscription;
gridBeforeCreationSubscriber: Subscription;

constructor(private ea: EventAggregator) {
  this.gridCreatedSubscriber = ea.subscribe('onGridCreated', (grid) => {
    this.gridObj = grid;
  });
  this.gridBeforeCreationSubscriber = ea.subscribe('onBeforeGridDestroy', (resp) => {
    console.log('onBeforeGridDestroy', resp);
  });
}

detached() {
  this.gridCreatedSubscriber.dispose();
  this.gridBeforeCreationSubscriber.dispose();
}
```