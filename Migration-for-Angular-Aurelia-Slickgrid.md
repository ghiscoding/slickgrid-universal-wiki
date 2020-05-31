Some of the Deprecated Code and Feature Changes

### Deprecations

## Changes
- Grid Height/Width should now be passed through the Grid Options `this.gridOptions = { gridHeight: 500, gridWidth: 600 }`
- `ExcelExportService` is now an opt-in package [@slickgrid-universal/excel-export](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/excel-export)
- `ExportService` was renamed to `FileExportService` and is now an opt-in package [@slickgrid-universal/file-export](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/file-export)

### Backend Service API
Note that the `BackendServiceApi` is no longer exposed in the `{Angular|Aurelia}GridInstance`, so if you wish to reference it (for example when you want to use it with an external export button), then create a reference while instantiating it.

### Excel Export Service
You need to use the new `@slickgrid-universal/excel-export` package and register the service in your grid options.
##### ViewModel
```ts
import { ExcelExportService } from '@slickgrid-universal/excel-export';

export class MyExample {
  prepareGrid {
    this.gridOptions = {
      enableExcelExport: true,
      excelExportOptions: {
        sanitizeDataExport: true
      },
      registerExternalServices: [new ExcelExportService()],
    }
  }
}
```
Also, note that the `ExcelExportService` is no longer exposed in the `{Angular|Aurelia}GridInstance`, so if you wish to reference it (for example when you want to use it with an external export button), then create a reference while instantiating it.
```ts
import { ExcelExportService } from '@slickgrid-universal/excel-export';

export class MyExample {
  excelExportService = new ExcelExportService();

  prepareGrid {
    this.gridOptions = {
      enableExcelExport: true,
      excelExportOptions: {
        sanitizeDataExport: true
      },
      registerExternalServices: [this.excelExportService],
    }
  }

  exportToExcel() {
    this.excelExportService.exportToExcel({ filename: 'export', format: FileType.xlsx });
  }
}
```