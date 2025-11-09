# Tutorial: A-Frame VR 360

**Live demo (optional):** https://YOUR-USERNAME.github.io/YOUR-REPO/

## Index
- [Requirements](#requirements)
- [Project Structure](#project-structure)
- [Step 1: Basic Scene](#step-1-basic-scene)
- [Step 2: Run Locally](#step-2-run-locally)
- [Step 3: 3D Model and 360 Environment](#step-3-3d-model-and-360-environment)
- [Step 4: Interaction (mouse/gaze) and image toggle](#step-4-interaction-mousegaze-and-image-toggle)
- [Step 5: Simple Teleport](#step-5-simple-teleport)
- [Step 6: Animation and Audio](#step-6-animation-and-audio)
- [Step 7: Materials](#step-7-materials)
- [Step 8: Shadows and Post-processing](#step-8-shadows-and-post-processing)
- [Step 9: Hybrid Controls (PC + VR)](#step-9-hybrid-controls-pc--vr)
- [Step 10: Publish on GitHub Pages](#step-10-publish-on-github-pages)
- [Credits](#credits)

---

## Requirements
- Browser compatible with **WebGL/WebXR** (Chrome, Edge, Firefox; optional VR headset).
- **Equirectangular 360 images (2:1)**.
- **Python** (or any simple static server) to serve local files.

## Project Structure
```
vr-aframe-tutorial/
├─ index.html
├─ assets/
│  ├─ images/360/
│  ├─ models/
│  └─ audio/
└─ README.md
```

---

## Step 1: Basic Scene
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
    <a-box position="0 1 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
    <a-plane rotation="-90 0 0" width="10" height="10" color="#7BC8A4"></a-plane>
    <a-sky color="#ECECEC"></a-sky>
  </a-scene>
</body>
</html>
```

Useful primitives: `<a-box>`, `<a-sphere>`, `<a-cylinder>`, `<a-cone>`, `<a-plane>`, `<a-ring>`, `<a-torus>`, `<a-entity>`

*Note:* polyhedra are not native tags; use `geometry="primitive: dodecahedron"` on `<a-entity>` if needed.

---

## Step 2: Run Locally

Server:
```
python -m http.server 8000
```

Browser:
```
http://localhost:8000
```

---

## Step 3: 3D Model and 360 Environment

Model (inside `<a-scene>` using `<a-assets>`):
```
<a-assets>
  <a-asset-item id="model" src="assets/models/model.glb"></a-asset-item>
</a-assets>

<a-entity gltf-model="#model" position="0 0 -3" scale="1 1 1"></a-entity>
```

360 image (2:1):
```
<a-sky src="assets/images/360/360image.jpg" rotation="0 -90 0"></a-sky>
```

---

## Step 4: Interaction (mouse/gaze) and image toggle

Mouse raycast cursor:
```
<a-entity cursor="rayOrigin: mouse" raycaster="objects: .interactable"></a-entity>
```

Show/Hide image on click:
```
<a-box position="-4 1.5 -3" color="#FFFFFF" depth="0.2" height="1" width="1.5"
       class="interactable"
       onclick="const img=document.getElementById('revealed-image');img.setAttribute('visible', img.getAttribute('visible')==='false')">
</a-box>

<a-image id="revealed-image" src="assets/images/image1.jpg"
         position="-4 2.5 -3" width="3" height="2" visible="false"></a-image>
```

Random color on click:
```
<a-box class="interactable" position="3 1 -3" color="#4CC3D9"
       onclick="this.setAttribute('color','#'+Math.floor(Math.random()*16777215).toString(16).padStart(6,'0'))">
</a-box>
```

---

## Step 5: Simple Teleport

Component:
```
<script>
AFRAME.registerComponent('teleport-on-click', {
  init: function () {
    this.el.addEventListener('click', () => {
      const rig = document.querySelector('#rig');
      const p = document.querySelector('#teleport-point').object3D.position;
      if (rig) rig.object3D.position.copy(p);
    });
  }
});
</script>
```

Usage in the scene:
```
<a-entity id="rig" position="0 1.6 0">
  <a-entity camera look-controls></a-entity>
</a-entity>

<a-box class="interactable" position="-3 1 -3" color="#FFC107" teleport-on-click></a-box>
<a-entity id="teleport-point" position="5 1.6 0"></a-entity>
```

---

## Step 6: Animation and Audio
```
<!-- Animation -->
<a-box position="0 2 -5" rotation="0 45 45" scale="2 2 2"
       animation="property: object3D.position.y; to: 2.2; dir: alternate; dur: 2000; loop: true"></a-box>

<!-- On-demand audio -->
<a-sphere class="interactable" position="4 1.5 -3" radius="0.5" color="#3399FF"
          onclick="document.querySelector('#audio').components.sound.playSound()"></a-sphere>
<a-sound id="audio" src="assets/audio/sound.mp3" autoplay="false"></a-sound>
```

---

## Step 7: Materials
```
<!-- Metal -->
material="metalness: 1; roughness: 0; color: #888"

<!-- Glass / transparent -->
material="color: #AAF; opacity: 0.3; transparent: true; roughness: 0.1; metalness: 0.5"

<!-- Plastic -->
material="color: #FF00FF; metalness: 0; roughness: 0.3"
```

---

## Step 8: Shadows and Post-processing (optional)
```
<script src="https://cdn.jsdelivr.net/gh/supermedium/superframe@master/components/postprocessing/dist/aframe-postprocessing-component.min.js"></script>

<a-scene shadow="type: pcfsoft" postprocessing="bloom: true; ssao: true">
  <a-light type="directional" color="#fff" intensity="0.4" position="2 4 4" castShadow="true"></a-light>
  <a-light type="ambient" intensity="0.4"></a-light>

  <!-- Entity shadows -->
  <a-entity gltf-model="#model" shadow="cast: true; receive: true"></a-entity>
  <a-plane rotation="-90 0 0" width="10" height="10" color="#7BC8A4" shadow="receive: true"></a-plane>
</a-scene>
```

---

## Step 9: Hybrid Controls (PC + VR)
```
<!-- Extras: movement-controls (gamepad/keyboard) -->
<script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@latest/dist/aframe-extras.min.js"></script>

<a-entity id="rig" movement-controls="controls: gamepad,keyboard; speed: 0.2" position="0 1.6 0">
  <a-entity id="camera" camera look-controls></a-entity>
  <a-entity laser-controls="hand: left"  raycaster="objects: .interactable"></a-entity>
  <a-entity laser-controls="hand: right" raycaster="objects: .interactable"></a-entity>
</a-entity>
```

*iOS (Safari) tip:* for gyroscope control, add a button that requests permission via `DeviceMotionEvent.requestPermission()`.

---

## Step 10: Publish on GitHub Pages

Make sure you have `index.html` at the **root** (or inside `/docs`).

In your repository:
```
Settings → Pages
```

Source:
```
Deploy from a branch → main → /(root) or /docs → Save
```

Your URL will be:
```
https://YOUR-USERNAME.github.io/YOUR-REPO/
```

---

## Credits
- Author: **VV**
- Framework: https://aframe.io
