# Introduction

The **SVG Icon Loader** provides a method for a web interface to use SVG images as icons, while being loaded from a single file. This reduces the amount of HTTP requests, offering the same kind of benefit available with CSS sprites. If your server allows gzipping on SVG files, the file download is made a lot smaller since SVG files compress very well. This is a modification of [svg-icon-loader](https://code.google.com/p/svg-icon-loader/) from google code.

[Home Page](http://cuixiping.github.io/svg-icon-loader/)

# Support

Successfully uses SVG images in the following browsers: 

*   Firefox 1.5+*   Opera 9.6+*   Safari 3+*   Chrome 2+*   IE 6+ (with the Chrome Frame plugin)

In older browsers fallback images will be used instead (if specified) 

# Demo

[demo 1 (svg)](http://cuixiping.github.io/svg-icon-loader/demo.html) &nbsp;|&nbsp;  [demo 2 (svgz)](http://cuixiping.github.io/svg-icon-loader/demo2.html)

# Script

The latest stable version (2.1) is available. [minified(4.8k)](http://cuixiping.github.io/svg-icon-loader/jquery.svgicons-2.1.min.js) or [gzipped(2.1k)](http://cuixiping.github.io/svg-icon-loader/jquery.svgicons-2.1.min.js.gz) or [uncompressed(11.1k)](http://cuixiping.github.io/svg-icon-loader/jquery.svgicons.js) version.

# Using the script

1. Create the SVG master file that includes all icons: 

The master SVG icon-containing file is an SVG file that contains  <tt>&lt;g&gt;</tt> elements. Each <tt>&lt;g&gt;</tt> element should contain the markup of an SVG icon. The <tt>&lt;g&gt;</tt> element has an ID that should  correspond with the ID of the HTML element used on the page that should contain  or optionally be replaced by the icon. Additionally, one empty element should be added at the end with id "svg_eof". 

2. Optionally create fallback raster images for each SVG icon. 

3. Include the jQuery and the SVG Icon Loader scripts on your page. 

4. Run `$.svgIcons()` when the document is ready: 

   `$.svgIcons( file string, options literal);`

File is the location of a local SVG or SVGz file. 

All options are optional and can include: 

*   `'w (number)'`: The icon widths (default: 24px)

*   `'h (number)'`: The icon heights (default: 24px)

*   `'fallback (object literal)'`: List of raster images with each key being the SVG icon ID to replace, and the value the image file name.

*   `'fallback_path (string)'`: The path to use for all images listed under "fallback"	 

*   `'replace (boolean)'`: If set to true, HTML elements will be replaced by, rather than include the SVG icon.

*   `'placement (object literal)'`: List with selectors for keys and SVG icon ids as values. This provides a custom method of adding icons.

*   `'callback (function)'`: A function to call when all icons have been loaded

*   `'id_match (boolean)'`: Automatically attempt to match SVG icon ids with corresponding HTML id (default: true)

*   `'no_img (boolean)'`: Prevent attempting to convert the icon into an <tt>&lt;img&gt;</tt> element (may be faster, help for browser consistency)

*   `'svgz (boolean)'`: Indicate that the file is an SVGZ file, and thus not to parse as XML. SVGZ files add compression benefits, but getting data from them fails in Firefox 2 and older.

5. To access an icon at a later point without using the callback, use this: 
> `$.getSvgIcon(id (string))`. This will return the icon (as jQuery object) with a given ID.

6. To resize icons at a later point without using the callback, use this: 
> `$.resizeSvgIcons(resizeOptions)` (use the same way as the "resize" parameter)

## Example usage #1
```javascript
$(function() {
    $.svgIcons('my_icon_set.svg'); // The SVG file that contains all icons
    // No options have been set, so all icons will automatically be inserted 
    // into HTML elements that match the same IDs. 
});
```

## Example usage #2
```javascript
$(function() {
    $.svgIcons('my_icon_set.svg', { // The SVG file that contains all icons
        callback: function(icons) { // Custom callback function that sets click
                        // events for each icon
            $.each(icons, function(id, icon) {
                icon.click(function() {
                    alert('You clicked on the icon with id ' + id);
                });
            });
        }
    }); //The SVG file that contains all icons
});
```

## Example usage #3
```javascript
$(function() {
    $.svgIcons('my_icon_set.svgz', { // The SVGZ file that contains all icons
        w: 32,  // All icons will be 32px wide
        h: 32,  // All icons will be 32px high
        fallback_path: 'icons/',  // All fallback files can be found here
        fallback: {
            '#open_icon': 'open.png',  // The "open.png" will be appended to the
                           // HTML element with ID "open_icon"
            '#close_icon': 'close.png',
            '#save_icon': 'save.png'
        },
        placement: {'.open_icon','open'}, // The "open" icon will be added
                          // to all elements with class "open_icon"
        callback: function(icons) {
            icons['open']
                .width(64)
                .height(64); // Sets different width/height for "open" icon 
        },
        svgz: true // Indicates that an SVGZ file is being used
    })
});
```
