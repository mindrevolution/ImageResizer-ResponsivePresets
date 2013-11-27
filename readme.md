# DEPRECATED!
## Look here for new version: [Slimmage+Slimresponse](https://github.com/imazen/slimmage)

# ResponsivePresets for ImageResizer.Net
**Effortless reponsive images powered by ImageResizer.Net**


##Introduction
Licensed under the [MIT Licence](http://www.opensource.org/licenses/mit-license.php), which allows open source as well as commercial use.

###What does it to?
* Converts image tags using [ImageResizer presets](http://imageresizing.net/plugins/presets) to picture tags (as currently proposed by the [Responsive Images Community Group](http://www.w3.org/community/respimg/). 
* Varies [ImageResizer preset names](http://imageresizing.net/plugins/presets) by media-queries and device pixel ratio for automatic image resizing according to responsive webdesign practices.


### Requirements
* ASP.Net 4.0
* [ImageResizer.Net](www.imageresizing.net) 3.1.5+ by Nathanael Jones
* [picturefill.js](https://github.com/Wilto/picturefill-proposal) by Mat Marquis (a polyfill for proposed behavior of the picture element)

### Spin it up quickly for a test!
* Use WebMatrix and fire it up on the "www"-folder, that's all!

----

## How to use

ResponsivePresets extends the presets that are available with ImageResizer. **With a simple dot-notation (of the preset name), you can enable screen resolution aware dynamic resizing for some - or all - of your images.**

**Tipp**
>You can run the www-folder in WebMatrix right after cloning the repo, as the assembly binaries are included for easy testing.

### 1. Enable ResponsivePresets

ResponsivePresets works in team with the standard Presets plugin for ImageResizer, so you need to enable and configure that as well. Deploy the assembly and activate the plugin:
* Copy ResponsivePresets.dll to /bin/
* Copy HtmlAgilityPack.dll to /bin/

Install the HttpModule that activates the request filter which transforms img tags with an dot-notation preset to full blown picture elements in `<system.webServer>`, `<modules>`:

```xml
<add name="ResponsivePresetsModule" type="ResponsivePresets.FilterModule.ActivateFilterModule, ResponsivePresets"/>
```

For a working example see web.config and ImageResizer.config in /www/

### 2. Segment device screen resolutions
The concept for ResponsivePresets is to group screen resolutions in classes that get prefixed to the base preset name. Define media queries for each prefix.

```xml
<responsivepresets respectPixelDensity="true">
	<preset prefix="mobile" />
	<preset prefix="desktop" media="(min-width: 960px)" default="true" />
	<preset prefix="hires" media="(min-width: 2000px)" />
</responsivepresets>
```

The attribute *default* set, defines the preset used for fallback images (noscript).

**Tipp: px or em?**
> If you want to also generate image variants optimized for hires-displays (like 4k, or at least "Retina"), it's advisable to use _em_ units and not _px_ units in your media queries.
 
You are free to segment and structure to your liking, this allows ResponsivePresets to adapt to almost any requirement.

### 3. Configure presets
Add a preset to your config, like you would normally do for the presets plugin:
    
```xml
<preset name="galleryimage" settings="width=800" />
```

If this setting is i.e. for standard desktop devices, prefix the preset name with *desktop.* like so:
    
```xml
<preset name="desktop.galleryimage" settings="width=800" />
```

Add presets for all the other prefixes (device segments)
    
```xml
<preset name="mobile.galleryimage" settings="width=400" />
<preset name="desktop.galleryimage" settings="width=800" />
<preset name="hires.galleryimage" settings="width=1600" />
```
### 4. Enable "respectPixelDensity"-Setting
If set to "true", ResponsivePresets will generate srcset values with 1x, 2x, 3x and 4x variants of the images by [zooming](http://imageresizing.net/docs/reference) the images accordingly.


### 5. Prepare your markup
* Reference the (minified) [polyfill javascript](https://github.com/Wilto/picturefill-proposal):

```html
	<script src="/picturefill.js"></script>
```

* If the pull request with my fix is not already merged, [use this corrected version of the polyfill](https://github.com/Wilto/picturefill-proposal/pull/2):

* Adjust image source attribute: Instead of adding a fixed preset name to the image's request string, write the base preset name and prepend a dot to indicate you want this image to be extended by ResponsivePresets:

```html
	<img src="sample.jpg?preset=.galleryimage" alt="sample image"/>
```

----

##Notes
Your stylesheet has to take care to resize the images to the desired dimensions, they now greatly vary depending on the client's screen resolution and pixel density.

Consider to also include [matchMedia](https://github.com/paulirish/matchMedia.js/).

Have a look at the [picturefill compatibility chart](https://github.com/Wilto/picturefill-proposal#support).



----

## Limitations

#### Polyfill vs. browser implementation
>While polyfilling does work, using it in real world sites before a major browser implements the picture tag has to be well-considered.

----

Follow [esn303](https://twitter.com/#!/esn303) on twitter

This project is open to collaboration. **Fork. Push. Innovate.**
