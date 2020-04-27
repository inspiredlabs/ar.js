<p align="center"><a href="https://aframe.io" target="_blank"><img width="480" alt="A-Frame" src="https://user-images.githubusercontent.com/674727/32120889-230ef110-bb0f-11e7-908c-76e39aa43149.jpg"></a></p>

<p align="center"><b>A-Frame, a web framework for building virtual reality experiences.</b></p>

## A-Frame maintainers

- [Diego Marcos](https://twitter.com/dmarcos)
- [Don McCurdy](https://twitter.com/donrmccurdy)
- [Kevin Ngo](https://twitter.com/andgokevin)

* * *

<h1 align="center">Augmented Reality reduced-test-cases for the web</h1>

## Overview

This repository aims to have working examples of common tasks for organising WebAR projects – hosted on GitHub Pages for convenience.

### `markerless.html` shows device location AR, [here](https://inspiredlabs.github.io/ar.js/markerless.html):

```html
<!doctype html>
<html>
<head>
<link rel="canonical" href="https://inspiredlabs.github.io/ar.js/markerless.html" />
<!-- aframe v0.9.2 is location based -->
<script src="https://aframe.io/releases/0.9.2/aframe.min.js"></script>
<!--<script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js"></script><!-- debug -->
<script>
  const log = console.log;
  window.onload = () => {
    let scene = document.querySelector('a-scene'); // Apply to whole scene?

    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(function (position) {
        let gps = document.createAttribute('gps-entity-place'),
            arjs = document.createAttribute('arjs'),
            welcome = document.getElementById('welcome');

        arjs.value = 'sourceType: webcam; sourceWidth: 1280; sourceHeight: 960; trackingMethod: best; debugUIEnabled: false;';
        gps.value = `latitude: ${position.coords.latitude - 0.001}; longitude: ${position.coords.longitude + 0.001}`;
        log(gps.value);
        welcome.setAttributeNode(gps);
        //scene.setAttributeNode(gps); // Apply to whole scene?
      });
    }

  };
</script>
</head>
<a-scene vr-mode-ui="enabled: false">
  <a-entity id="wrapper" position="0 -8 0"><a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9" shadow></a-box>
    <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E" shadow></a-sphere>
    <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D" shadow></a-cylinder>
    <a-plane position="0 0 -5.0" rotation="-90 0 0" width="7" height="7" color="#7BC8A4" shadow></a-plane>
    <a-text id="welcome" value="Welcome" scale="75 75 75" color="#000000"  position="-30 0 -150">
    </a-text><!-- look-at="[gps-camera]" --></a-entity><!-- /wrapper -->
    <a-camera camera="fov: 60;" gps-camera rotation-reader></a-camera>
    </a-scene>
  </body>
</html>
```

## Twitter

- Follow [@inspiredlabs](https://twitter.com/inspiredlabs) on Twitter.

## License

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <https://www.gnu.org/licenses/>.
