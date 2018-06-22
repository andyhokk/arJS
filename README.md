# arToolKit JR Guite
[Live Demo](https://andyhokk.github.io/arJS/dist)

## Used Library
| Library  | Version |
| ------------- | ------------- |
| THREE.OBJLoader2 | [1.3.1](https://github.com/mrdoob/three.js/blob/67f24d0d2dc72b375c1eddc2aa05c2624da71ab0/examples/js/loaders/OBJLoader2.js)  |
| three.min.js (three.js minified version)  | [r93](https://github.com/mrdoob/three.js/blob/1300607b1471fd7148cf35169c3eaaa15b44a9d0/build/three.min.js)  |
| artoolkit.min.js (artoolkit.js minified version)  | [ea819f1  on 29 Jan](https://github.com/artoolkitx/jsartoolkit5/blob/ea819f11566f02eb10fc4d2abd8ce531983c06e2/build/artoolkit.min.js)  |
| artoolkit.three.js | [afd2765 on 16 Jan](https://github.com/artoolkitx/jsartoolkit5/blob/afd27655c3c3868fc79d579aa2e0898b2981e191/js/artoolkit.three.js) |
| jquery-3.3.1.slim.min.js (jquery.js minified version) | 3.3.1 |
| popper.min.js (popper.js minified version) | 1.14.3 |
| bootstrap.min.js (bootstrap.js minified version) | 4.1.1 |

## Inital the library in .html
```html
<!--artoolkit.js used library-->
<script async src="artoolkit.min.js"></script>
<script async src="three.min.js"></script>
<script async src="artoolkit.three.js"></script>

<!--bootstrap used library-->
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js" integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
```

## Creacte Object
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
