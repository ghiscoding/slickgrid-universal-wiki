##### updated doc to `2.x` version

#### index
* [Inline Editors](/ghiscoding/slickgrid-universal/wiki/Editors#how-to-use-inline-editors)
   * [Custom Editor](/ghiscoding/slickgrid-universal/wiki/Editors#custom-inline-editor)
* [Perform an Action after Inline Edit](/ghiscoding/slickgrid-universal/wiki/Editors#perform-an-action-after-inline-edit)
* [onClick Action Editor (icon click)](/ghiscoding/slickgrid-universal/wiki/Editors#onclick-action-editor-icon-click)
* [AutoComplete Editor](/ghiscoding/slickgrid-universal/wiki/Editors#autocomplete-editor)
* [Select (single/multi) Editors](/ghiscoding/slickgrid-universal/wiki/Editors#select-editors)
  - [Editor Options (multipleSelectOption interface)](/ghiscoding/slickgrid-universal/wiki/Editors#editor-options-multipleselectoption-interface)
  - [Collection Async Load](/ghiscoding/slickgrid-universal/wiki/Editors#collection-async-load)
  - [Collection Label Prefix/Suffix](/ghiscoding/slickgrid-universal/wiki/Editors#collection-label-prefixsuffix)
  - [Collection Label Render HTML](/ghiscoding/slickgrid-universal/wiki/Editors#collection-label-render-html)
  - [`multiple-select.js` Options](/ghiscoding/slickgrid-universal/wiki/Editors#multiple-selectjs-options)
- [Validators](/ghiscoding/slickgrid-universal/wiki/Editors#validators)
   - [Custom Validator](/ghiscoding/slickgrid-universal/wiki/Editors#custom-validator)
- [Disabling specific cell Edit](/ghiscoding/slickgrid-universal/wiki/Editors#disabling-specific-cell-edit)
- [Editors on Mobile Phone](/ghiscoding/slickgrid-universal/wiki/Editors#editors-on-mobile-phone)

## Inline Editors
`Slickgrid-Universal` ships with a few default inline editors (checkbox, dateEditor, float, integer, text, longText). You can see the full list [here](/ghiscoding/slickgrid-universal/tree/master/packages/common/src/editors).

**Note:** For the Float Editor, you can provide decimal places with `params: { decimalPlaces: 2 }` to your column definition else it will be 0 decimal places by default.

### Demo
##### with plain javascript/jQuery
[Demo Page](https://ghiscoding.github.io/slickgrid-universal/#/example03) / [Demo ViewModel](https://github.com/ghiscoding/slickgrid-universal/blob/master/packages/web-demo-vanilla-bundle/src/examples/example03.ts)


### How to use Inline Editors
Simply call the editor in your column definition with the `Editors` you want, as for example (`editor: { model: Editors.text }`). Here is an example with a full column definition:
```ts
this.columnDefinitions = [
  { id: 'title', name: 'Title', field: 'title', type: FieldType.string, editor: { model: Editors.longText } },
  { id: 'duration', name: 'Duration (days)', field: 'duration', type: FieldType.number, editor: { model: Editors.text } },
  { id: 'complete', name: '% Complete', field: 'percentComplete', type: FieldType.number, editor: { model: Editors.integer } },
  { id: 'start', name: 'Start', field: 'start', type: FieldType.date, editor: { model: Editors.date } },
  { 
    id: 'finish', name: 'Finish', field: 'finish', type: FieldType.date, 
    editor: { 
      model: Editors.date,
      
      // you can also add an optional placeholder
      placeholder: 'choose a date'
    }
  },
  { 
    id: 'effort-driven', name: 'Effort Driven', field: 'effortDriven', formatter: Formatters.checkmark, 
    type: FieldType.number, editor: { model: Editors.checkbox } 
  }
];
```

#### Editor Output Type
You could also define an `outputType` to an inline editor. The only built-in Editor with this functionality for now is the `dateEditor`. For example, on a date field, we can call this `outputType: FieldType.dateIso` (by default it uses `dateUtc` as the output):
```javascript
this.columnDefinitions = [
 {
   id: 'start', name: 'Start', field: 'start',
   type: FieldType.date,
   editor: { model: Editors.date }, outputType: FieldType.dateIso
  }
];
```

## Perform an action After Inline Edit
#### Recommended way
What is ideal is to bind to a SlickGrid Event, for that you can take a look at this [Wiki - On Events](/ghiscoding/slickgrid-universal/wiki/Grid-&-DataView-Events)

#### Not recommended
You could also, perform an action after the item changed event with `onCellChange`. However, this is not the recommended way, since it would require to add a `onCellChange` on every every single column definition.

## Custom Inline Editor
To create a Custom Editor, you need to create a `class` that will extend the [`Editors` interface](/ghiscoding/slickgrid-universal/blob/master/packages/common/src/interfaces/editor.interface.ts) and then use it in your grid with `editor: { model: myCustomEditor }` and that should be it.

Once you are done with the class, just reference it's class name as the `editor`, for example:

##### Class implementing Editor
```javascript
export class IntegerEditor implements Editor {
  constructor(private args: any) {
    this.init();
  }

  init(): void {}
  destroy() {}
  focus() {}
  loadValue(item: any) {}
  serializeValue() {}
  applyValue(item: any, state: any) {}
  isValueChanged() {}
  validate() {}
}
```

##### Use it in your Column Definition
TODO add more docs

```ts
this.columnDefinitions = [
  {
    id: 'title2', name: 'Title, Custom Editor', field: 'title',
    type: FieldType.string,
    editor: {
      model: CustomInputEditor // reference your custom editor class
    },
  }
];
```

## OnClick Action Editor (icon click)
Instead of an inline editor, you might want to simply click on an edit icon that could call a modal window, or a redirect URL, or whatever you wish to do. For that you can use the inline `onCellClick` event and define a callback function for the action (you could also create your own [Custom Formatter](https://github.com/ghiscoding/slickgrid-universal/wiki/Formatters)).
- The `Formatters.editIcon` will give you a pen icon, while a `Formatters.deleteIcon` is an "x" icon
```javascript
this.columnDefinitions = [
   {
      id: 'edit', field: 'id',
      formatter: Formatters.editIcon,
      maxWidth: 30,
      onCellClick: (args: OnEventArgs) => {
        console.log(args);
      }
   },
   // ...
];
```
The `args` returned to the `onCellClick` callback is of type `OnEventArgs` which is the following:
```javascript
export interface OnEventArgs {
  row: number;
  cell: number;
  columnDef: Column;
  dataContext: any;
  dataView: any;
  grid: any;
  gridDefinition: GridOption;
}
```

## AutoComplete Editor
(TODO: add autocomplete filter)
The AutoComplete Editor has the same configuration (except for the `model: Editors.autoComplete`) as the AutoComplete Filter, so you can refer to the [AutoComplete Filter Wiki](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Filter) for more info on how to use it.

## Select Editors
The library ships with two select editors: `singleSelectEditor` and the `multipleSelectEditor`. Both support the [multiple-select](https://github.com/ghiscoding/multiple-select-adapted/blob/master/src/multiple-select.js) library, but fallback to the bootstrap form-control style if you decide to exclude this library from your build. These editors will work with a list of foreign key values (custom structure not supported) and can be displayed properly with the [collectionFormatter](/ghiscoding/slickgrid-universal/blob/master/packages/common/src/formatters/collectionFormatter.ts). 

Here's an example with a `collection`, `collectionFilterBy` and `collectionSortBy`

```javascript
this.columnDefinitions = [
  {
    id: 'prerequisites', name: 'Prerequisites', field: 'prerequisites',
    type: FieldType.string,
    editor: {
      model: Editors.multipleSelect,
      collection: Array.from(Array(12).keys()).map(k => ({ value: `Task ${k}`, label: `Task ${k}` })),
      collectionSortBy: {
        property: 'label',
        sortDesc: true
      },
      collectionFilterBy: {
        property: 'label',
        value: 'Task 2'
      }
    }
  }
];
```

### Editor Options (`MultipleSelectOption` interface)
All the available options that can be provided as `editorOptions` to your column definitions can be found under this [multipleSelectOption interface](https://github.com/ghiscoding/slickgrid-universal/blob/master/packages/common/src/interfaces/multipleSelectOption.interface.ts) and you should cast your `editorOptions` to that interface to make sure that you use only valid options of the `multiple-select.js` library. 

```ts
editor: {
  model: Editors.SingleSelect,
  editorOptions: {
    maxHeight: 400
  } as MultipleSelectOption
}
```

### Collection Label Prefix/Suffix
You can use `labelPrefix` and/or `labelSuffix` which will concatenate the multiple properties together (`labelPrefix` + `label` + `labelSuffix`) which will used by each Select Filter option label. You can also use the property `separatorBetweenTextLabels` to define a separator between prefix, label & suffix.

**Note**
If `enableTranslateLabel` flag is set to `True`, it will also try to translate the Prefix / Suffix / OptionLabel texts.

For example, say you have this collection
```javascript
const currencies = [
  { symbol: '$', currency: 'USD', country: 'USA' },
  { symbol: '$', currency: 'CAD', country: 'Canada' }
];
```

You can display all of these properties inside your dropdown labels, say you want to show (symbol with abbreviation and country name). Now you can.

So you can create the  `multipleSelect` Filter with a `customStructure` by using the symbol as prefix, and country as suffix. That would make up something like this:
- $ USD USA
- $ CAD Canada

with a `customStructure` defined as
```javascript
editor: {
  collection: this.currencies,
  customStructure: {
    value: 'currency',
    label: 'currency',
    labelPrefix: 'symbol',
    labelSuffix: 'country',
  collectionOptions: {
    separatorBetweenTextLabels: ' ', // add white space between each text
    includePrefixSuffixToSelectedValues: true // should the selected value include the prefix/suffix in the output format
  },
  model: Editors.multipleSelect
}
```

### Collection Label Render HTML
By default HTML is not rendered and the `label` will simply show HTML as text. But in some cases you might want to render it, you can do so by enabling the `enableRenderHtml` flag.

**NOTE:** this is currently only used by the Editors that have a `collection` which are the `MultipleSelect` & `SingleSelect` Editors.

```javascript
this.columnDefinitions = [
  {
    id: 'effort-driven', name: 'Effort Driven', field: 'effortDriven',
    formatter: Formatters.checkmark,
    type: FieldType.boolean,
    editor: {
      // display checkmark icon when True
      enableRenderHtml: true,
      collection: [{ value: '', label: '' }, { value: true, label: 'True', labelPrefix: `<i class="fa fa-check"></i> ` }, { value: false, label: 'False' }],
      model: Editors.singleSelect
    }
  }
];
```

### `multiple-select.js` Options
You can use any options from [Multiple-Select.js](http://wenzhixin.net.cn/p/multiple-select) and add them to your `filterOptions` property. However please note that this is a customized version of the original (all original [lib options](http://wenzhixin.net.cn/p/multiple-select/docs/) are available so you can still consult the original site for all options).

Couple of small options were added to suit SlickGrid-Universal needs, which is why it points to `slickgrid-universal/lib` folder (which is our customized version of the original). This lib is required if you plan to use `multipleSelect` or `singleSelect` Filters. What was customized to (compare to the original) is the following:
- `okButton` option was added to add an OK button for simpler closing of the dropdown after selecting multiple options.
  - `okButtonText` was also added for locale (i18n)
- `offsetLeft` option was added to make it possible to offset the dropdown. By default it is set to 0 and is aligned to the left of the select element. This option is particularly helpful when used as the last right column, not to fall off the screen.
- `autoDropWidth` option was added to automatically resize the dropdown with the same width as the select filter element.
- `autoAdjustDropHeight` (defaults to true), when set will automatically adjust the drop (up or down) height
- `autoAdjustDropPosition` (defaults to true), when set will automatically calculate the area with the most available space and use best possible choise for the drop to show (up or down)
- `autoAdjustDropWidthByTextSize` (defaults to true), when set will automatically adjust the drop (up or down) width by the text size (it will use largest text width)
- to extend the previous 3 autoAdjustX flags, the following options can be helpful
   - `minWidth` (defaults to null, to use when `autoAdjustDropWidthByTextSize` is enabled)
   - `maxWidth` (defaults to 500, to use when `autoAdjustDropWidthByTextSize` is enabled)
   - `adjustHeightPadding` (defaults to 10, to use when `autoAdjustDropHeight` is enabled), when using `autoAdjustDropHeight` we might want to add a bottom (or top) padding instead of taking the entire available space
   - `maxHeight` (defaults to 275, to use when `autoAdjustDropHeight` is enabled)

##### Code
```javascript
this.columnDefinitions = [
  {
    id: 'isActive', name: 'Is Active', field: 'isActive',
    filterable: true,
    editor: {
      collection: [{ value: '', label: '' }, { value: true, label: 'true' }, { value: false, label: 'false' }],
      model: Editors.singleSelect,
      elementOptions: {
        // add any multiple-select.js options (from original or custom version)
        autoAdjustDropPosition: false, // by default set to True, but you can disable it
        position: 'top'
      }
    }
  }
];
```

### Change Default DOMPurify Options (sanitize html)
If you find that the HTML that you passed is being sanitized and you wish to change it, then you can change the default `sanitizeHtmlOptions` property defined in the Global Grid Options, for more info on how to change these global options, see the `Global Grid Options` and also take a look at the [GitHub - DOMPurify](https://github.com/cure53/DOMPurify#can-i-configure-it) configurations.

## Validators
Each Editor needs to implement the `validate()` method which will be executed and validated before calling the `save()` method. Most Editor will simply validate that the value passed is correctly formed. The Float Editor is one of the more complex one and will first check if the number is a valid float then also check if `minValue` or `maxValue` was passed and if so validate against them. If any errors is found it will return an object of type `EditorValidatorOutput` (see the signature on top).

### Custom Validator
If you want more complex validation then you can implement your own Custom Validator as long as it implements the following  signature.
```ts
export type EditorValidator = (value: any, args?: EditorArgs) => EditorValidatorOutput;
```
So the `value` can be anything but the `args` is interesting since it provides multiple properties that you can hook into, which are the following
```ts
export interface EditorArgs {
  column: Column;
  container: any;
  grid: any;
  gridPosition: ElementPosition;
  item: any;
  position: ElementPosition;
  cancelChanges?: () => void;
  commitChanges?: () => void;
}
```
And finally the Validator Output has the following signature
```ts
export interface EditorValidatorOutput {
  valid: boolean;
  msg?: string | null;
}
```

So if we take all of these informations and we want to create our own Custom Editor to validate a Title field, we could create something like this:
```ts
const myCustomTitleValidator: EditorValidator = (value: any, args: EditorArgs) => {
  // you can get the Editor Args which can be helpful, e.g. we can get the Translate Service from it
  const grid = args && args.grid;
  const gridOptions = (grid && grid.getOptions) ? grid.getOptions() : {};
  const i18n = gridOptions.i18n;

  if (value == null || value === undefined || !value.length) {
    return { valid: false, msg: 'This is a required field' };
  } else if (!/^Task\s\d+$/.test(value)) {
    return { valid: false, msg: 'Your title is invalid, it must start with "Task" followed by a number' };
    // OR use the Translate Service with your custom message
    // return { valid: false, msg: i18n.tr('YOUR_ERROR', { x: value }) };
  } else {
    return { valid: true, msg: '' };
  }
};
```
and use it in our Columns Definition like this:
```ts
this.columnDefinition = [
  {
    id: 'title', name: 'Title', field: 'title',
    editor: {
      model: Editors.longText,
      validator: myCustomTitleValidator, // use our custom validator
    },
    onCellChange: (e: Event, args: OnEventArgs) => {
      // do something
      console.log(args.dataContext.title);
    }
  }
];
```

## Disabling specific cell edit
This can be answered by searching on Stack Overflow Stack Overflow and this is the best [answer](https://stackoverflow.com/questions/10491676/disabling-specific-cell-edit-in-slick-grid) found.

#### View
```html
<div id="grid1">
</div>
```

#### Component
```ts
  attached() {
    const dataset = this.initializeGrid();
    const gridContainerElm = document.querySelector<HTMLDivElement>(`.grid4`);

    // gridContainerElm.addEventListener('onclick', handleOnClick);
    gridContainerElm.addEventListener('onbeforeeditcell', this.handleOnBeforeEditVerifyCellIsEditable.bind(this));
    this.slickgridLwc = new Slicker.GridBundle(gridContainerElm, this.columnDefinitions, { ...ExampleGridOptions, ...this.gridOptions }, dataset);
  }

  handleOnBeforeEditVerifyCellIsEditable(event) {
        const eventData = event.detail.eventData;
        const args = event && event.detail && event.detail.args;
        const { column, item, grid } = args;

        if (column && item) {
            if (!checkItemIsEditable(item, column, grid)) {
                event.preventDefault();
                eventData.stopImmediatePropagation();
            }
        }
        return false;
  }
```

#### SalesForce (ES6)
For SalesForce it's nearly the same, the only difference is that we add our events in the View instead of in the ViewModel

#### View (SF)
```html
<div class="grid-container slds-p-horizontal" style="padding: 10px">
    <div class="grid1" 
            onvalidationerror={handleOnValidationError} 
            onbeforeeditcell={handleOnBeforeEditVerifyCellIsEditable}
            onslickergridcreated={handleOnSlickerGridCreated}>
    </div>
</div>
```

### Editors on Mobile Phone
If your grid uses the `autoResize` and you use Editors in your grid on a mobile phone, Android for example, you might have undesired behaviors. It might call a grid resize (and lose input focus) since the touch keyboard appears. This in term, is a bad user experience to your user, but there is a way to avoid this, you could use the `pauseResizer`

##### View
```
<div id="grid1">
</div>
```
##### Component
```
  attached() {
    const dataset = this.initializeGrid();
    const gridContainerElm = document.querySelector<HTMLDivElement>(`.grid4`);

    gridContainerElm.addEventListener('onslickergridcreated', this.handleOnSlickerGridCreated.bind(this));
    this.slickgridLwc = new Slicker.GridBundle(gridContainerElm, this.columnDefinitions, this.gridOptions, dataset);
  }

  handleOnSlickerGridCreated(event) {
    this.slickerGridInstance = event && event.detail;
    this.gridObj = this.slickerGridInstance && this.slickerGridInstance.slickGrid;
    this.dataViewObj = this.slickerGridInstance && this.slickerGridInstance.dataView;
  }

  onAfterEditCell($event) {
    // resume autoResize feature,  and after leaving cell editing mode
    // force a resize to make sure the grid fits the current dimensions
    this.slickerGridInstance.resizerService.pauseResizer(false);    
    this.slickerGridInstance.resizerService.resizeGrid();
  }

  onBeforeEditCell($event) {
    this.slickerGridInstance.resizerService.pauseResizer(true);
  }
```

#### SalesForce (ES6)
For SalesForce it's nearly the same, the only difference is that we add our events in the View instead of in the ViewModel

#### View (SF)
```html
<div class="grid-container slds-p-horizontal" style="padding: 10px">
    <div class="grid1" onslickergridcreated={handleOnSlickerGridCreated}>
    </div>
</div>