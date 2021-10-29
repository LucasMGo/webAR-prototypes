<template>
  <canvas class="element" width="300px" height="500px"></canvas>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
export default {
  name: "Vase",
  computed: {
    publicPath() {
      return process.env.BASE_URL;
    },
  },
  methods: {
    loadScene() {
      const canvas = document.querySelector(".element");

      const scene = new THREE.Scene();
      const path = `${this.publicPath}assets/vase/f1980_193-150k-4096.gltf`;
      const loader = new GLTFLoader();
      loader.load(
        path,
        (gltf) => {
          const gltfScene = gltf.scene;
          gltfScene.scale.set(5, 5, 5);
          gltfScene.position.set(0, 1, 0);
          scene.add(gltfScene);
        },
        null,
        (error) => {
          console.log("error in gltf loading process", error);
        }
      );

      const light = new THREE.PointLight(0xffffff, 3, 10);
      light.position.set(0, 1, 2);
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