# Tutorial: A‑Frame VR 360 (README)

## Índice rápido
- [Requisitos](#requisitos)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Pasos principales](#pasos-principales)
- [Publicación en GitHub Pages](#publicación-en-github-pages)
- [Créditos](#créditos)

## Requisitos
- Navegador compatible con WebGL/WebXR.
- Imágenes 360 equirectangulares (relación 2:1).

## Estructura del proyecto
```
vr-aframe-tutorial/
├─ index.html
├─ assets/
│  ├─ images/360/
│  ├─ models/
│  └─ audio/
└─ README.md
```



<!-- Page 1 -->

A c t i v i t y
Web-Based VR Creation with A-Frame
### T O O L S
Code Editor: Visual studio, Sublime
HTML Explorer: Chrome, Firefox, Edge
Local Server: Python
Visual Assets: 3d model (.glb), 360
environments, Images for textures
Immersive Hardware: VR Headset,
Google Cardboard
### I N S T R U C T I O N S
Work individually: Use the tools and
examples provided
Follow the step-by-step guide to create your
own immersive scene
Publish your project and share the link with
the group
### W O R K F L O W
1. Create an HTML file with a basic
structure
2. Test locally using Python server
3. Add components (models, sound,
events)
4. Upload to GitHub Pages to publish
5. Explore visual quality, physics, and
interaction
6. Share the link

<!-- Page 2 -->
Step 1:
Basic Scene
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>VR Scene</title>
<script src="https://aframe.io/releases/1.5.0/aframe.min.js"></script>
</head>
<body>
<a-scene>
<a-box position="0 1 -3" rotation="0 45 0" color="#4CF3D9"></a-box>
<a-plane rotation="-90 0 0" width="10" height="10" color="#7BC8A4"></a-plane>
<a-sky color="#ECECEC"></a-sky>
</a-scene>
</body>
</html>
```

Figures
```
<a-box>
<a-sphere>
<a-cylinder>
<a-cone>
<a-plane>
<a-ring>
<a-torus>
<a-torus-knot>
<a-dodecahedron>
<a-octahedron>
<a-tetrahedron>
<a-icosahedron>
```
Environment
```
<a-scene>
<a-sky>
<a-assets>
<a-asset-item>
<a-image>
<a-video>
<a-videosphere>
<a-text>
<a-entity>
```
Interaction
```
<a-camera>
<a-cursor>
```
Lights
```
<a-light>
```
Audio
```
<a-sound>
```
<!-- Page 3 -->
Step 2:
Test Scene
Server:
```
python -m http.server 8000
```
Web:
```
http://localhost:8000
```


<!-- Page 4 -->
Add 3d model and Environment
3d model inside <a-scene>
```
<a-asset-item id="model" src="model.glb"></a-asset-item>
<a-entity gltf-model="#model" position="0 0 -3" scale="1 1 1"></a-entity>Step 3:
```
360 image:
```
<a-sky src="360image.jpg" rotation="0 -90 0"></a-sky>
```

<!-- Page 5 -->

<a-entity
cursor="rayOrigin: mouse"
raycaster="objects: .interactable"
position="0 0 -1"
geometry="primitive: ring; radiusInner: 0.01; radiusOuter: 0.015"
material="color: white; shader: flat">
</a-entity>Step 4:
Visual Interaction
<a-box position="-4 1.5 -3" color="#FFFFFF" depth="0.2" height="1" width="1.5"
class="interactable"
onclick="const i=document.getElementById('revealed-image');i.setAttribute('visible',!i.getAttribute('visible'))">
</a-box>
<a-image id="revealed-image" src="image1.jpg" position="-4 2.5 -3" width="3" height="2" visible="false"></a-image>
Interaction:
Toogle image:
<a-box class="interactable" position="3 1 -3" color="#4CF3D9"
onclick="this.setAttribute('color', '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0'))">
</a-box>
Random color:

<!-- Page 6 -->

<script>
AFRAME.registerComponent('teleport-on-click', {
init: function () {
this.el.addEventListener('click', () => {
const rig = document.querySelector('#rig');
const point = document.querySelector('#teleport-point').object3D.position;
if (rig) {
rig.object3D.position.copy(point);
}
});
}
});
</script>Step 5:
T e l e p o r t
Component:
<a-box class="interactable" position="-3 1 -3" color="#FFC107"
teleport-on-click
Teleport on click inside <a-scene>
<a-entity id="teleport-point" position="5 1.6 0"></a-entity>
Teleport point position

<!-- Page 7 -->

<a-box position="0 2 -5" rotation="0 45 45" scale="2 2 2"
animation="property: object3D.position.y; to: 2.2; dir: alternate; dur: 2000; loop: true"></a-box>Step 6:
Animation and Sound
Animated box:
<a-sphere class="interactable" position="4 1.5 -3" radius="0.5" color="#3399FF"
onclick="document.querySelector('#audio').components.sound.playSound()">
</a-sphere>
<a-sound id="audio" src="sound.mp3" autoplay="false"></a-sound>
Sound object:

<!-- Page 8 -->

Step 7:
M a t e r i a l s
material="metalness: 1; roughness: 0; color: #888">
material="color: #AAF; opacity: 0.3; transparent: true; roughness: 0.1; metalness: 0.5">
material="color: #FF00FF; metalness: 0; roughness: 0.3">
Materials (metal, glass, plastic):

<!-- Page 9 -->

Step 8:
P o s t - p r o c e s s i n g
Shadows in each object:
<script src="https://cdn.jsdelivr.net/gh/supermedium/superframe@master/components/postprocessing/dist/aframe-postprocessing-
component.min.js">
</script>
<a-scene shadow="type: pcfsoft" postprocessing="bloom: true; ssao: true">
Bloom:
shadow="receive: true"
Lights:
<a-light type="directional" color="#fff" intensity="0.4" position="2 4 4" castShadow="true"></a-light>
<a-light type="ambient" intensity="0.4"></a-light>

<!-- Page 10 -->

Step 9:
VR Settings
Replace with hybrid control:
<script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@latest/dist/aframe-extras.min.js"></script>
Movement controls (VR + keyboard)
<a-entity id="rig" movement-controls="controls: gamepad,keyboard; speed: 0.2" position="0
1.6 0">
<a-entity id="camera" camera look-controls wasd-controls></a-entity>
<a-entity laser-controls="hand: left" raycaster="objects: .interactable"></a-entity>
<a-entity laser-controls="hand: right" raycaster="objects: .interactable"></a-entity>
</a-entity>
Delete camera:
<a-entity camera look-controls wasd-controls position="0 1.6 0"></a-entity>

<!-- Page 11 -->

Step 10:
Publish website
1. Make sure your project has an index.html
This file should be in the root of the repository, or inside a specific folder if you're using a subdirectory or a
different branch.
2. Enable GitHub Pages
Go to your repository on GitHub.
Click on "Settings" (at the top of the repository).
In the left sidebar, go to "Pages" (under the Code and automation or Deploy section).
In the "Source" section, choose: /(root) if your index.html is in the root.
Click "Save".
3. Get the public URL
After saving, GitHub will generate a URL like:
https://your-username.github.io/your-repository-name/
GitHub Pages:

<!-- Page 12 -->

E x a m p l e
h t t p s : / / v i c v a l e n s . g i t h u b . i o / v r _ a f r a m e /
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>VR Scene</title>
<script src="https://aframe.io/releases/1.5.0/aframe.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/supermedium/superframe@master/components/postprocessing/dist/aframe-postprocessing-
component.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@latest/dist/aframe-extras.min.js"></script>
<script src="https://unpkg.com/aframe-effects/dist/aframe-effects.min.js"></script>
<script>
AFRAME.registerComponent('teleport-on-click', {
init: function () {
this.el.addEventListener('click', () => {
const rig = document.querySelector('#rig');
const point = document.querySelector('#teleport-point').object3D.position;
if (rig) {
rig.object3D.position.copy(point);
}
});
}
});
</script>
</head>
<body>
<a-scene shadow="type: pcfsoft" postprocessing="bloom: true; ssao: true">
<!-- Assets -->
<a-assets>
<a-asset-item id="model" src="model.glb"></a-asset-item>
</a-assets>
<!-- Cámara -->
<a-entity id="rig" movement-controls="controls: gamepad,keyboard; speed: 0.2" position="0 1.6 0">
<a-entity id="camera" camera look-controls wasd-controls></a-entity>
<a-entity laser-controls="hand: left" raycaster="objects: .interactable"></a-entity>
<a-entity laser-controls="hand: right" raycaster="objects: .interactable"></a-entity>
</a-entity>
<!-- Cursor -->
<a-entity
cursor="rayOrigin: mouse"
raycaster="objects: .interactable"
position="0 0 -1"
geometry="primitive: ring; radiusInner: 0.01; radiusOuter: 0.015"
material="color: white; shader: flat">
</a-entity>
<!-- Luces -->
<a-light type="directional" color="#fff" intensity="0.4" position="2 4 4" castShadow="true"></a-light>
<a-light type="ambient" intensity="0.4"></a-light>
<!-- Objetos -->
<a-entity gltf-model="#model" position="0 .5 -3" scale="1 1 1" shadow="cast: true" shadow="receive: true"></a-entity>
<a-box position="0 .5 -3" rotation="0 45 0" color="#4CF3D9" shadow="cast: true"></a-box>
<a-plane rotation="-90 0 0" width="10" height="10" color="#7BC8A4" shadow="receive: true"></a-plane>
<a-sky src="360image.jpg" rotation="0 -90 0"></a-sky>
<!-- Caja que revela imagen -->
<a-box position="-4 1.5 -3" color="#FFFFFF" depth="0.2" height="1" width="1.5"
class="interactable"
onclick="const i=document.getElementById('revealed-image');i.setAttribute('visible',!i.getAttribute('visible'))"
material="color: #FF00FF; metalness: 0; roughness: 0.3"
shadow="cast: true">
</a-box>
<a-image id="revealed-image" src="image1.jpg" position="-4 2.5 -3" width="3" height="2" visible="false"></a-image>
<!-- Caja que cambia de color -->
<a-box class="interactable" position="3 1 -3" color="#4CF3D9"
onclick="this.setAttribute('color', '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0'))"
shadow="cast: true">
</a-box>
<!-- Caja para teletransporte -->
<a-box class="interactable" position="-3 1 -3" color="#FFC107"
teleport-on-click
shadow="cast: true">
</a-box>
<a-entity id="teleport-point" position="5 1.6 0"></a-entity>
<!-- Animación -->
<a-box position="0 2 -5" rotation="0 45 45" scale="2 2 2"
animation="property: object3D.position.y; to: 2.2; dir: alternate; dur: 2000; loop: true"
shadow="cast: true">
</a-box>
<!-- Audio -->
<a-sphere class="interactable" position="4 1.5 -3" radius="0.5" color="#3399FF"
onclick="document.querySelector('#audio').components.sound.playSound()"
material="metalness: 1; roughness: 0; color: #888"
shadow="cast: true">
</a-sphere>
<a-sound id="audio" src="sound.mp3" autoplay="false"></a-sound>
<a-sphere radius="0.5" position="-2 1 -3"
material="color: #AAF; opacity: 0.3; transparent: true; roughness: 0.1; metalness: 0.5"
shadow="cast: true">
</a-sphere>
</a-scene>
</body>
</html>

## Publicación en GitHub Pages
1. Crea un repositorio público en GitHub.
2. Sube `index.html`, `assets/` y este `README.md`.
3. Ve a **Settings → Pages** y activa **Deploy from a branch**.
4. Elige `main` y raíz (`/`) o `/docs`.
5. Comparte la URL de GitHub Pages con tus alumnos.

## Créditos
- Autor: VV
- Framework: [A‑Frame](https://aframe.io)
