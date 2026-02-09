<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>3D Planet Explorer</title>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>

<script src="https://cdn.jsdelivr.net/npm/three@0.152.2/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.152.2/examples/js/controls/OrbitControls.js"></script>

<script>
  // Scene
  const scene = new THREE.Scene();

  // Camera
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.z = 4;

  // Renderer
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  // Controls (exploration)
  const controls = new THREE.OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.enableZoom = true;

  // Light
  const light = new THREE.PointLight(0xffffff, 1);
  light.position.set(5, 5, 5);
  scene.add(light);

  const ambientLight = new THREE.AmbientLight(0x333333);
  scene.add(ambientLight);

  // Planet
  const textureLoader = new THREE.TextureLoader();
  const planetTexture = textureLoader.load(
    "https://threejs.org/examples/textures/planets/earth_atmos_2048.jpg"
  );

  const geometry = new THREE.SphereGeometry(1.5, 64, 64);
  const material = new THREE.MeshStandardMaterial({ map: planetTexture });
  const planet = new THREE.Mesh(geometry, material);
  scene.add(planet);

  // Animate
  function animate() {
    requestAnimationFrame(animate);
    planet.rotation.y += 0.0005; // slow spin
    controls.update();
    renderer.render(scene, camera);
  }
  animate();

  // Resize
  window.addEventListener("resize", () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
</script>

</body>
</html>
