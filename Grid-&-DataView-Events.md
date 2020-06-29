SlickGrid has a nice amount of [Grid Events](https://github.com/6pac/SlickGrid/wiki/Grid-Events) or [DataView Events](https://github.com/6pac/SlickGrid/wiki/Dataview-Events) which you can use by simply hook a `subscribe` to them (the `subscribe` are a custom `SlickGrid Event` and are **NOT** an `RxJS Observable` type but they very similar). You can access them in Slickgrid-Universal by following the documentation below

##### View
```html
<div class="grid1">
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
    gridContainerElm.addEventListener('oncellchanged', this.handleOnCellChanged.bind(this));
    gridContainerElm.addEventListener('onslickergridcreated', this.handleOnSlickerGridCreated.bind(this));
    this.slickgridLwc = new Slicker.GridBundle(gridContainerElm, this.columnDefinitions, this.gridOptions, dataset);
  }

  handleOnSlickerGridCreated(event) {
    this.slickerGridInstance = event && event.detail;
    this.gridObj = this.slickerGridInstance && this.slickerGridInstance.slickGrid;
    this.dataViewObj = this.slickerGridInstance && this.slickerGridInstance.dataView;
  }

  handleOnCellClicked(e, args) {
    // do something
  }

  handleOnCellChanged(e, args) {
    this.updatedObject = args.item;
    this.slickerGridInstance.resizerService.resizeGrid(10);
  }
}
```

##### View - SalesForce (ES6)
For SalesForce it's nearly the same, the only difference is that we add our events in the View instead of in the ViewModel

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