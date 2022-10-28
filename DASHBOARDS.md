# Dashboard data structure

Dashboards data is stored as JSON, and can be changed using the API:

 - [Dashboard creation](https://app.deepint.net/api/v1/documentation/#/dashboards/post_api_v1_workspace__workspaceId__dashboards)
 - [Dashboard update](https://app.deepint.net/api/v1/documentation/#/dashboards/post_api_v1_workspace__workspaceId__dashboard__dashboardId_)

Dashboards have the following root properties:

| Property | Type | Description |
|---|---|---|
| `border` | boolean | Set it to true to display borders between widgets. Set it to false for no border. |
| `responsive` | boolean | Set it to true to use responsive sizes. Set it to false to use static sizes. |
| `cellHeight` | number | Cell height in pixels. This determines the height unit. Can only be: `1`, `8`, `16` or `32` |
| `variables` | Array | Array of variables. |
| `filters` | Array | Array of filter applied to the entire dashboard. |
| `sections` | Array | Array of sections. |

Example:

```json
{
    "border": true,
    "responsive": true,
    "cellHeight": 1,
    "variables": [],
    "filters": [],
    "sections": []
}
```

## Variables

Variables are used to store values in order to use in filters.

Each variable is en object with the following properties:

| Property | Type | Description |
|---|---|---|
| `name` | string | Variable name. You use this name to reference it, so it should be unique. |
| `type` | string | Variable type. The same types as data source feature types: `nominal`, `text`, `numeric`, `logic` or `date` |
| `inputType` | string | Determines if it's a variable the user provides (`input`) or it's computed from the values of other variables (`computed`) |
| `defaultValue` | string | Default value for the variable. |
| `expression` | string | Expression to compute the variable. Only if `inputType` = `computed`. Example: `(var1 + var2) / var3` |

Example:

```json
{
    "name": "var1",
    "type": "numeric",
    "inputType": "input",
    "defaultValue": "0",
    "expression": ""
}
```

```json
{
    "name": "var_computed",
    "type": "numeric",
    "inputType": "computed",
    "defaultValue": "0",
    "expression": "var1 * 2"
}
```

## Filters

Filters are used to filter the data sources, in order to use the same visualization for different sets of data in the same source.

Filters are JSON structures that extends the data source filters. See [Sources filtering](./SOURCES.md#api-filtering).

Each filter has the following properties:

| Property | Type | Description |
|---|---|---|
| `source` | string | ID of the data source to filter. |
| `source_name` | string | Optional. Name of the source to display when modifying the dashboard. |
| `fields` | Array | Array of fields the source had when the filter was created. This is used in order to automatically modify it when you change the source features order. |
| `root` | Object | Data filter tree |

Example:

```json
{
    "source": "xxxx-xxxxx-xxxxx-xxxx",
    "source_name": "IRIS",
    "fields": [
        {"index": 0, "name": "sepalLength", "type": "numeric"},
        {"index": 1, "name": "sepalWidth", "type": "numeric"},
        {"index": 2, "name": "petalLength", "type": "numeric"},
        {"index": 3, "name": "petalWidth", "type": "numeric"},
        {"index": 4, "name": "species", "type": "nominal"}
    ],
    "root": {
        "type":"one",
        "left":0,
        "operation":"lt",
        "right":"6.5"
    }
}
```

You can add variables to the filter nodes, in order to do it, add the `rightType` property set to `var` and `rightName` set to the variable name:

```json
{
    "source": "xxxx-xxxxx-xxxxx-xxxx",
    "source_name": "IRIS",
    "fields": [
        {"index": 0, "name": "sepalLength", "type": "numeric"},
        {"index": 1, "name": "sepalWidth", "type": "numeric"},
        {"index": 2, "name": "petalLength", "type": "numeric"},
        {"index": 3, "name": "petalWidth", "type": "numeric"},
        {"index": 4, "name": "species", "type": "nominal"}
    ],
    "root": {
        "type":"one",
        "left":0,
        "operation":"lt",
        "rightType":"var",
        "rightName": "var1"
    }
}
```

## Sections

Each section has the following properties:

| Property | Type | Description |
|---|---|---|
| `title` | string | Section title. |
| `filters` | Array | Array of filters to apply only for this section |
| `layout` | Array | Array of widgets to display for the section |

Example:

```json
{
    "title": "Section Title",
    "filters": [],
    "layout": []
}
```

## Widgets

Each widgets is an objet with a numeric ID, a position and a size:

| Property | Type | Description |
|---|---|---|
| `i` | number | Widget unique ID |
| `x` | number | Horizontal position, from `0` to `12` |
| `y` | number | vertical position. Each unit is defined in pixel by `cellHeight` |
| `w` | number | Width, from `1` to `12` |
| `h` | number | Height. Each unit is defined in pixel by `cellHeight` |

There are several types of widgets, each one width its own properties.

### Visualization widget

Used to display visualizations.

| Property | Type | Description |
|---|---|---|
| `vid` | string | Visualization ID |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "vid": "xxxx-xxxx-xxxx-xxxx"
}
```

### Iframe / Image

Used to display iframes of images.

| Property | Type | Description |
|---|---|---|
| `url` | string | URL to display. |
| `image` | boolean | True if the URL must display as an image. |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "url": "https://example.com/image.jpg",
    "image": true
}
```

### Rich text

Used to display rich text.

| Property | Type | Description |
|---|---|---|
| `delta` | Object | Data encoded using delta format. See: [https://quilljs.com/docs/delta/](https://quilljs.com/docs/delta/) |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "delta": {
        "ops": []
    }
}
```

### Variable input

Used to ask the user to input variables.

| Property | Type | Description |
|---|---|---|
| `variable` | string | Variable name |
| `label` | string | Label to display for the user |
| `max` | number | Max value, for numeric variables. |
| `min` | number | Min value, for numeric variables. |
| `step` | number | Step value for arrows, for numeric variables. |
| `source` | string | Source ID for nominal value mapping |
| `source_name` | string | Optional. Name of the source for display |
| `fields` | Array | Array of fields. |
| `field` | number | Index of a nominal source field to map nominal values |
| `categoryMap` | Array | Array of mappings between source values and display values |
| `filter` | Object | Static filter to apply when searching for nominal values |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "variable": "var2",
    "label": "species",
    "source": "xxxx-xxxxx-xxxxx-xxxx",
    "source_name": "IRIS",
    "fields": [
        {"index": 0, "name": "sepalLength", "type": "numeric"},
        {"index": 1, "name": "sepalWidth", "type": "numeric"},
        {"index": 2, "name": "petalLength", "type": "numeric"},
        {"index": 3, "name": "petalWidth", "type": "numeric"},
        {"index": 4, "name": "species", "type": "nominal"}
    ],
    "field": 4,
    "categoryMap": [
        {
            "index": 4,
            "name": "Species",
            "map": [
                { "key": "setosa", "value": "Setosa (IRIS)" },
                { "key": "virginica", "value": "Virginica (IRIS)" },
                { "key": "versicolor", "value": "Versicolor (IRIS)" }
            ]
        }
    ]
}
```

### Date picker

Used to pick a range of dates. The start date and end date are stored in separate variables.

| Property | Type | Description |
|---|---|---|
| `variableFrom` | string | Name of the variable to store the start date. |
| `variableTo` | string | Name of the variable to store the ending date. |
| `label` | string | Label to display to the user |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "variableFrom": "start_date",
    "variableTo": "end_date",
    "label": "Filter by date"
}
```

### Model widget

Used to display information of features from an artificial intelligence model.

| Property | Type | Description |
|---|---|---|
| `mid` | string | ID of the model. |
| `mtype` | string | Type of widget. |

Available model widget types:

 - `metrics` - Model metrics
 - `confusion` - Confusion matrix
 - `roc` - ROC chart
 - `rules` - List of rules
 - `importances` - List of importances
 - `test` - Predict form

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "mid": "xxxx-xxxx-xxxx-xxxx",
    "mtype": "confusion"
}
```

### Alerts feed

Used to display alerts historic data.

| Property | Type | Description |
|---|---|---|
| `alertf` | boolean | Set to true for this widget type. |
| `alerts` | string | List opf IDs of alerts types to display, split by commas. |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "alertf": true,
    "alerts": "xxxxx,yyyyy,zzzzzz"
}
```

### Raw text

Raw text widget.

| Property | Type | Description |
|---|---|---|
| `raw` | string | Raw text to display. |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "raw": "Example raw text widget"
}
```

### Filler

Empty widget.

| Property | Type | Description |
|---|---|---|
| `isFiller` | boolean | Set to true for this widget type. |

```json
{
    "i": 1,
    "x": 0,
    "y": 0,
    "w": 6,
    "h": 100,
    "isFiller": true
}
```
