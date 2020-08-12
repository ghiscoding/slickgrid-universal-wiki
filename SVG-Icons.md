#### index
- [Using built-in Themes](/ghiscoding/Angular-Slickgrid/wiki/SVG-Icons#using-built-in-themes)
- [Using SVG with SASS](/ghiscoding/Angular-Slickgrid/wiki/SVG-Icons#using-custom-svgs-with-sass)
- [How to change SVG color?](/ghiscoding/Angular-Slickgrid/wiki/SVG-Icons#how-to-change-svg-color)


### Description
Angular-Slickgrid was built with a Font set, mainly Font-Awesome 4, and if you use SASS it was easy enough to replace Font-Awesome to any other Font based set. The question is how do we use SVG instead of a Font? Most frameworks are switching to SVGs instead of Fonts (for smaller size and also efficiency). Angular-Slickgrid now has 2/3 Styling Themes that support SVGs which are Material Design & Salesforce Themes. These 2 new Themes use a subset of [Material Design Icons](https://materialdesignicons.com/) SVGs (even a portion of the Salesforce theme). There are no Font-Awesome 5, I wouldn't mind adding a new Theme for that and if you wish to contribute then please open a new issue.

If you use SASS, you will find out that it's super easy to use either (Font) or (SVG), you simply have to replace the SASS necessary variables, more on that later.

### Demo
- Material Theme - [demo](https://ghiscoding.github.io/Angular-Slickgrid/#/tree-data-parent-child)
- Salesforce Theme - [demo](https://ghiscoding.github.io/Angular-Slickgrid/#/tree-data-hierarchical)

### Using built-in Themes
The Material & Salesforce Themes are now using SVGs for the icons used by the grid. Each built-in Themes have CSS and SASS files associated with each theme. To take benefit of this, just import whichever CSS/SASS file associated with the Theme you wish to use.

##### with CSS
```scss
/* style.css */
@import 'angular-slickgrid/styles/css/slickgrid-theme-bootstrap.css';

// or other Themes
// @import 'angular-slickgrid/styles/css/slickgrid-theme-material.css';
// @import 'angular-slickgrid/styles/css/slickgrid-theme-salesforce.css';
```

##### with SASS
```scss
/* change any SASS variables before loading the theme */
$primary-color: green;

/* style.scss */
@import 'angular-slickgrid/styles/sass/slickgrid-theme-bootstrap.scss';

// or other Themes
// @import 'angular-slickgrid/styles/sass/slickgrid-theme-material.scss';
// @import 'angular-slickgrid/styles/sass/slickgrid-theme-salesforce.scss';
```

### Using Custom SVGs with SASS
You could use Custom SVGs and create your own Theme and/or a different set of SVG Icons, each of the icons used in Angular-Slickgrid has an associated SASS variables which allow you to override any one of them. All grid of the icons are loaded with the `content` property of the `:before` pseudo (for example, see this [line](https://github.com/ghiscoding/Angular-Slickgrid/blob/master/src/app/modules/angular-slickgrid/styles/slick-bootstrap.scss#L323)) and the difference between Font and SVG is simple, if you want to use a Font then you use the Font unicode but if you want an SVG then you use a `url` with `svg+xml` as shown below. 

##### with Font
```scss
$primary-color: blue;
$icon-sort-color:       $primary-color
$icon-sort-asc:         "\f0d8";
$icon-sort-desc:        "\f0d7";
$icon-sort-font-size:   13px;
$icon-sort-width:       13px;

// then on the last line, import the Theme that you wish to override
@import 'angular-slickgrid/styles/sass/slickgrid-theme-bootstrap.scss';
```

##### with SVG
```scss
// a simple utility that will encode a color with hash sign into a valid HTML URL (e.g. #red => %23red)
// you could also skip the use of this and simply manually change # symbol to %23
@import 'angular-slickgrid/styles/sass/sass-utilities';

$primary-color: blue;

$icon-sort-color:       $primary-color
$icon-sort-asc:         url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" fill="#{encodecolor($icon-sort-color)}" viewBox="0 0 24 24"><path d="M13,20H11V8L5.5,13.5L4.08,12.08L12,4.16L19.92,12.08L18.5,13.5L13,8V20Z"></path></svg>');
$icon-sort-desc:        url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" fill="#{encodecolor($icon-sort-color)}" viewBox="0 0 24 24"><path d="M11,4H13V16L18.5,10.5L19.92,11.92L12,19.84L4.08,11.92L5.5,10.5L11,16V4Z"></path></svg>');
$icon-sort-font-size:   13px;
$icon-sort-width:       13px;

// then on the last line, import the Theme that you wish to override
@import 'angular-slickgrid/styles/sass/slickgrid-theme-material.scss';
```

### How to change SVG color?
#### The following works for both CSS and SASS
So there's a known caveat with using embedded SVG (which is what this lib does), you can only add color once with the `fill` property and for that I added SASS variables for each icon (for example `$icon-sort-color` for the Sort icon color, or `$icon-color` for all icons) but once you do that it will apply that same color across the document (want it or not).

##### for CSS and SASS
After lot of research, I found a way to hack it via this SO answer [change any SVGs color via CSS `filter`](https://stackoverflow.com/a/53336754/1212166), for example if we wish to apply a red color on the `mdi-close` icon (for the Draggable Grouping feat), we'll have to do this `filter`
```css
.slick-groupby-remove.mdi-close {
  /* close icon - red */
  filter: invert(32%) sepia(96%) saturate(7466%) hue-rotate(356deg) brightness(97%) contrast(120%);
}
```
You might be thinking, like I did, but how to get this long `filter` calculation???
For that you can visit the following blog post and use its [converter](https://dev.to/jsm91/css-filter-generator-to-convert-from-black-to-target-hex-color-188h) that was posted to answer this SO [question](https://stackoverflow.com/q/42966641/1212166)

##### for SASS only
There is also a SASS Mixin to convert the color using only SASS as posted [here](https://stackoverflow.com/a/62880368/1212166) in the same SO question. That will be part of the lib soon enough and we'll be able to use it this way (much cleaner):
```scss
.slick-groupby-remove.mdi-close {
  /* close icon - red */
  @include recolor(#ff0000, 0.8);
}
```
Note that even though the code looks smaller and more human readable, in reality the code produced will still be a `filter`

`@include recolor(#ff0000, 0.8);` will in fact be converted to `filter: invert(32%) sepia(96%) saturate(7466%) hue-rotate(356deg) brightness(97%) contrast(120%);`