# arToolKit JR Guite
[Live Demo](https://andyhokk.github.io/arJS/dist)

## Used Library
| Library  | Version |
| ------------- | ------------- |
| THREE.OBJLoader2 | [1.3.1](https://github.com/mrdoob/three.js/blob/67f24d0d2dc72b375c1eddc2aa05c2624da71ab0/examples/js/loaders/OBJLoader2.js)  |
| three.min.js (three.js minified version)  | [r93](https://github.com/mrdoob/three.js/blob/1300607b1471fd7148cf35169c3eaaa15b44a9d0/build/three.min.js)  |
| artoolkit.debug.js (artoolkit.js debug version)  | [7773318  on 3 Feb 2017](https://github.com/artoolkit/jsartoolkit5/blob/77733182a4c519b8e683cbf246a22920d94f3deb/build/artoolkit.debug.js)  |
| artoolkit.three.js | [afd2765 on 16 Jan](https://github.com/artoolkitx/jsartoolkit5/blob/afd27655c3c3868fc79d579aa2e0898b2981e191/js/artoolkit.three.js) |
| artoolkit.api.js | [638125f  on 28 Oct 2017](https://github.com/artoolkit/jsartoolkit5/blob/638125fc40eae53793e34bb97ce66799aae85801/js/artoolkit.api.js) |
| jquery-3.3.1.slim.min.js (jquery.js minified version) | 3.3.1 |
| popper.min.js (popper.js minified version) | 1.14.3 |
| bootstrap.min.js (bootstrap.js minified version) | 4.1.1 |

## Inital the library in .html
```html
<!--artoolkit.js used library-->
<script defer src='three.min.js'></script>
<script async src='artoolkit.debug.js'></script>
<script defer src='artoolkit.three.js'></script>
<script defer src='artoolkit.api.js'></script>

<!--bootstrap used library-->
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js" integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
```

## Edit the artoolkit.api & artoolkit.three.js to fix the Mobile Portrait mode
### Replace the _' copyImageToHeap'_ in _artoolkit.api_ to:
```javascript
ARController.prototype._copyImageToHeap = function (image) {
        if (!image) {
            image = this.image;
        }

        this.ctx.save();

        if (this.orientation === 'landscape') {
            //landscape
            this.ctx.drawImage(image, 0, 0, this.canvas.width, this.canvas.height); // draw video
        }
        else {
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
            //portrait
            var scale = this.canvas.height / this.canvas.width;
            var scaledHeight = this.canvas.width * scale;
            var scaledWidth = this.canvas.height * scale;
            var marginLeft = (this.canvas.width - scaledWidth) / 2;
            this.ctx.drawImage(image, marginLeft, 0, scaledWidth, scaledHeight); // draw video
            //this.ctx.drawImage(image, 0, 0, this.canvas.height, this.canvas.width);
        }

        this.ctx.restore();

        var imageData = this.ctx.getImageData(0, 0, this.canvas.width, this.canvas.height);
        var data = imageData.data;

        if (this.videoLuma) {
            var q = 0;
            //Create luma from video data assuming Pixelformat AR_PIXEL_FORMAT_RGBA (ARToolKitJS.cpp L: 43)

            for (var p = 0; p < this.videoSize; p++) {
                var r = data[q + 0], g = data[q + 1], b = data[q + 2];
                // videoLuma[p] = (r+r+b+g+g+g)/6;         // https://stackoverflow.com/a/596241/5843642
                this.videoLuma[p] = (r + r + r + b + g + g + g + g) >> 3;
                q += 4;
            }
        }

        if (this.dataHeap) {
            this.dataHeap.set(data);
            return true;
        }
        return false;
    };
```
[Credit by pikilipita](https://github.com/pikilipita/AR.js/blob/703e1082bd3e36bfebb175f3985053126594ddbb/three.js/vendor/jsartoolkit5/js/artoolkit.api.js)
### Replace the _'createThreeScene'_ in _artoolkit.three.js_ to:
~~plane.rotation.z = Math.PI/2;~~
```javascript
if (this.orientation === 'portrait') {
	
}
```

## Create Object
### Shpere
```javascript
var createShpere = function () {
	var sphere = new THREE.Mesh(
		new THREE.SphereGeometry(0.5, 8, 8),
		new THREE.MeshNormalMaterial()
	);
	sphere.material.shading = THREE.FlatShading;
	sphere.position.z = 0.5;

	return sphere;
}
```
## QR Marker
### Add a pattern 5 QR Code
```javascript
arController.setPatternDetectionMode(artoolkit.AR_MATRIX_CODE_DETECTION);
...
var markerRoot = arController.createThreeBarcodeMarker(5);
markerRoot.add(sphere); // Marker represent the sphere
arScene.scene.add(markerRoot);
```
### Add .patt QR Code (patt.hiro)
```javascript
arController.loadMarker('Data/patt.hiro', function(markerId) {
  var markerRoot = arController.createThreeMarker(markerId);
  markerRoot.add(sphere);
  arScene.scene.add(markerRoot);
  });
```

## Under findObjectUnderEvent class
### For single object, use intersectObject(objects)
```javascript
var intersects = raycaster.intersectObject(objects);
```
### For mult. object, use intersectObjects(objects)
```javascript
var intersects = raycaster.intersectObjects(objects.children, true);
```

## Manager (Get the status of the loaded model)
```javascript
var manager = new THREE.LoadingManager();
    manager.onProgress = function (item, loaded, total) {
        console.log(item, loaded, total);
    };
```

## OBJ Loader 2 (Load the .obj model)
```javascript
var loader = new THREE.OBJLoader2(manager);
loader.load(model_url, function (object) {
	// Get the sub-class of the obj model
	object.traverse(function (child) {
		if (child instanceof THREE.Mesh) {
			console.log(child);
		}
        });
}, onProgress, onError);

//Callback function
var onProgress = function (xhr) {
	if (xhr.lengthComputable) {
		var percentComplete = xhr.loaded / xhr.total * 100;
		console.log(Math.round(percentComplete, 2) + '% downloaded');
        }
};

var onError = function (xhr) { };
```
## WebGL Renderer
```javascript
var renderer = new THREE.WebGLRenderer({ antialias: false, alpha: true, powerPreference: 'low-power' });
renderer.autoClear = true;
renderer.setPixelRatio(window.devicePixelRatio);
```
** set the antialias be *true* will increase the GPU usage almost be 90-100% in 1920x1080 render, set *false* will be 40-50% in 1920x1080 render
