# arToolKit JR Guite
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
