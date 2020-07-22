## Installation Instructions for Salesforce
This document shows how to get this Slickgrid bundle working with Salesforce LWC (Lighning Web Component). 

### Step 1. install dependencies (static resources) only once in your org
The first thing you'll need to do is to add 3 different dependencies as static resources, you can see below the name of each static resources we use in our Salesforce org. The resource named `Sf_SlickGrid` comes from the zip file that gets created every time a new dist build is updated, the zip file can be downloaded [here](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/vanilla-bundle/dist-grid-bundle-zip). 

#### Here are the 3 static resource files
Click on the `zip` link and then the `Download` button on the top right to download it locally, then upload it to your org.

| Static Resource Name | Zip | Notes |
| -------- | --- | ----------- |
| `jQuery3` | [zip](/ghiscoding/slickgrid-universal/blob/master/packages/vanilla-bundle/external-libs/jQuery3.zip) | jQuery version 3.4.1 |
| `jQueryUI` | [zip](/ghiscoding/slickgrid-universal/blob/master/packages/vanilla-bundle/external-libs/jQueryUI.zip) | jQuery UI version 1.12.x |
| `Sf_SlickGrid` | [zip](/ghiscoding/slickgrid-universal/blob/master/packages/vanilla-bundle/dist-grid-bundle-zip/slickgrid-vanilla-bundle.zip) | filename is `slickgrid-vanilla-bundle.zip` but you must name your static resource as **`Sf_SlickGrid`** |



### Step 2. load Slickgrid
Create all the Static Resources that are required by Slickgrid as shown below (they could be different names in your org).

In the same file, load all external files with `renderedCallback` and get your data through a `@wire` method. Technically the `@wire` method will be processed before the `renderedCallback` and so you can assume that when calling the `initializeGrid` method we will already have the dataset ready.

Notice below that in the `gridOptions`, there is a flag `useSalesforceDefaultGridOptions` that was added specifically for Salesforce project, it will internally use these [grid options](https://github.com/ghiscoding/slickgrid-universal/blob/master/packages/vanilla-bundle/src/salesforce-global-grid-options.ts) for Salesforce (these options are merged with the following default [grid options](https://github.com/ghiscoding/slickgrid-universal/blob/master/packages/common/src/global-grid-options.ts)).
```js
import { LightningElement, api, wire } from 'lwc';
import { loadStyle, loadScript } from 'lightning/platformResourceLoader';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';

// Static Resources (jQuery, jQueryUI, Slickgrid, and Icon Font)
import jQuery_bundle from '@salesforce/resourceUrl/jQuery3';
import jQueryUI_bundle from '@salesforce/resourceUrl/jQueryUI';
import sf_slickGrid_bundle from '@salesforce/resourceUrl/Sf_SlickGrid'; // the zip described at step 1.1

import getSomeData from '@salesforce/apex/SomeService.getSomeData';

export default class YourComponent extends LightningElement {
  slickGridInitialized = false;
  slickGridLwc;
  isLoaded = false;
  dataset = []; // load your data through an Apex Controller with @wire

  @api recordId;

  @wire(getSomeData, { recordId: '$recordId' })
  wiredGetSomeData({ error, data }) {
    if (data) {
        this.dataset = data || [];
        
        if (window.Slicker && window.Slicker.Utilities && this.slickGridLwc) {
            this.slickGridLwc.dataset = this.dataset;
        }
    } else if (error) {}
    this.isLoaded = true; // stop the spinner
  }

  async renderedCallback() {
    if (this.slickGridInitialized) {
        return;
    }

    try {
        this.slickGridInitialized = true;

        // load all CSS Styles
        await loadStyle(this, `${sf_slickGrid_bundle}/styles/css/slickgrid-theme-salesforce.css`);

        // load all JS files
        await loadScript(this, `${jQuery_bundle}/jquery.min.js`);
        await loadScript(this, `${jQueryUI_bundle}/jquery-ui.min.js`);
        await loadScript(this, `${sf_slickGrid_bundle}/slickgrid-vanilla-bundle.js`);

        // create the grid (column definitions, grid options & dataset)
        this.initializeGrid();        
    } catch (error) {
        this.dispatchEvent(new ShowToastEvent({ title: 'Error loading SlickGrid', message: error && error.message || '', variant: 'error', }));
    }
  }

  initializeGrid() {
    this.columnDefinitions = [
      { id: 'firstName', name: 'First Name', field: 'firstName' },
      { id: 'lastName', name: 'Last Name', field: 'lastName' },
      // ...
   ];

    this.gridOptions = { 
      useSalesforceDefaultGridOptions: true,  // enable this flag to use regular grid options used for SF project

      autoResize: {
        container: '.grid-container',
        minHeight: 250,
        rightPadding: 50,
        bottomPadding: 75,
      },

      // or use fixed size
      // gridHeight: 300,
      // gridWidth: 800,

      // datasetIdPropertyName: 'someOtherId', // default is "Id" (case sensitive)

      /** other options... */ 
    };

    // find your HTML slickGrid container & pass it to the Slicker.GridBundle instantiation
    const gridContainerElement = this.template.querySelector(`.user-grid`);
    this.slickGridLwc = new Slicker.GridBundle(gridContainerElement, this.columnDefinitions, this.gridOptions, this.dataset);
  }
}
```

###### Template
```html
<template>
    <!-- show a spinner -->
    <div if:false={isLoaded} class="slds-is-relative">
          <lightning-spinner alternative-text="Loading...">
          </lightning-spinner>
    </div>

    <!-- slickGrid container-->
    <div class="grid-container slds-p-horizontal">
          <div class="user-grid"></div>
    </div>
</template>
```
