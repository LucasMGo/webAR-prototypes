<template>
  <canvas class="element" width="300px" height="500px"></canvas>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
export default {
  name: "Zimmerpflanze",
  computed: {
    publicPath() {
      return process.env.BASE_URL;
    },
  },
  methods: {
    loadScene() {
      const canvas = document.querySelector(".element");

      const scene = new THREE.Scene();
      const path = `${this.publicPath}assets/zimmerpflanze/scene.gltf`;
      const loader = new GLTFLoader();
      loader.load(
        path,
        (gltf) => {
          const gltfScene = gltf.scene;
          gltfScene.scale.set(0.3, 0.3, 0.3);
          gltfScene.position.set(0, -0.2, 0.2);
          scene.add(gltfScene);
        },
        null,
        (error) => {
          console.log("error in gltf loading process", error);
        }
      );

      const light = new THREE.PointLight(0xffffff, 9, 20);
      light.position.set(0, 1, 1);
      scene.add(light);

      const camera = new THREE.PerspectiveCamera(
        75,
        canvas.width / canvas.height
      );
      camera.position.set(0, 1, 2);
      scene.add(camera);

      scene.background = new THREE.Color(0xffffff);

      const renderer = new THREE.WebGL1Renderer({
        canvas: canvas,
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