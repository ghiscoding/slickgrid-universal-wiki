#### Index
- [Using fixed `collection` or `collectionAsync`](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#using-collection-or-collectionasync)
- [Filter Options (`AutocompleteOption` interface)](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#filter-options-autocompleteoption-interface)
- [Using Remote API](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#using-external-remote-api)
  - [with `renderItem` + custom Layout (`twoRows` or `fourCorners`)](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#remote-api-with-renderitem--custom-layout-tworows-or-fourcorners)
  - [with jQueryUI `_renderItem` callback + custom Layout (`twoRows` or `fourCorners`)](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#remote-api-with-jquery-ui-_renderitem-callback--custom-layout-tworows-or-fourcorners)
- [Force User Input](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#autocomplete---force-user-input)
- [Animated Gif Demo](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#animated-gif-demo)

### Demo
[Demo Page](https://ghiscoding.github.io/slickgrid-universal/#/example11) | [Demo Component](/ghiscoding/slickgrid-universal/blob/master/examples/webpack-demo-vanilla-bundle/src/examples/example11.ts)

### Introduction
AutoComplete is a functionality that let the user start typing characters and the autocomplete will try to give suggestions according to the characters entered. The collection can be a fxied JSON files (collection of strings or objects) or can also be an external remote resource to an external API. For a demo of what that could look like, take a look at the [animated gif demo](/ghiscoding/aurelia-universal/wiki/AutoComplete-Editor#animated-gif-demo) below.

## Using `collection` or `collectionAsync`
If you want to pass the entire list to the AutoComplete (like a JSON file or a Web API call), you can do so using the `collection` or the `collectionAsync` (the latter will load it asynchronously). You can also see that the Editor and Filter have almost the exact same configuration (apart from the `model` that is obviously different).

##### View
```html
<aurelia-slickgrid
    grid-id="gridId"
    column-definitions.bind="columnDefinitions"
    grid-options.bind="gridOptions"
    dataset.bind="dataset">
</aurelia-slickgrid>
```

##### Component
```javascript
export class GridBasicComponent {
  columnDefinitions: Column[];
  gridOptions: GridOption;
  dataset: any[];

  attached(): void {
      // your columns definition
    this.columnDefinitions = [
      {
        id: 'countryOfOrigin', name: 'Country of Origin', field: 'countryOfOrigin',
        formatter: Formatters.complexObject,
        dataKey: 'code', // our list of objects has the structure { code: 'CA', name: 'Canada' }, since we want to use the code`, we will set the dataKey to "code"
        labelKey: 'name', // while the displayed value is "name"
        type: FieldType.object,
        sorter: Sorters.objectString, // since we have set dataKey to "code" our output type will be a string, and so we can use this objectString, this sorter always requires the dataKey
        filterable: true,
        sortable: true,
        minWidth: 100,
        editor: {
          model: Editors.autoComplete,
          customStructure: { label: 'name', value: 'code' },
          collectionAsync: this.http.get('assets/data/countries.json'), // this demo will load the JSON file asynchronously
        },
        filter: {
          model: Filters.autoComplete,
          customStructure: { label: 'name', value: 'code' },
          collectionAsync: this.http.get('assets/data/countries.json'),
        }
      }
    ];

    this.gridOptions = {
      // your grid options config
    }
  }
}
```

### Filter Options (`AutocompleteOption` interface)
All the available options that can be provided as `filterOptions` to your column definitions can be found under this [AutocompleteOption interface](/ghiscoding/Angular-Slickgrid/blob/master/src/app/modules/angular-slickgrid/models/autocompleteOption.interface.ts) and you should cast your `filterOptions` to that interface to make sure that you use only valid options of the jQueryUI autocomplete library. 

```ts
filter: {
  model: Filters.autoComplete,
  filterOptions: {
    minLength: 3,
  } as AutocompleteOption
}
```

## Using External Remote API
You could also use external 3rd party Web API (can be JSONP query or regular JSON). This will make a much shorter result since it will only return a small subset of what will be displayed in the AutoComplete Editor or Filter. For example, we could use GeoBytes which provide a JSONP Query API for the cities of the world, you can imagine the entire list of cities would be way too big to download locally, so this is why we use such API.

### Remote API with `renderItem` + custom layout (`twoRows` or `fourCorners`)
#### See animated gif ([twoRows](/ghiscoding/slickgrid-universal/wiki/AutoComplete-Editor#with-tworows-custom-layout-without-optional-left-icon) or [fourCorners]())
The lib comes with 2 built-in custom layouts, these 2 layouts also have SASS variables if anyone wants to style it differently. When using the `renderItem`, it will require the user to provide a `layout` (2 possible options `twoRows` or `fourCorners`) and also a `templateCallback` that will be executed when rendering the AutoComplete Search List Item. For example:

##### Component
```javascript
export class GridBasicComponent {
  columnDefinitions: Column[];
  gridOptions: GridOption;
  dataset: any[];

  initializeGrid() {
      // your columns definition
    this.columnDefinitions = [
      {
        id: 'product', name: 'Product', field: 'product',
        filterable: true,
        minWidth: 100,
        editor: {
          model: Editors.autoComplete,
          alwaysSaveOnEnterKey: true,
          customStructure: {
            label: 'itemName',
            value: 'id'
          },
          editorOptions: {
            openSearchListOnFocus: true,
            minLength: 1,
            source: (request, response) => {
              const products = yourAsyncApiCall(request.term) // typically you'll want to return no more than 10 results
                 .then(result => response((results.length > 0) ? results : [{ label: 'No match found.', value: '' }]); })
                 .catch(error => console.log('Error:', error);
            },
            renderItem: {
              layout: 'twoRows',
              templateCallback: (item: any) => `<div class="autocomplete-container-list">
                <div class="autocomplete-left">
                  <span class="mdi ${item.icon} mdi-26px"></span>
                </div>
                <div>
                  <span class="autocomplete-top-left">
                    <span class="mdi ${item.itemTypeName === 'I' ? 'mdi-information-outline' : 'mdi-content-copy'} mdi-14px"></span>
                    ${item.itemName}
                  </span>
                <div>
              </div>
              <div>
                <div class="autocomplete-bottom-left">${item.itemNameTranslated}</div>
              </div>`,
            },
          } as AutocompleteOption,
        },
      }
    ];

    this.gridOptions = {
      // your grid options config
    }
  }
}
```

### Remote API with jQuery UI `_renderItem` callback + custom layout (`twoRows` or `fourCorners`)
The previous example can also be written using the jQuery UI `_renderItem` callback and adding `classes`, this is actually what Slickgrid-Universal does internally, you can do it yourself if you wish to have more control on the render callback result. 

##### Component
```javascript
export class GridBasicComponent {
  columnDefinitions: Column[];
  gridOptions: GridOption;
  dataset: any[];

  initializeGrid() {
      // your columns definition
    this.columnDefinitions = [
      {
        id: 'product', name: 'Product', field: 'product',
        filterable: true,
        minWidth: 100,
        editor: {
          model: Editors.autoComplete,
          alwaysSaveOnEnterKey: true,
          customStructure: {
            label: 'itemName',
            value: 'id'
          },
          editorOptions: {
            openSearchListOnFocus: true,
            minLength: 1,
            classes: {
              // choose a custom style layout
              // 'ui-autocomplete': 'autocomplete-custom-two-rows', 
              'ui-autocomplete': 'autocomplete-custom-four-corners',
            },
            source: (request, response) => {
              const products = yourAsyncApiCall(request.term) // typically you'll want to return no more than 10 results
                 .then(result => response((results.length > 0) ? results : [{ label: 'No match found.', value: '' }]); })
                 .catch(error => console.log('Error:', error);
            },
            renderItem: {
              layout: 'twoRows',
              templateCallback: (item: any) => `<div class="autocomplete-container-list">
                <div class="autocomplete-left">
                  <span class="mdi ${item.icon} mdi-26px"></span>
                </div>
                <div>
                  <span class="autocomplete-top-left">
                    <span class="mdi ${item.itemTypeName === 'I' ? 'mdi-information-outline' : 'mdi-content-copy'} mdi-14px"></span>
                    ${item.itemName}
                  </span>
                <div>
              </div>
              <div>
                <div class="autocomplete-bottom-left">${item.itemNameTranslated}</div>
              </div>`,
            },
          } as AutocompleteOption,
          callbacks: {
             // callback on the jQuery UI AutoComplete on the instance, example from https://jqueryui.com/autocomplete/#custom-data
             _renderItem: (ul: HTMLElement, item: any) => {
                const template = `<div class="autocomplete-container-list">
                      <div class="autocomplete-left">
                        <!--<img src="http://i.stack.imgur.com/pC1Tv.jpg" width="50" />-->
                        <span class="mdi ${item.icon} mdi-26px"></span>
                      </div>
                      <div>
                        <span class="autocomplete-top-left">
                          <span class="mdi ${item.itemTypeName === 'I' ? 'mdi-information-outline' : 'mdi-content-copy'} mdi-14px"></span>
                          ${item.itemName}
                        </span>
                      <div>
                    </div>
                    <div>
                      <div class="autocomplete-bottom-left">${item.itemNameTranslated}</div>
                    </div>`;

                return $('<li></li>')
                  .data('item.autocomplete', item)
                  .append(template)
                  .appendTo(ul);
              }
          },
        },
      }
    ];

    this.gridOptions = {
      // your grid options config
    }
  }
}
```

#### with JSONP
Example from an external remote API (geobytes) returning a JSONP response. 

##### Component
```javascript
export class GridBasicComponent {
  columnDefinitions: Column[];
  gridOptions: GridOption;
  dataset: any[];

  attached(): void {
      // your columns definition
    this.columnDefinitions = [
      {
        id: 'cityOfOrigin', name: 'City of Origin', field: 'cityOfOrigin',
        filterable: true,
        minWidth: 100,
        editor: {
          model: Editors.autoComplete,
          placeholder: 'search city', //  you can provide an optional placeholder to help your users

          // use your own autocomplete options, instead of $.ajax, use http
          // here we use $.ajax just because I'm not sure how to configure http with JSONP and CORS
          editorOptions: {
            minLength: 3, // minimum count of character that the user needs to type before it queries to the remote
            source: (request, response) => {
              $.ajax({
                url: 'http://gd.geobytes.com/AutoCompleteCity',
                dataType: 'jsonp',
                data: {
                  q: request.term // geobytes requires a query with "q" queryParam representing the chars typed (e.g.:  gd.geobytes.com/AutoCompleteCity?q=van
                },
                success: (data) => response(data)
              });
            }
          },
        },
        filter: {
          model: Filters.autoComplete,
          // placeholder: '&#128269; search city', // &#128269; is a search icon, this provide an option placeholder

          // use your own autocomplete options, instead of $.ajax, use http
          // here we use $.ajax just because I'm not sure how to configure http with JSONP and CORS
          filterOptions: {
            minLength: 3, // minimum count of character that the user needs to type before it queries to the remote
            source: (request, response) => {
              $.ajax({
                url: 'http://gd.geobytes.com/AutoCompleteCity',
                dataType: 'jsonp',
                data: {
                  q: request.term
                },
                success: (data) => {
                  response(data);
                }
              });
            }
          },
        }
      }
    ];

    this.gridOptions = {
      // your grid options config
    }
  }
}
```

## Autocomplete - force user input
If you want to add the autocomplete functionality but want the user to be able to input a new option, then follow the example below:

```ts
  this.columnDefinitions = [{
        id: 'area',
        name: 'Area',
        field: 'area',
        type: FieldType.string,
        filter: {
          model: Filters.autoComplete,
          filterOptions: {
            minLength: 0,
            forceUserInput: true,
            source: this.areas, // add here the array
          }
        }
      },
  ];
```
You can also use the `minLength` to limit the autocomplete text to `0` characters or more, the default number is `3`.

## Animated Gif Demo
### Basic (default jQuery UI layout)
![](https://user-images.githubusercontent.com/643976/50624023-d5e16c80-0ee9-11e9-809c-f98967953ba4.gif)

### with `twoRows` custom layout (without optional left icon)
![](https://i.imgur.com/V9XzVXS.gif)

### with `fourCorners` custom layout (with extra optional left icon)
![](https://i.imgur.com/LirGZFm.gif)