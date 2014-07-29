jQMultiTouch
============

## The jQuery of Multi-touch

jQMultiTouch is a lightweight toolkit and development framework for multi-touch web interface development. Not only is it similar to jQuery in terms of the idea of providing a lightweight and general framework, but also because of the fact that it adopts many key ideas from jQuery and applies them to the framework's core concepts.
			The main ideas behind jQMultiTouch and its core features are described [in this paper](http://www.globis.ethz.ch/publications/?action=show_abstract&pubid=346) presented at the [EICS 2012](http://eics-conference.org/2012) conference.

jQMetrics was originally created by in the [Global Information Systems Group](http://www.globis.ethz.ch) at ETH Zurich. It is written and maintained by [Michael Nebeling](http://www.michael-nebeling.de). It is available as free open-source software distributed under the GPLv3 license. See the file LICENSE for more information.

Should you have any questions or comments, please feel free to [send an email to Michael Nebeling](mailto:michael.nebeling@gmail.com).

**Note: Please cite the [EICS 2012 Paper](http://dl.acm.org/citation.cfm?id=2305497) if you are using jQMultiTouch in your academic projects.**

## Features
				
* **Support for Firefox and WebKit Browsers**:  One of the core features of jQMultiTouch is that it supports Firefox and WebKit-based browsers such as Safari. As a result, cross-browser compatibilty issues between different touch event models and data are resolved and touch events can be handled in a uniform way independent of browser-specific implementations.
* **Default Multi-Touch Gesture Handlers**:  jQMultiTouch provides support for custom touch and gesture handlers, but also offers default implementations to enable common dragging, scaling and rotation operations in supported browsers.
* **Touch History Query and Evaluation Mechanisms**:  One of the core components of jQMultiTouch is the touch event buffer or history. The touch history keeps a record of current and previous touches, and can be filtered by all touch-related properties, e.g. touch id (i.e. finger), touch event, target element and timestamp. This is important for gestures that may involve several fingers, a series of touch down, move and up events as well as multiple target elements within a certain period of time.
* **Gesture Templates**: To make working with the touch history easier for developers, jQMultiTouch provides several methods for querying and filtering the entries according to a combination of criteria. These can for example be used to detect simple swipe left/right gestures.
* **Touch Support**: jQMultiTouch also provides a cross-browser compatible method to find out whether touch events are supported by the device and browser in use.
* **Device Orientation**: In addition, jQMultiTouch provides a simple way of reacting to changes in the orientation of the device.

## Usage

jQMultiTouch needs to be included using the following statements:

```html
<!-- include jQuery (required) -->
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>

<!-- include jQMultiTouch core (required) -->
<script src="../js/jquery.multitouch.js" type="text/javascript"></script>

<!-- include jQMultiTouch attachable behaviours (optional) -->
<script src="../js/jquery.multitouch-draggable.js" type="text/javascript"></script>
<script src="../js/jquery.multitouch-scalable.js" type="text/javascript"></script>
<script src="../js/jquery.multitouch-resizable.js" type="text/javascript"></script>
<script src="../js/jquery.multitouch-rotatable.js" type="text/javascript"></script>

<!-- include jQMultiTouch stylesheet (optional) -->
<link href="../css/jquery.multitouch.css" rel="stylesheet" type="text/css">
```

jQMultiTouch requires jQuery (compatible with versions 1.4.2 or higher) and the core (`js/jquery.multitouch.js`) to be included in the document. The attachable behaviours and the stylesheet are optional.

### Attachable Multi-touch Behaviours

The core multi-touch behaviour in jQMultiTouch is `touchable`.
In addition, jQMultiTouch implements four attachable multi-touch behaviours:

* [Draggable Behaviour](js/jquery.multitouch-draggable.js): Translate touchable elements using drag and drop gesture
* [Scalable Behaviour](js/jquery.multitouch-scalable.js): Zoom touchable elements using pinch gestures
* [Resizable Behaviour](js/jquery.multitouch-resizable.js): Change size of touchable elements using pinch gestures
* [Rotatable Behaviour](js/jquery.multitouch-rotatable.js): Rotate touchable elements

**Note that all `draggable`, `scalable`, `resizable` and `rotatable` elements automatically become `touchable` elements.**

**Also note that the `scalable` and `resizable` behaviour cannot be attached to the same element at the same time.**

Below you can find an example taken from the [Picture Viewing Application](examples/pictures.html). The script turns all images in a web page into `draggable`, `scalable` and `rotatable`elements.

```javascript
$(img)
  .draggable()
  .scalable({ minScale: 0.5, maxScale: 1.5 })
  .rotatable();
```

#### Touchable Behaviour

The `touchable` behaviour and its defaults are defined as follows:

```javascript
$.touch.touchable = {
    // callback when a new touch is registered, return true to contiue, false to cancel
    touchDown: function(e, touchHistory) { return true },
    // callback when touch is moving, return true to contiue, false to cancel
    touchMove: function(e, touchHistory) {return true },
    // callback when touch is released, return true to contiue, false to cancel
    touchUp: function(e, touchHistory) {return true },
    // custom gesture callback
    gesture: function(e, touchHistory) { return false },
    // register gesture templates
    gestureCallbacks: [],
    // default touchable handler, override for custom touchable behaviour
    touchableHandler: function(e, touchHistory) { },
}
```

The default `touchableHandler` adds the CSS class `touched` on `touchDown` and removes it on `touchUp`.

When the `gesture` event is triggered and the `gesture` callback does not return `false`, it will subsequently evaluate all templates registered under `gestureCallbacks` and, for each template, execute the callback defined for the template.

#### Draggable Behaviour

The `draggable` behaviour and its defaults are defined as follows:

```javascript
$.touch.draggable: {
    // perform dragging according to x/y grid by flooring target coordinates
    grid: [1, 1],			
    // set to true to constrain drag operation to parent
    constrainParent: false,
    // before dragging callback, return true to contiue, false to cancel
    dragBefore: function(e) { return true },
    // during dragging callback, return true to contiue, false to cancel
    dragDuring: function(e) { return true },
    // after dragging callback, return true to contiue, false to cancel
    dragAfter: function(e) { return true },
    // default draggable handler, override for custom dragging behaviour
    draggableHandler: function(e, touchHistory) { ... }
}
```

#### Scalable Behaviour

The `scalable` behaviour and its defaults are defined as follows:

```javascript
$.touch.scalable: {
    minScale: false,
    maxScale: false,
    scaleBefore: function(e) {},
    scaleDuring: function(e) {},
    scaleAfter: function(e) {},
    scalableHandler: function(e, touchHistory) { ... },
}
```

#### Resizable Behaviour

The `resizable` behaviour and its defaults are defined as follows:

```javascript
$.touch.resizable: {
    minWidth: false,
    maxWidth: false,
    minHeight: false,
    maxHeight: false,
    resizeBefore: function(e) {},
    resizeDuring: function(e) {},
    resizeAfter: function(e) {},
    resizableHandler: function(touchEvent, touchHistory) { ... },
}
```

#### Rotatable Behaviour

The `rotatable` behaviour and its defaults are defined as follows:

```javascript
$.touch.rotatable: {
    step: 1, // TODO: not implemented yet
    rotateBefore: function(e) { return true },
    rotateDuring: function(e) { return true },
    rotateAfter: function(e) { return true },
    rotatableHandler: function(touchEvent, touchHistory) {
}
```

#### Orientable Behaviour

In addition, jQMultiTouch implements the [Orientable Behaviour](js/jquery.multitouch-orientable.js) a simple way to react to changes in orientation.

Example:
```javascript
$.touch.orientationChanged(function (orientation) {
    if (Math.abs(orientation) === 90) {
        alert('landscape');
    } else if (orientation === 0) {
        alert('portrait');
    }
});
```
The default handler will automatically show/hide `orientable` elements marked with the following classes according to the orientation.

* `.portrait`: Only visible in portrait mode
* `.landscape`: Only visible in landscape mode (if device was rotated to the right)
* `.landscape-90`: Only visible in landscape mode (if device was rotated to the left)

See the [Orientation Demo](examples/orientation.html) for an example.

### Custom Gesture Handlers

```javascript
$.touch.ready(function() {
  $(element).touchable({
    gesture: function(e, th) { // custom gesture handler
      th.start({ type: 'touchup' }).stop({ type: 'touchdown' }); // segment touch history
      if (th.filter({ finger: 0, time: '<100' }).match({ deltaX: '<-100' })) { // constrain touch history
        // swipe left detected
      }
    }
  });
});
```

See the [Gestures Demo](examples/gestures.html) for an example.

### Other Features

jQMultiTouch's core defines the `$.touch` environment and with it the following features and options:

* `$.touch.preventDefault = true/false`: Can be used to enable default browser behaviour (default is true, i.e. to prevent default behaviour)

* `$.touch.triggerMouseEvents = true/false`: Can be set to true to simulate touch events, e.g. for legacy support and testing on non-touch devices (default is false, i.e. to only react to touch events)

* `$.touch.enabled(): true/false`: Can be used to check whether touch is available on the device

* `$.touch.ready(function): true/false`: Can be used to register a `ready` function that will be executed when the document is ready, provided that touch is available (see `$.touch.enabled()`) or `$.touch.triggerMouseEvents` is set to `true`

* `$.fn.touches`: Returns true if more than one touch was registered on the DOM element, e.g.
```javascript
if ($(element).touches().length > 1) {
    // more than one finger is touching the element
}
```

* `$.touch.allTouches = {}`: Keeps a record of all active touches, e.g.
```javascript
$.each($.touch.allTouches, function() {
    // log all elements that are currently being touched
    console.log(this.target);
});
```

Note that a touch event `e` is defined as follows:

```javascript
var e = {
    // touch object that triggered the event
    touch: touch,
    // target element of touch event
    target: touch.target,
    // x position of touch point within document
    clientX: touch.clientX,
    // y position of touch point within document
    clientY: touch.clientY,
    // type of touch event
    type: touchdown|touchmove|touchup|gesture,
    // timestamp when event was triggered
    time: $.now(),
}
```

* `$.touch.history = new TouchHistory([])`: Keeps a record of the entire touch history, can be useful for global analysis

* `$.touch.historyMaxSize = 64`: Sets the maximum size of the touch event buffer (default is 64)

* `$.touch.historyFilterPredicates = {}`: Defines the predicates that can be used in `TouchHistory.filter()` and `TouchHistory.find()`, e.g. `type`, `target`, `touch`, `finger` and `time`

* `$.touch.historyMatchPredicates = {}`: Defines the predicates that can be used in `TouchHistory.match()`, e.g. `clientX`, `clientY`, `deltaX`, `deltaY`, `netX` and `netY`

* `$.touch.noConflict(): true/false`: Can be used for conflict resolution of jQMultiTouch's `$.touch` environment

See [jQMultiTouch Core](js/jquery.multitouch.js) for details.

## Examples

**The following applications are simple examples that demonstrate the general use of the framework. They have been designed to illustrate the basic concepts of the framework and how they could be applied in applications.**

* [Behaviours Demo](examples/behaviours.html): Simple demonstrator of jQMultiTouch's core and attachable multi-touch behaviours
* [Orienation Demo](examples/orientation.html): Shows jQMultiTouch's support for handling device orientation
* [Gestures Demo](examples/gestures.html): Shows jQMultiTouch's support for custom multi-touch gestures
* [$1 Demo](examples/dollar.html): Demonstrates the integration with [$1 Unistroke Recognizer](http://depts.washington.edu/aimgroup/proj/dollar) and jQMultiTouch for stroke-based gesture recognition
* [Simple Line Drawing Application](examples/draw.html): Simple application for drawing on a HTML5 canvas using one or more fingers, shows custom and browser-specific touch event handlers
* [Picture Viewing Application](examples/pictures.html): Simple picture viewing application that shows jQMultiTouch's support for attachable behaviours for dragging, scaling and rotating elements
* [CNN Application](examples/cnn.html): Simple prototype of the CNN web site using simple swipe gestures to play around with the transition between screens

## FAQ

**Q: What is jQMultiTouch?**

*A: jQMultiTouch is a development framework for multi-touch web interfaces. It is based on jQuery and adopts some of its core concepts with the goal of providing a uniform method for implementing multi-device web applications with support for multi-touch interactions. This is a challenge due to the increased diversity of touch-enabled devices ranging not only from the iPhone/iPod touch and iPad to Android phones and tablets, but nowadays also including laptops and all-in-one desktop computers with support for touch input. It currently supports Firefox and WebKit-based browsers, and provides different methods for the recognotion and implementation of gesture-based interactions.*

**Q: How is it different from jQuery Mobile, jQTouch and Sencha Touch?**

*A: jQMultiTouch does not have the specific focus on mobile web application development and also does not provide UI widgets and components that mimic the look of native controls. The idea behind jQMultiTouch is to provide a general framework for multi-touch web interface development and to address some of the core challenges such as tracking multiple input points on one or more web page elements at a time, supporting multi-touch event capture and delegation and implementing custom gesture handlers.*

**Q: Can I use jQMultiTouch in my projects?**

*A: Yes, please fork the [GitHub repository](https://github.com/michaelnebeling/jqmultitouch) and go ahead. jQMultiTouch has been released as free open-source software distributed under the GPLv3 (GNU Public License).*

**Q: Why doesn't it work on my touch device?**

*A: We know that compatibility with some devices is still a problem (e.g. Windows Phone and Windows 8 touch devices are not yet supported), and are working on fixing the issues.*

## About

jQMultiTouch was originally created by [Michael Nebeling](http://www.michael-nebeling.de) in the [Global Information Systems Group](http://www.globis.ethz.ch) at ETH Zurich. It also contains contributions from Saiganesh Swaminathan, Maximilian Speicher, Martin Grubinger, Maria Husmann and Yassin Hassan. It is available as free open-source software distributed under the GPLv3 license.

Should you have any questions or comments, please feel free to [send an email to Michael Nebeling](mailto:michael.nebeling@gmail.com).