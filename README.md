<p align="center"><a href="https://aframe.io" target="_blank"><img width="480" alt="A-Frame" src="https://user-images.githubusercontent.com/674727/32120889-230ef110-bb0f-11e7-908c-76e39aa43149.jpg"></a></p>

<p align="center"><b>A-Frame, a web framework for building virtual reality experiences.</b></p>

## A-Frame maintainers

- [Diego Marcos](https://twitter.com/dmarcos)
- [Don McCurdy](https://twitter.com/donrmccurdy)
- [Kevin Ngo](https://twitter.com/andgokevin)

* * *

<h1 align="center">AR.js reduced-test-cases for Web Augmented Reality</h1>

## Overview

This repository is an introduction to common tasks for organising WebAR projects. Examples are hosted on GitHub Pages for convenient reference.

### Camera Blur 

Defocusing the camera background can degrade performance – consider this if you need to quickly pick out the subject in 3D.

[`blur.html`](https://inspiredlabs.github.io/ar.js/blur.html) applies CSS filters to `#arjs-video` canvas:

```
#arjs-video {
    filter: blur(8px);
    -webkit-filter:blur(8px);
    /* transform: scale(1.8); *//* Might stretch CPU! */
  }
```




* * *

### Trex

[`trex.html`](https://inspiredlabs.github.io/ar.js/trex.html) extends `aframe.min.js` with `aframe-ar.js`, a marker + location based build without NFT support. 

Print the marker or use it on your tablet todisplay a `glTF` model on-top of this marker ([8Kb jpg](https://inspiredlabs.github.io/ar.js/hiro.jpg), or [1Kb webP](https://inspiredlabs.github.io/ar.js/hiro.webp)). Open [`trex.html`](https://inspiredlabs.github.io/ar.js/trex.html) on your mobile and point the camera to:

![Hiro marker](https://inspiredlabs.github.io/ar.js/hiro.webp "Hiro marker")

In the HTML, you'll notice `rotation="pitch yaw roll"` to describe each degree of freedom. This makes orientation of 3D objects in co-ordinate space trivial: 

```
Roll, Pitch, Yaw:
 - pitch: vertical rollercoaster up/down, nose & tail (nose dive: 90°).
 - yaw: steering, like a bike (opposite direction: -90°).          
 - roll: horizontal tilting, like plane wings (try: 45°).
```

* * *
	


### Shadow on Transparent Plane

See: [`shadow-material.html`](https://inspiredlabs.github.io/ar.js/shadow-material.html) working in the viewport.

 `getOrCreateObject3D('name', constructor)` applies `shadow-material` attribute directly to `<a-plane>`. This script works in the `<head>`:

```
AFRAME.registerComponent('shadow-material', {
      init() {
        this.material = new THREE.ShadowMaterial();
        this.el.getOrCreateObject3D('mesh').material = this.material;
        this.material.opacity = 0.3;
      }
    });
```

⚠️ [Deprecated](https://github.com/aframevr/aframe/blob/master/CHANGELOG.md#deprecations-2) potential [update](https://codepen.io/inspiredlabs/pen/ZEbyzOK).

* * *

### Location Based Demo 

In the console the entity `a-scene` in [`markerless.html`](https://inspiredlabs.github.io/ar.js/markerless.html) shows device location AR:


```html
<!doctype html>
<html>

<head>
  <link rel="canonical" href="https://inspiredlabs.github.io/ar.js/markerless.html" />
  <!-- location based aframe v0.9.2 -->
  <script src="https://aframe.io/releases/0.9.2/aframe.min.js"></script>
  <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js"></script><!-- debug -->
  <script>
    const log = console.log;
    window.onload = () => {
      let scene = document.querySelector('a-scene'); /* Apply to whole scene */

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function (position) {
          let gps = document.createAttribute('gps-entity-place'),
            arjs = document.createAttribute('arjs'),
            welcome = document.getElementById('welcome');

          arjs.value = 'sourceType: webcam; sourceWidth: 1280; sourceHeight: 960; trackingMethod: best; debugUIEnabled: false;';
          gps.value = `latitude: ${position.coords.latitude - 0.001}; longitude: ${position.coords.longitude + 0.001}`;
          log(gps.value);
          scene.setAttributeNode(gps); /* Apply to whole scene */
          scene.setAttributeNode(arjs);
        });
      }

    };
  </script>
</head>
<a-scene vr-mode-ui="enabled: false">
  <a-entity id="wrapper" position="0 -8 0">

    <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9" shadow></a-box>
    <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E" shadow></a-sphere>
    <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D" shadow></a-cylinder>
    <a-plane position="0 0 -5.0" rotation="-90 0 0" width="7" height="7" color="#7BC8A4" shadow></a-plane>

    <a-text id="welcome" value="Welcome" scale="75 75 75" color="#000000" position="-30 0 -150"></a-text>
    <!-- look-at="[gps-camera]" -->

  </a-entity><!-- /wrapper -->

  <a-camera camera="fov: 60;" gps-camera rotation-reader></a-camera>
</a-scene>
</body>

</html>
```


<!--
`setAttribues` won't work:
```
// `setAttribues` helper function: https://stackoverflow.com/a/12274782
    function setAttributes(el, attrs) {
      for (var key in attrs) {
        el.setAttribute(key, attrs[key]);
      }
    };
```
-->


🎈 Object identification is [limited](https://www.youtube.com/watch?v=8TVOTf33q8A) with predefined dataset properties such as `data-*`. Their naming patterns include hyphen `-`, underscore `_`, period `.` or colon `:`. Whilst this makes them easy to access, they lack complex, human readable names:

- `log(gps.dataset['gps']);`
- `log(gps.dataset.gps);`
- `log(gps.getAttribute('data-gps'));`

♻️ `createAttribute` enables you to write custom attributes to refrence in the DOM. They are constructed in three steps:

```
let gps = document.createAttribute('gps-entity-place');
```

`createAttribute` expects a `string`, and doesn't apply a value automatically. Here, we'll use `position` in a template literal to form the property value correctly:

```
gps.value = `latitude: ${position.coords.latitude - 0.001}; longitude: ${position.coords.longitude + 0.001}`;
// log(gps.value); // check property value
```

The final step is to apply to the element to **one** DOM node:

```
scene.setAttributeNode(gps);
//welcome.setAttributeNode(gps); // apply text only 
```

This should be invoked using `window.onload`.

* * * 



## About

Learn more about Scott Phillips on [Inspired Labs](https://inspiredlabs.co.uk), or follow [@inspiredlabs](https://twitter.com/inspiredlabs) on Twitter.

## License

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <https://www.gnu.org/licenses/>.
