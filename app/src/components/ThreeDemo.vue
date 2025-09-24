<template>
  <div ref="threeContainer" style="width: 100%; height: 400px; background: #222;"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';

const threeContainer = ref(null);
let renderer, scene, camera, cube, animationId;

const initThree = () => {
  const width = threeContainer.value.clientWidth || 600;
  const height = threeContainer.value.clientHeight || 400;

  // Scene
  scene = new THREE.Scene();

  // Camera
  camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
  camera.position.z = 5;

  // Renderer
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(width, height);
  renderer.setClearColor(0x222222);
  threeContainer.value.appendChild(renderer.domElement);

  // Cube
  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  // Animate
  const animate = () => {
    animationId = requestAnimationFrame(animate);
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
  };
  animate();
};

onMounted(() => {
  initThree();
});

onBeforeUnmount(() => {
  cancelAnimationFrame(animationId);
  if (renderer) renderer.dispose();
});
</script>