<template>
  <canvas class="element" width="300px" height="300px"></canvas>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
export default {
  name: "Auto",
  computed: {
    publicPath() {
      return process.env.BASE_URL;
    },
  },
  methods: {
    loadScene() {
      const canvas = document.querySelector(".element");

      const scene = new THREE.Scene();
      const path = `${this.publicPath}assets/lancia/scene.gltf`;
      const loader = new GLTFLoader();
      loader.load(
        path,
        (gltf) => {
          const gltfScene = gltf.scene;
          gltfScene.scale.set(1, 1, 1);
          gltfScene.rotation.set(0, 2.7, 0);
          gltfScene.position.set(0.7, 0, 0);
          scene.add(gltfScene);
        },
        null,
        (error) => {
          console.error("error in gltf loading process", error);
        }
      );

      const directionalLight = new THREE.DirectionalLight(0xffffff, 20);
      directionalLight.position.set(0.5, 1, 1);
      scene.add(directionalLight);

      const camera = new THREE.PerspectiveCamera(
        75,
        canvas.width / canvas.height
      );
      camera.position.set(1, 0.5, 3);
      scene.add(camera);

      scene.background = new THREE.Color(0xffffff);

      const renderer = new THREE.WebGL1Renderer({
        canvas: canvas,
        antialias: true,
      });
      renderer.setSize(canvas.width, canvas.height);

      function animate() {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      }

      animate();
    },
  },
  mounted() {
    this.loadScene();
  },
};
</script>

<style scoped>
.element {
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
</style>