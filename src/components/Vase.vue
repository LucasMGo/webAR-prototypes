<template>
  <div class="element"></div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { ARButton } from "three/examples/jsm/webxr/ARButton";
export default {
  name: "Vase",
  data: () => ({
    gltfScene: null,
    renderer: null,
    scene: null,
    camera: null,
    reticle: null,
  }),
  computed: {
    publicPath() {
      return process.env.BASE_URL;
    },
  },
  methods: {
    loadGltf() {
      const path = `${this.publicPath}assets/vase/f1980_193-150k-4096.gltf`;
      const loader = new GLTFLoader();
      return new Promise((resolve, reject) => {
        loader.load(
          path,
          (gltf) => {
            this.gltfScene = gltf.scene;
            resolve();
          },
          (xhr) => {
            console.log("loaded: ", xhr.loaded / xhr.total);
          },
          (error) => {
            console.log("error in gltf loading process", error);
            reject(error);
          }
        );
      });
    },
    loadScene() {
      const container = document.querySelector(".element");

      this.scene = new THREE.Scene();

      const gltfScene = this.gltfScene;
      gltfScene.scale.set(1, 1, 1);
      gltfScene.position.set(0, 1, 0);
      this.scene.add(gltfScene);

      const light = new THREE.PointLight(0xffffff, 3, 10);
      light.position.set(0, 1, 2);
      this.scene.add(light);

      this.camera = new THREE.PerspectiveCamera(15, 1, 0.01, 20);
      this.camera.position.set(0, 1, 2);
      this.scene.add(this.camera);

      this.renderer = new THREE.WebGL1Renderer({
        alpha: true,
        antialias: true,
      });
      this.renderer.setSize(container.clientWidth, container.clientWidth);
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.xr.enabled = true;
      container.appendChild(this.renderer.domElement);

      const buttonDiv = document.getElementById("ar-button");
      const button = ARButton.createButton(this.renderer, {
        requiredFeatures: ["hit-test"],
      });

      button.addEventListener("click", () => {
        gltfScene.visible = false;
      });
      button.style.position = "static";
      button.style.top = 0;
      button.style.bottom = 0;
      button.style.left = 0;
      button.style.right = 0;
      buttonDiv.appendChild(button);

      const controller = this.renderer.xr.getController(0);
      controller.addEventListener("select", this.onSelect);
      this.scene.add(controller);

      this.reticle = new THREE.Mesh(
        new THREE.PlaneGeometry(0.3, 0.3).rotateX(-Math.PI / 2),
        new THREE.MeshBasicMaterial({
          color: 0x727272,
          opacity: 0.5,
          transparent: true,
        })
      );
      this.reticle.matrixAutoUpdate = false;
      this.reticle.visible = false;
      this.scene.add(this.reticle);

      this.animate();
    },
    onSelect() {
      console.log("Select new position");
      if (this.reticle.visible) {
        this.gltfScene.visible = true;
        this.gltfScene.position.setFromMatrixPosition(this.reticle.matrix);
      }
    },
    animate() {
      let hitTestSource = null;
      this.renderer.setAnimationLoop(async (timestamp, frame) => {
        if (frame) {
          const referenceSpace = this.renderer.xr.getReferenceSpace();
          const session = this.renderer.xr.getSession();

          if (hitTestSource) {
            const hitTestResults = frame.getHitTestResults(hitTestSource);

            if (hitTestResults.length) {
              const hit = hitTestResults[0];

              this.reticle.visible = true;
              this.reticle.matrix.fromArray(
                hit.getPose(referenceSpace).transform.matrix
              );
            } else {
              this.reticle.visible = false;
            }
          } else {
            const sessionReferenceSpace = await session.requestReferenceSpace(
              "viewer"
            );
            hitTestSource = await session.requestHitTestSource({
              space: sessionReferenceSpace,
            });

            session.addEventListener("end", function (event) {
              console.log("End session", event);
              hitTestSource = null;
            });
          }
        }

        this.renderer.render(this.scene, this.camera);
      });
    },
  },
  async mounted() {
    await this.loadGltf();
    this.loadScene();
  },
};
</script>

<style scoped>
.element {
  width: 50%;
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
</style>