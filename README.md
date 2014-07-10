wcDocker (Web Cabin Docker) is a page layout framework that gives you dynamic docking windows.  Windows can float on their own or be docked; docking can be on either side, the bottom, or even nested within each other.  All windows can be moved, resized, removed, and created at will by the user.  This project is currently under development by Jeff Houde (lochemage@gmail.com).  wcDocker requires the JQuery library, currently developed under 2.1.1 although much earlier versions should work fine.

For a currently working demo, try it here [http://webcabin.org/test](http://webcabin.org/test)

### The following features are supported currently: ###

* Windows can be docked to either side, the bottom, or nested within each other.
* Windows can be detached from a dock position and float on their own.
* Windows can be resized.
* Windows can be closed via close button.
* Windows can be initialized in code for a default layout.
* Windows can be programmed with a minimum size constraint.
* Multiple windows with the same contents can be created.


### The following features are still under development: ###

* Windows can be created via context menu.
* Ability to group multiple windows into the same dock panel via tabbing.
* Save/Restore support for remembering a user's window setup.
* Dynamic update support to help optimize refreshing window contents when another window's change affects it.
* An option to enable/disable closing of all or individual window types.
* A possible option of restricting the number of total windows created.
* A possible option of restricting the total copies of the same window type.

****

# Usage #


Begin by creating an instance of the main window and assigning it a container JQuery DOM element.
Typically this would be the document body, but there is no restriction if you want to use a
smaller area instead.  Multiple main windows can be used, however, no support exists for
cross interaction between them (yet?).  Also note that floating windows are not constrained to
the given container element, they can float anywhere in the browser window.
```
#!javascript
var wcWindow = new wcMainWindow($('body'));
```
To access the central widget (our main window view that is always present).
First retrieve the central widget, which is a layout.

```
#!javascript
var layout = wcWindow.center();
```
The central widget, as well as all docking windows contain a layout;
The layout is the container where all your window content will be placed.
Each layout is a table grid that is dynamically resized based on content
given. To add a DOM element, add it into the layout with an x, y grid
location. You can also optionally include width and height values if
you wish to stretch an element over multiple cells.
```
#!javascript
layout.addItem($dom, x, y, width, height);
```
All dock windows have a type that must be registered first before use, this allows
the main window to know what types of dock windows are available and allow the user
to add and remove them at will.  To register a new type, you will need a unique name
to identify it and a function callback which allows you to setup the window when
it is created.
```
#!javascript
wcWindow.registerDockWidgetType('Some type name', function(widget) {
```
Inside the callback function you are given the windows widget, from here you can
access the layout and populate it the same as done with the central widget.
```
#!javascript
  widget.layout().addItem($dom, x, y, width, height);
```
You can also set various properties of the widget here as well, such as
the desired size, or the minimum size.  More properties will become available
as development continues.
```
#!javascript
  widget.size(200, 200);
  widget.minSize(100, 100);
});
```
Once you have registered one or more dock window types, you can initialize
an initial window configuration (Note that the user has full access to add,
remove, or re-arrange these windows).

To add a new dock window, give it a valid pre-registered type name and a
desired destination for placement.  The destination can be one of either:

wcGLOBALS.DOCK_LOC.FLOAT    = Make a floating window that is not docked.

wcGLOBALS.DOCK_LOC.LEFT     = Dock it to the left side of the window.

wcGLOBALS.DOCK_LOC.RIGHT    = Dock it to the right side of the window.

wcGLOBALS.DOCK_LOC.BOTTOM   = Dock it on the bottom of the window.

The third parameter determines whether this window is allowed to group
up with another already existing window in a tabbed view (currently, tabbed
frames are not supported so there will be no way to switch the widget
being viewed) or if the new window should appear standalone creating
another window panel.

The final parameter is optional, normally docked windows will dock onto
a side of the main window's central widget. However, by supplying a
specific widget, your new window will dock onto a side of that widget
instead.
```
#!javascript
wcWindow.addDockWidget('Some type name', wcGLOBALS.DOCK_LOC.LEFT, false, optionalTargetWidget);
```
addDockWidget also returns you the newly created dock widget item, in the
case that you may want it.