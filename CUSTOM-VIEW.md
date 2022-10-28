# Custom visualizations

Deep intelligence provides custom visualizations as a tool to integrate custom and specific visualizations. 

Custom visualizations load inside iframes, allowing them to load the required data in order to render the visualizations.

Remember custom visualization are slower than regular visualization, as they require the user to load more resources. Only use this feature in the built-in visualizations options are not enough for your use case.

You can create your own custom visualization in a single `.html` file and host it using any static web hosting service, like, for example, [GitHub Pages](https://pages.github.com/)

In order for your custom visualization to work, you need to import the script we provide below `custom-view.js`. That script reads the parameters given by Deep Intelligence and loads the required data for the visualization. 

```html
<script type="text/javascript" src="https://app.deepint.net/static/js/custom-view.js"></script>
```

You also need to call the script in order to get the data before you render anything. See an example below:

```js
DeepIntelligence.ready(function () {
  // Extract custom parameters given by the user
  var x = DeepIntelligence.param("x");
  var y = DeepIntelligence.param("y");
  
  DeepIntelligence.fetchData(
    [x, y], // Projection (selects the features to get)
    function (error, features, instances, projection) {
      renderMyCustomGraph(instances);
    }
  );
});
```

The function **DeepIntelligence.ready** takes a function as parameter, calling it when the document is ready.

After the document is ready, you can read any parameters given by the user with the function **DeepIntelligence.param(key)**. It takes the key as a parameter and returns an string with the value provided by the user configuration.

Then, you must call **DeepIntelligence.fetchData(projection, callback)**. 
 - The first parameter it takes is an array of the features indexes you want to get. For example, if you render an XY chart, you may need only the features mapped to the X and Y axis.
 - The second parameter is the callback. It receives a total of 4 parameters:
   - **error** - An instance of Error class, only if the request failed. If the request succeeded, error is set to null.
   - **features** - An array of features definitions. Each feature has `name` and `type` values.
   - **instances** - The data for the visualization, as a two-dimensional matrix.
   - **projection** - The applied projection. It is equals to the provided projection unless the projection is invalid.

Example response with example data:

```json
{
    "features": [
        {
            "index": 0,
            "name": "sepalLength",
            "type": "numeric"
        },
        {
            "index": 1,
            "name": "sepalWidth",
            "type": "numeric"
        },
        {
            "index": 2,
            "name": "petalLength",
            "type": "numeric"
        },
        {
            "index": 3,
            "name": "petalWidth",
            "type": "numeric"
        },
        {
            "index": 4,
            "name": "species",
            "type": "nominal"
        }
    ],
    "projection": null,
    "instances": [
        [
            5,
            3.5,
            1.6,
            0.6,
            "setosa"
        ],
        [
            5.1,
            3.4,
            1.5,
            0.2,
            "setosa"
        ],
        [
            4.4,
            2.9,
            1.4,
            0.2,
            "setosa"
        ]
    ]
}
```
