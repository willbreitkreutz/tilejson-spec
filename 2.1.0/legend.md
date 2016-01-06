# Legend object type
In order for CorpsMap to be able to draw a legend for a layer a legend object should be provided in the tilejson metadata manifest for the layer.

Currently we support the types of legends listed here, over time we will be adding additional legends as needs arise.

Legend objects should follow one of these formats:

### Static

Static legends are served by the source server and only need to reference the legend end-point for that layer.  Static legends should contain an image that covers the entire layer, or return HTML that can be rendered in the legend element.

```javascript
{
  // REQUIRED STRING
  // Distinguishes between the type of legend to draw
  "type":"static",
  // REQUIRED STRING
  // URL to the legend graphic, can be static or dynamically generated on the server
  "src":"/mapserv/legend.png"

}
```

### Point Symbols

Point symbol legends can take an array of symbol definitions that provide a title and the information needed to load an image of the icon used for that point.

```javascript
  {
    // REQUIRED STRING
    // Distinguishes between the type of legend to draw
    "type":"point",
    // REQUIRED ARRAY
    // Array of at least one point symbol object, each point symbol
    // object should have the structure described inside the items array.
    "items": [
      {
        // REQUIRED STRING
        // Name of the class for this icon, MUST not be interpreted as HTML
        "title":"Airport",
        // REQUIRED STRING
        // Path to the image to be loaded for the icon
        "src":"/plane.png",
        // OPTIONAL NUMBER
        // DEFAULT: 0
        // Rotation of the icon in degrees clockwise from source
        "rotation":0,
        // OPTIONAL NUMBER
        // DEFAULT: 1
        // Opacity to be used on the icon, 0 = transparent 1 = opaque
        "opacity":1
      }
    ]
  }
```

### Classified Fill

Classified fill legends can take an array of fill objects to draw a classified legend.

```javascript
  {
    // REQUIRED STRING
    // Distinguishes between the type of legend to draw
    "type":"classified-fill",
    // REQUIRED ARRAY
    // Array of at least one fill symbol object, each fill symbol
    // object should have the structure described inside the items array.
    "items": [
      {
        // REQUIRED STRING
        // Name of the class for this item, MUST not be interpreted as HTML
        "title":"Urban Area",
        // OPTIONAL STRING
        // Path to the image to be loaded for the fill,
        // if specified, it will override all other color and border settings
        "src":"/plane.png",
        // OPTIONAL NUMBER
        // DEFAULT: 0
        // Rotation of the icon in degrees clockwise from source
        "fill-color":0,
        // OPTIONAL STRING
        // DEFAULT: #ccc
        // Color of the border line around the fill polygon, in hex format
        "border-color":"#ff00ff"
        // OPTIONAL NUMBER
        // DEFAULT: 0
        // Width in pixels of the border of the fill polygon.  
        // Borders will be inside the fill polygon, large widths can overpower the polygon itself.
        "border-width":2
        // OPTIONAL NUMBER
        // DEFAULT: 1
        // Opacity to be used on the fill, 0 = transparent 1 = opaque
        "opacity":1
      }
    ]
  }
```

### Graduated Fill

Graduated fill legends display a gradient with a min and max value.

```javascript
  {
    // REQUIRED STRING
    // Distinguishes between the type of legend to draw
    "type":"graduated-fill",
    // REQUIRED STRING
    // Text to display at the start of the gradient.
    "min":"0",
    // REQUIRED STRING
    // Text to display at the end of the gradient.
    "max":"100",
    // REQUIRED ARRAY
    // Array of at least two stop objects, each
    // object should have the structure described inside the stops array.
    "stops": [
      {
        // REQUIRED NUMBER
        // Index of the stop between 0 and 100.  At least a stop at 0 and one at 100
        // are required.
        "stop":0,
        // REQUIRED STRING
        // Color at the start of the gradient
        "color":"#ff00ff",
        // OPTIONAL NUMBER
        // DEFAULT: 1
        // Opacity to be used on the fill, 0 = transparent 1 = opaque
        "opacity":1
      },
      {
        // REQUIRED NUMBER
        // Index of the stop between 0 and 100.  At least a stop at 0 and one at 100
        // are required.
        "stop":100,
        // REQUIRED STRING
        // Color at the start of the gradient
        "color":"#ffAAff",
        // OPTIONAL NUMBER
        // DEFAULT: 1
        // Opacity to be used on the fill, 0 = transparent 1 = opaque
        "opacity":1
      }
    ]
  }
```

### Line

Line legends display a single line with the described settings or an optional image.

```javascript
{
  // REQUIRED STRING
  // Distinguishes between the type of legend to draw
  "type":"line",
  // REQUIRED ARRAY
  // Array of at least one line symbol object, each line symbol
  // object should have the structure described inside the items array.
  "items": [
    {
      // REQUIRED STRING
      // Name of the class for this item, MUST not be interpreted as HTML
      "title":"Urban Area",
      // OPTIONAL STRING
      // Path to the image to be loaded for the line symbol,
      // if specified, it will override all other color and border settings
      "src":"/plane.png",
      // OPTIONAL STRING
      // DEFAULT: #ccc
      // Color of the line, in hex format
      "line-color":"#ff00ff",
      // OPTIONAL NUMBER
      // DEFAULT: 0
      // Width in pixels of the line
      "line-width":2,
      // OPTIONAL ARRAY[NUMBER]
      // DEFAULT null
      // Array of numbers describing dash dimensions, i.e. - [dash, space, dash, space]
      // Must contain at least two numbers to describe the pattern
      "line-dash":[2,2]
      // OPTIONAL NUMBER
      // DEFAULT: 1
      // Opacity to be used on the line, 0 = transparent 1 = opaque
      "opacity":1
    }
  ]
}
```
