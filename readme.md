#Layer Exporter for Adobe Illustrator



![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/banner.png)

Layer Exporter lets you automatically export Illustrator layers to SVG, JPG or PNG files, along with coordinates data and HTML CSS files, with a single click. Think of it as a modest equivalent of [Photoshop Asset Generator](https://github.com/adobe-photoshop/generator-assets) for Illustrator, with full HTML & CSS export.


It's a free & open source extension panel for Adobe Illustrator CC, made with the [creative cloud extensiblity helpers](http://davidderaedt.github.io/ccext-website/).

##Install

CC extensions can either be installed via the built-in *Adobe Exchange Panel* (preferred method), or via a ZXP package executed in *Extension Manager CC* (which you must install first via the *Creative Cloud Desktop* application).

Open Illustrator and choose `Window > extensions > Adobe Echange` to open the exchange panel, look for "Layer exporter" and you should find it directly.

![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/exchange-workflow.png)

Alternatively, you can download and install the current [production ZXP](https://github.com/davidderaedt/Illustrator-Layer-Exporter/raw/master/bin/LayerExporter.zxp) (stable) or the [development ZXP](https://github.com/davidderaedt/Illustrator-Layer-Exporter/raw/master/bin/LayerExporter-dev.zxp) (use with caution) for use with *Extension Manager CC*.



##Exporting Images 

This panel uses naming conventions to create the JPG, PNG or SVG files from your artwork. For instance, to export the content of your "Logo" layer, simply rename it to "Logo.png", and your `Logo.png` file will be created in your destination folder.

![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/layers-panel.png)

Once you click the *Export* button, it will parse both layers and individual art items contained in those layers. All layers or items suffixed with a supported image file format will be created, unless it was made invisible. However, please note that only top level layers will be used, as *sub-layers will be ignored*.

The *Options* panel will let you specify globally image format options such as JPG quality, or whether or not SVG should outline text. Contrary to Photoshop Generator, you cannot set those options for individual image files.

![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/p2.png)


###Exporting Layers

If you have a big file with many layers, renaming all those layers can be somewhat tedious. To help you with that renaming step, you can use the *Tools* panel to automatically rename all your layers so that they get exported to the appropriate file format.

> Choosing *None* will simply remove all suffixes from layer names

![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/p1.png)


###Exporting art items

All art items (such as paths, texts and groups) inside layers can also be suffixed to be exported to image files, even if its parent layer has itself been renamed to be exported as an image file.

You can automatically set the destination format for the items you selected by clicking the appropriate button in the *Tools* panel.

You can also use the *Move selected items* options to automatically create top level layers from those items. Choose *One Layer* to gather all selected items to the same new Layer. Alternatively, choose *Multiple Layers* to create as many layers as the selected items, each being located in its own layer.



##Exporting composition data

If you want to use exported image files somewhere (say, in a webpage or a native app), chances are you're going to need to get the coordinates of those elements in order to recreate the original composition.

There several options to help you with that.


###Exporting to HTML


If you want to export your whole composition to HTML, choose the corresponding option to generate an HTML file in the parent folder of destination folder for the images.

You can choose to have CSS styles declared inside a `<style>` tag in the `<head>` of this HTML page, or to create a separate CSS file.

Layer Exporter will then use HTML tags to describe the composition, starting with a top level *div* named after the Illustrator file. Inside this *div*, all exported Layers will be converted to *img* tags.

Any layer that is not suffixed with a supported image file will be considered as a child *div* of the main *div*. All art items inside this layer suffixed with a supported image file format will be considered as a child *img* tag of the parent *div*.

![image](https://raw.githubusercontent.com/davidderaedt/Illustrator-Layer-Exporter/master/pics/web.png)

###Experimental HTML features

We're working on some additional features to help with HTML export. Since those features are experimental, they are likely to change in the future. Also, things will break.

* Text: text items suffixed with *=text* will be converted to full HTML text with the corresponding CSS styles (font family, font size, color and alignment).
* Div backgrounds: inside layers, path items named "#bgd" will be ignored but used to determine the background color and opacity of the parent *div*.


###Edge Animate project

Layer Exporter has limited support for Adobe's *Edge Animate* Projects.

Since *Edge Animate* supports HTML as input, you could simply open it inside the tool, but you'd then be missing a few capabilities, such as the ability to group or create symbol.

The *Import to Edge Animate* option lets you choose an existing .an file and will overwrite it with the corresponding data. A typical workflow would then consist of first creating a empty .an project, importing the illustrator composition via the *Import to Edge Animate* option, and then updating artwork as needed from Illustrator.

###Exporting to other contexts


Alternatively, you can choose to create a dump of the raw data of the composition so that you, a developer, or a 3rd party tool can interpret the data . Choose the `Create JSON data file` to create a JSON representation of the composition inside the image destination folder.




##Release notes

V 2.0.1 - April 7th 2014

> WARNING: this release has major functional differences with previous builds. You may have to reorganize your art to conform to the new model.

* You can now also separately export art items inside layers. By default, layers are no longer exported as images and are treated as divs in HTML. Sub layers are not supported.
* Art items named "#bgd" inside layers are ignored but used to set the background color & opacity of the parent div.
* Elements suffixed with "=text" inside layers are exported as text and converted to \<p\> tags in HTML.
* Option to generate separate CSS files for HTML export
* Option to generate the data.json file 
* New Helper tools to automate layer and items suffixes

V 1.5.0 - March 11th 2014

* Generate HTML pages including all generated assets (including CSS)
* Import to Edge Animate
* New Helper tools to create layers from path items 
* New UI based on topcoat (Regression: host theme support is removed in this build)
* Bug Fixes

V 1.0 - June 2013

* Generate JPG, PNG & SVG via layer names
* Options to define the quality of JPG and SVG text handling
* data.json file describes the scene graph for 3rd party tools


##Known issues

* Sublayers are not supported. Rationale: a recursive process could be used, but an Illustrator bug prevents from knowing the relative zorder of layers vs art items, making it impossible to create a proper representation of the whole composition.
* Special items (divs backgrounds and texts) are not currently supported in edge animate export
* Layer names cannot be set with the same UX as with art items (eg "set selected layers as PNG"). Rationale: the illustrator API does not let us know which layers are selected.



##About the source code

This repository is the source for the panel. The underlying logic, written in ExtendScript, is located in the CSscript repository.