#### index
  - [Editor Options (multipleSelectOption interface)](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#editor-options-multipleselectoption-interface)
  - [Complex Object](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#complex-object)
  - [Collection Async Load](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#collection-async-load)
  - [Collection Label Prefix/Suffix](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#collection-label-prefixsuffix)
  - [Collection Label Render HTML](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#collection-label-render-html)
  - [`multiple-select.js` Options](/ghiscoding/slickgrid-universal/wiki/Select-Dropdown-Editor-(single-multiple)#multiple-selectjs-options)
  - See the [Editors - Wiki](/ghiscoding/slickgrid-universal/wiki/Editors) for more general info about Editors (validators, event handlers, ...)

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
### Complex Object
If your `field` string has a dot (.) it will automatically assume that it is dealing with a complex object. There are however some options you can use with a complex object, the following options from the `ColumnEditor` might be useful to you
```ts
interface ColumnEditor {
  /**
   * When providing a dot (.) notation in the "field" property of a column definition, we might want to use a different path for the editable object itself
   * For example if we provide a coldef = { field: 'user.name' } but we use a SingleSelect Editor with object values, we could override the path to simply 'user'
   */
  complexObjectPath?: string;

  /**
   * defaults to 'object', how do we want to serialize the editor value to the resulting dataContext object when using a complex object?
   * For example, if keep default "object" format and the selected value is { value: 2, label: 'Two' } then the end value will remain as an object, so { value: 2, label: 'Two' }.
   * On the other end, if we set "flat" format and the selected value is { value: 2, label: 'Two' } then the end value will be 2.
   */
  serializeComplexValueFormat?: 'flat' | 'object';
}
```

```ts
this.columnDefinitions = [{
  id: 'firstName', name: 'First Name', field: 'user.firstName',
  editor: {
    model: Editors.SingleSelect,
    complexObjectPath: 'user.middleName',
    serializeComplexValueFormat: 'flat' // (flat) will return 'Bob', (object) will return { label: 'Bob', value: 'Bob' }
  }
}];
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
