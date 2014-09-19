voronoi-map-js
==============

JavaScript port of Amit Patel's mapgen2 https://github.com/amitp/mapgen2 Map generator for games. Generates island maps with a focus on mountains, rivers, coastlines.

Based on commit: e05075cbb82851e2a3bacaa2e49e4da998894379

Flash dependencies removed.

Built with JavaScript, Node.js, JQuery, Lo-Dash, Browserify, UglifyJS, Nodeunit, Sublime Text.

[Try the demo](http://rjanicek.github.io/voronoi-map-js/)

[Install me from NPM](https://npmjs.org/package/voronoi-map)

[Fork me on GitHub](https://github.com/rjanicek/voronoi-map-js)

Installation & usage
--------------------

Using [`npm`](http://npmjs.org/):

```bash
npm install --save voronoi-map
```

In CommonJS / [Browserify](http://browserify.org/):

```js
var vm = require('voronoi-map');

var map = vm.map({width: 1000.0, height: 1000.0});
map.newIsland(vm.islandShape.makeRadial(1), 1);
map.go0PlacePoints(100, pointSelectorModule.generateRandom(map.SIZE.width, map.SIZE.height, map.mapRandom.seed));
map.go1BuildGraph();
map.assignBiomes();
map.go2AssignElevations();
map.go3AssignMoisture();
map.go4DecorateMap();

var lava = vm.lava();
var roads = vm.roads();
roads.createRoads(map, [0, 0.05, 0.37, 0.64]);
var watersheds = vm.watersheds();
watersheds.createWatersheds(map);
var noisyEdges = vm.noisyEdges();
noisyEdges.buildNoisyEdges(map, lava, map.mapRandom.seed);

var canvas = document.createElement('canvas');
vm.canvasRender.graphicsReset(canvas, map.SIZE.width, map.SIZE.height, vm.style.displayColors);
vm.canvasRender.renderDebugPolygons(canvas, map, vm.style.displayColors);
```

Tasks
-----

* fix smooth rendering bug for square point selection
	* `canvas-render.js` ~line 300, problem is with `graphics.stroke()` original render logic only draws fill paths. HTML canvas fill path does not join other paths and shows a seam between them. stroke() worked to hide the seam but square point selection exposes a bug where some strokes are not the correct color
* fix point-selector square and hexagon so distribution is symetrical when size is asymetrical