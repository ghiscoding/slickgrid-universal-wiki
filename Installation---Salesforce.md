## Installation Instructions for Salesforce
This document shows how to get this Slickgrid bundle working with Salesforce LWC (Lighning Web Component). 

### Step 1. install dependencies (static resources) only once in your org
The first thing you'll need to do is to add 4 different dependencies as static resources, you can see below the name of each static resources we use in our Salesforce org. The resource named `Sf_SlickGrid` comes from the zip file that gets created every time a new dist build is updated, the zip file can be downloaded [here](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/vanilla-bundle/dist-grid-bundle-zip). 

Download the slickgrid bundle zip file from [here](https://github.com/ghiscoding/slickgrid-universal/tree/master/packages/vanilla-bundle/dist-grid-bundle-zip), the zip file is meant to be plug-and-play, just upload it as a static resource and it should work.

### Step 2. load Slickgrid
Create all the Static Resources that are required by Slickgrid as shown below (they could be different names in your org).

In the same file, load all external files with `renderedCallback` and get your data through a `@wire` method. Technically the `@wire` method will be processed before the `renderedCallback` and so you can assume that when calling the `initializeGrid` method we will already have the dataset ready.
```js
import { LightningElement, api, track, wire } from 'lwc';
import getQuoteLineItemsByQuoteId from '@salesforce/apex/SlickGridDataService.getQuoteLineItemsByQuoteId';

// Static Resources (jQuery, jQueryUI, Slickgrid, and Icon Font)
import jQuery_bundle from '@salesforce/resourceUrl/jQuery3';
import jQueryUI_bundle from '@salesforce/resourceUrl/jQueryUI';
import sf_slickgrid_bundle from '@salesforce/resourceUrl/Sf_SlickGrid'; // the zip described at step 1.1
import materialicons_bundle from '@salesforce/resourceUrl/material_icons';

slickgridLwc;
isLoaded = false;
dataset = []; // load your data through an Apex Controller with @wire

@wire(getQuoteLineItemsByQuoteId, { quoteId: '$recordId' })
wiredQuoteLines({ error, data }) {
    if (data) { this.dataset = data; } else if (error) {}
    this.isLoaded = true; // stop the spinner
}

async renderedCallback() {
    try {
        // load all CSS Styles
        await loadStyle(this, `${materialicons_bundle}/css/materialdesignicons.min.css`);
        await loadStyle(this, `${sf_slickgrid_bundle}/styles/css/se-slickgrid-theme-material.css`);

        // load all JS files
        await loadScript(this, `${jQuery_bundle}/jquery.min.js`);
        await loadScript(this, `${jQueryUI_bundle}/jquery-ui.min.js`);
        await loadScript(this, `${sf_slickgrid_bundle}/slickgrid-vanilla-bundle.js`);

        // create the grid (column definitions, grid options & dataset)
        this.initializeGrid();
    } catch (error) {
        this.dispatchEvent(new ShowToastEvent({ title: 'Error loading SlickGrid', message: error && error.message || '', variant: 'error', }));
    }
}

initializeGrid() {
    this.columnDefinitions = [ /* ... */ ];
    this.gridOptions = { /** options */ };

    // find your HTML slickgrid container & pass it to the Slicker.GridBundle instantiation
    const gridContainerElm = this.template.querySelector(`.myGrid`);
    this.slickgridLwc = new Slicker.GridBundle(gridContainerElm, this.columnDefinitions, this.gridOptions, this.dataset);
}
```

###### Template
```html
<template>
    <lightning-card>
        <!-- show a spinner -->
        <div if:false={isLoaded} class="slds-is-relative">
            <lightning-spinner alternative-text="Loading...">
            </lightning-spinner>
        </div>

        <!-- slickgrid container-->
        <div class="grid-container">
            <div class="myGrid"></div>
        </div>
    </lightning-card>
</template>
```
