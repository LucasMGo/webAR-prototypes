<template>
  <v-container class="ma-md-12">
    <v-row justify="start">
      <v-col cols="auto">
        <h1 class="font-weight-light">{{ name }}</h1>
      </v-col>
      <v-col cols="auto" id="ar-button"> </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" lg="6" class="font-weight-light">
        <p>
          {{ description }}
        </p>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col cols="12">
        <div class="element">
          <div id="dom-overlay">
            <div id="button-wrapper">
              <v-row no-gutters>
                <v-col cols="auto">
                  <v-btn outlined id="increase-button" @click="increase()">
                    Size +25%
                    <v-icon large>mdi-plus-box-outline</v-icon>
                  </v-btn>
                </v-col>
                <v-col cols="auto">
                  <v-btn outlined id="decrease-button" @click="decrease()">
                    Size -25%
                    <v-icon large>mdi-minus-box-outline</v-icon>
                  </v-btn>
                </v-col>
                <v-spacer></v-spacer>
                <v-col cols="auto">
                  <v-btn outlined id="turnleft-button" @click="turnleft()">
                    Turn
                    <v-icon large>mdi-arrow-left-top-bold</v-icon>
                  </v-btn>
                </v-col>
                <v-col cols="auto">
                  <v-btn outlined id="turnright-button" @click="turnright()">
                    Turn
                    <v-icon large>mdi-arrow-right-top-bold</v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </div>
          </div>
        </div>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col cols="12" lg="6">
        <em class="credit">{{ credit }}</em>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import products from "../data/products.json";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { ARButton } from "three/examples/jsm/webxr/ARButton";
export default {
  name: "Product",
  components: {},
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
    productInfo() {
      const thisProduct = products[this.$router.currentRoute.params.id];
      return thisProduct;
    },
    description() {
      return this.productInfo.description;
    },
    name() {
      return this.productInfo.name;
    },
    credit() {
      return this.productInfo.credit;
    },
  },
  methods: {
    loadGltf() {
      const path = `${this.publicPath}assets/${this.name}/scene.gltf`;
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
    setupZimmerpflanze() {
      this.gltfScene.scale.set(0.1, 0.1, 0.1);
      this.gltfScene.position.set(0, 0.6, 0);
      this.scene.add(this.gltfScene);

      const light = new THREE.PointLight(0xffffff, 9, 20);
      light.position.set(0, 1, 1);
      this.scene.add(light);

      this.camera = new THREE.PerspectiveCamera(25, 1, 0.01, 20);
      this.camera.position.set(0, 1, 2);
      this.scene.add(this.camera);
    },
    setupVase() {
      this.gltfScene.scale.set(1, 1, 1);
      this.gltfScene.position.set(0, 1, 0);
      this.scene.add(this.gltfScene);

      const light = new THREE.PointLight(0xffffff, 3, 10);
      light.position.set(0, 1, 2);
      this.scene.add(light);

      this.camera = new THREE.PerspectiveCamera(15, 1, 0.01, 20);
      this.camera.position.set(0, 1, 2);
      this.scene.add(this.camera);
    },
    setupTischlampe() {
      this.gltfScene.scale.set(1, 1, 1);
      this.gltfScene.position.set(0, -0.15, 0);
      this.gltfScene.rotation.set(0, 0, 0);
      this.scene.add(this.gltfScene);

      const light = new THREE.PointLight(0xffffff, 5, 10);
      light.position.set(0, 0.2, 0.4);
      this.scene.add(light);

      this.camera = new THREE.PerspectiveCamera(75, 1, 0.01, 20);
      this.camera.position.set(0, 0.07, 0.4);
      this.scene.add(this.camera);
    },
    setupAuto() {
      this.gltfScene.scale.set(1, 1, 1);
      this.gltfScene.rotation.set(0, 2.7, 0);
      this.gltfScene.position.set(0.7, 0, 0);
      this.scene.add(this.gltfScene);

      const directionalLight = new THREE.DirectionalLight(0xffffff, 20);
      directionalLight.position.set(0.5, 1, 1);
      this.scene.add(directionalLight);

      this.camera = new THREE.PerspectiveCamera(70, 2, 0.01, 20);
      this.camera.position.set(1, 0.5, 3);
      this.scene.add(this.camera);
    },
    setupModelLayout() {
      this[`setup${this.name}`]();
    },
    loadScene() {
      const container = document.querySelector(".element");

      this.scene = new THREE.Scene();

      this.setupModelLayout();
      this.renderer = new THREE.WebGL1Renderer({
        alpha: true,
        antialias: true,
      });
      this.renderer.setSize(container.clientWidth, container.clientWidth);
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.xr.enabled = true;
      container.appendChild(this.renderer.domElement);

      let options = {
        requiredFeatures: ["hit-test"],
        optionalFeatures: ["dom-overlay"],
        domOverlay: { root: document.getElementById("dom-overlay") },
      };

      const buttonDiv = document.getElementById("ar-button");
      const button = ARButton.createButton(this.renderer, options);

      button.addEventListener("click", () => {
        this.gltfScene.visible = false;
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

      // prevent touch event to be misinterpreted as a hit test placement -> touching inside of button wrapper region should only trigger corresponding action (resize, turn)
      document
        .getElementById("button-wrapper")
        .addEventListener("beforexrselect", (e) => {
          e.preventDefault();
        });
      this.animate();
    },
    onSelect() {
      if (this.reticle.visible) {
        document.getElementById("button-wrapper").style.display = "block";
        this.gltfScene.visible = true;
        this.gltfScene.position.setFromMatrixPosition(this.reticle.matrix);
      }
    },
    increase() {
      this.gltfScene.scale.x = this.gltfScene.scale.x * 1.25;
      this.gltfScene.scale.y = this.gltfScene.scale.y * 1.25;
      this.gltfScene.scale.z = this.gltfScene.scale.z * 1.25;
    },
    decrease() {
      this.gltfScene.scale.x = this.gltfScene.scale.x / 1.25;
      this.gltfScene.scale.y = this.gltfScene.scale.y / 1.25;
      this.gltfScene.scale.z = this.gltfScene.scale.z / 1.25;
    },
    turnleft() {
      this.gltfScene.rotateY(-Math.PI / 2);
    },
    turnright() {
      this.gltfScene.rotateY(Math.PI / 2);
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
.credit {
  font-size: 0.8em;
}

.element {
  width: 50%;
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
#button-wrapper {
  display: none;
}
</style>