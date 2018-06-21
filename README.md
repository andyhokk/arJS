# arToolKit JR Guite
## Used Library
| Library  | Version |
| ------------- | ------------- |
| THREE.OBJLoader2 | [1.3.1](https://github.com/mrdoob/three.js/blob/67f24d0d2dc72b375c1eddc2aa05c2624da71ab0/examples/js/loaders/OBJLoader2.js)  |
| three.min.js (three.js minified version)  | [r93](https://github.com/mrdoob/three.js/blob/1300607b1471fd7148cf35169c3eaaa15b44a9d0/build/three.min.js)  |
| artoolkit.min.js (artoolkit.js minified version)  | [ea819f1  on 29 Jan](https://github.com/artoolkitx/jsartoolkit5/blob/ea819f11566f02eb10fc4d2abd8ce531983c06e2/build/artoolkit.min.js)  |
| artoolkit.three.js | [afd2765 on 16 Jan](https://github.com/artoolkitx/jsartoolkit5/blob/afd27655c3c3868fc79d579aa2e0898b2981e191/js/artoolkit.three.js) |

## Inital the library in .html
```html
<script async src="artoolkit.min.js"></script>
<script async src="three.min.js"></script>
<script async src="artoolkit.three.js"></script>
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
## QR Code
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

### Custom Marker
#### Image marker
```html
<a-marker preset='custom' type='pattern' url='your-marker.patt'>
  <!-- the object or 3D model after recongzine the QR-Marker-->
  <a-box position='0 0.5 0' material='color: black;'></a-box>
</a-marker-camera>
```
[Online marker generate tool](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)

#### Barcode Marker
##### Edit the Scene Config before use the Barcode Marker
```html
<a-scene arjs='detectionMode: mono_and_matrix; matrixCodeType: 3x3;'></a-scene>
```
##### Example of include the barcode marker
```html
<a-marker type='barcode' value='5'></a-marker>
```
