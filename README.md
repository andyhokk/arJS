# arJS with a-frame Guide

## Including the Libraries

```html
<script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
<script src="https://cdn.rawgit.com/jeromeetienne/AR.js/1.6.0/aframe/build/aframe-ar.js"></script>
```

## Ten lines code example

```html
<!doctype HTML>
<html>
<script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
<script src="https://cdn.rawgit.com/jeromeetienne/AR.js/1.6.0/aframe/build/aframe-ar.js"></script>
  <body style='margin : 0px; overflow: hidden;'>
    <a-scene embedded arjs>
  	<a-marker preset="hiro">
            <a-box position='0 0.5 0' material='color: black;'></a-box>
  	</a-marker>
  	<a-entity camera></a-entity>
    </a-scene>
  </body>
</html>
```

## Including the Marker example

### Build-in 'hiro'/'kanji' Marker

```html
<a-marker-camera preset='hiro'>
  <!-- the object or 3D model after recongzine the QR-Marker-->
  <a-box position='0 0.5 0' material='color: black;'></a-box>
</a-marker-camera>
```
```html
<a-marker-camera preset='kanji'>
  <!-- the object or 3D model after recongzine the QR-Marker-->
  <a-box position='0 0.5 0' material='color: black;'></a-box>
</a-marker-camera>
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

## Including the 3D object
### Include the 3D box
```html
<a-box position='0 0.5 0' material='color: black;'></a-box>
```
[Attributes](https://aframe.io/docs/0.8.0/primitives/a-box.html)
### Include the Text
```html
<a-text value="Hello, World!"></a-text>
```
[Attributes](https://aframe.io/docs/0.8.0/primitives/a-text.html#attributes)
