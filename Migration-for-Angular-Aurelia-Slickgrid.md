Some of the Deprecated Code and Feature Changes

## Biggest Changes
1. Backend Service API are now decoupled, see [Backend Service API](https://github.com/ghiscoding/slickgrid-universal/wiki/Migration-for-Angular-Aurelia-Slickgrid#backend-service-apis)
2. Export to File & Export to Excel are now decoupled, see []()

### Deprecated Code (removed)
- removed `registerPlugins` Grid Option since all useful plugins/controls already exist in the lib.
- removed `hideColumn(column)` please use `hideColumnById` or `hideColumnByIds` instead
- removed `hideColumnByIndex(idx)` please use `hideColumnById` or `hideColumnByIds` instead

## Changes
- Grid Height/Width should now be passed through the Grid Options instead of the View
   - `this.gridOptions = { gridHeight: 500, gridWidth: 600 }`
- Grid Service `updateItem()` will no longer highlight the row by default (if you want to get this behavior then change the options `highlightRow: true`)
- `CheckboxSelector` interface renamed to `CheckboxSelectorOption`
- `ExcelExportService` is now an opt-in package [@slickgrid-universal/excel-export](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/excel-export)
- `ExportService` was renamed to `FileExportService` (export extensions are `.txt`, `.csv`) and is now an opt-in package [@slickgrid-universal/file-export](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/file-export)
  - however note that the options are still under the same grid option property name of `exportOptions`
- Header Menu Commands (typo)
  - rename `hideFilterCommands` to singular `hideFilterCommand` since there can only be 1 filter per column
- renamed `EditorValidatorOutput` interface to `EditorValidationResult`
- `Sorter` interface got renamed to `SortComparer`
- `Sorters` got renamed to `SortComparers` (often used when using the Grouping feature)

### Backend Service APIs
Note that the `BackendServiceApi` is no longer exposed in the `{Angular|Aurelia}GridInstance`, so if you wish to reference it (for example when you want to use it with an external export button), then create a reference while instantiating it.

#### Changes
- `columnDefinitions` is no longer a valid property of `BackendServiceOption` which means that you cannot use it anymore with OData/GraphQL. This is no longer necessary since the Services can get the columns definition right from the grid object.

##### OData Service
```ts
import { Column, GridOption } from 'angular-slickgrid'; // or 'aurelia-slickgrid'
import { GridOdataService, OdataServiceApi, OdataOption } from '@slickgrid-universal/odata';

export class MyExample {
  prepareGrid {
    this.columnDefinitions = [ /*...*/ ];
    this.gridOptions = {
      backendServiceApi: {
        service: new GridOdataService(),
        options: { /*...*/ } as OdataServiceApi
    }
  }
}
```

##### GraphQL Service
```ts
import { Column, GridOption } from 'angular-slickgrid'; // or 'aurelia-slickgrid'
import { GraphqlService, GraphqlPaginatedResult, GraphqlServiceApi, } from '@slickgrid-universal/graphql';

export class MyExample {
  prepareGrid {
    this.columnDefinitions = [ /*...*/ ];
    this.gridOptions = {
      backendServiceApi: {
        service: new GraphqlService(),
        options: { /*...*/ } as GraphqlServiceApi
    }
  }
}
```

### Export Services
You need to use the new `@slickgrid-universal/excel-export` and/or `@slickgrid-universal/file-export` packages and register the service(s) in your grid options as shown below.
##### ViewModel
```ts
import { Column, GridOption } from 'angular-slickgrid'; // or 'aurelia-slickgrid'
import { ExcelExportService } from '@slickgrid-universal/excel-export';
import { FileExportService } from '@slickgrid-universal/file-export';

export class MyExample {
  prepareGrid {
    this.columnDefinitions = [ /*...*/ ];
    this.gridOptions = {
      enableExcelExport: true,
      enableExport: true,
      excelExportOptions: { sanitizeDataExport: true },
      exportOptions: { sanitizeDataExport: true },
      registerExternalServices: [new ExcelExportService(), new FileExportService()],
    }
  }
}
```
Also, note that the `ExcelExportService` is no longer exposed in the `{Angular|Aurelia}GridInstance`, so if you wish to reference it (for example when you want to use it with an external export button), then create a reference while instantiating it.
```ts
import { Column, GridOption } from 'angular-slickgrid'; // or 'aurelia-slickgrid'
import { ExcelExportService } from '@slickgrid-universal/excel-export';

export class MyExample {
  excelExportService = new ExcelExportService();

  prepareGrid {
    this.columnDefinitions = [ /*...*/ ];
    this.gridOptions = {
      enableExcelExport: true,
      excelExportOptions: { sanitizeDataExport: true },
      registerExternalServices: [this.excelExportService],
    }
  }

  exportToExcel() {
    this.excelExportService.exportToExcel({ filename: 'export', format: FileType.xlsx });
  }
}
```