---
title: "API Reference"
date: 2020-05-17T14:14:36+02:00
draft: false
---

## Initializing Annotorious

Initialize an Annotorious instance on an image with 

```javascript
var anno = Annotorious.init(config);
```

The configuration must be an object with the following properties:

| Property    | Value                                                                               | Default |
|-------------|-------------------------------------------------------------------------------------|---------|
| `image`     | __REQUIRED__ the DOM image element to make annotate-able or, alternatively, it the element ID | -       |
| `readOnly`  | Set to `true` to display annotations read-only            | `false`    |
| `headless`  | Headless mode completely disables the editor popup. Use the events along with [applyTemplate](https://github.com/recogito/annotorious/wiki/API-Reference#applytemplatetemplate-openeditor) to control the annotation lifecycle from outside via the API. Note that headless mode currently doesn’t support shape editing - only shape creation. | `false`    |

## Instance Methods

### addAnnotation

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](https://github.com/recogito/annotorious/wiki/Web-Annotation-Model). At the moment, 
only a single `FragmentSelector` with an `xywh=pixel` fragment is supported.

| Argument     | Value                                         |
|--------------|-----------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format    |

### removeAnnotation

Removes an annotation programmatically. 

| Argument     | Value                                         |
|--------------|-----------------------------------------------|
| `arg` | the annotation in W3C WebAnnotation format or the annotation ID |

### setAnnotations

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Value                                         |
|---------------|-----------------------------------------------|
| `annotations` | array of annotations in W3C WebAnnotation format |

### loadAnnotations

Loads annotations from a JSON file. The method returns a promise, in 
case you want to perform an action after the annotations have loaded.

```javascript
anno.loadAnnotations(url).then(function(annotations) {
  // Do something
});
```

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `url`     | the URL to HTTP GET the annotations from |

### getAnnotations

Returns all annotations according to the current rendered state, in W3C Web Annotation format. 

### setAnnotationsVisible

Shows or hides the annotation layer.

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `visible` | if `true` show the annotation layer, otherwise hide it |

### selectAnnotation

Selects an annotation programmatically, highlighting its shape, and opening the editor popup. 

- If no argument is provided (or the annotation or ID is unknown), this method will deselect 
  the current selection, if any
- The method will return the selected annotation as a result
- Note that the the `selectAnnotation` event will __not__ fire when using this method

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `arg` | the annotation or the annotation ID |

### applyTemplate

When a new annotation is created, Annotorious will automatically apply the given 
template. The template can be a single annotation body, or an array of bodies.
If `openEditor` is set to true, the editor popup will open, allowing the user to 
edit the annotation normally. Otherwise, Annotorious will __instantly__ create an 
annotation from the template bodies __only__. The editor will not open. 

Use this method if you want to implement batch- or fast annotation modes, where
the same annotation should be applied repeatedly, and the user should only take
care of drawing selections.

| Argument   | Value                                    |
|------------|------------------------------------------|
| `template` | the template body or list of bodies to apply automatically |
| `openEditor` | if `true`, the editor will open after the template is applied |

### destroy

Destroys this instance of Annotorious, removing the annotation layer on the image.

### on

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Value                                          |
|------------|------------------------------------------------|
| `event`    | the name of the event                          |
| `callback` | the function to call when the event is emitted |

### off

Unsubscribe from an event. If no callback is provided,
all event handlers for this event will be unsubscribed.

| Argument   | Value                                       |
|------------|---------------------------------------------|
| `event`    | the name of the event                       |
| `callback` | the function used when binding to the event |

## Events

### selectAnnotation

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.


| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

### createAnnotation

Fired when a new annotation is created from a user selection.

| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

### updateAnnotation

Fired when an existing annotation was updated.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the updated annotation                 |
| `previous`   | the annotation state before the update |

### deleteAnnotation

Fired when an existing annotation was deleted.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the deleted annotation                 |

### mouseEnterAnnotation

Fired when the mouse moves into an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

### mouseLeaveAnnotation

Fired when the mouse moves out of an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

