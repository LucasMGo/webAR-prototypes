<template>
  <v-container class="ma-md-12">
    <v-row
      ><v-col cols="12"
        ><h1 class="font-weight-light">{{ name }}</h1></v-col
      ></v-row
    >
    <v-row>
      <v-col cols="12" lg="6" class="font-weight-light">
        <p>
          {{ description }}
        </p>
      </v-col>
    </v-row>
    <v-row no-gutters class="mt-6">
      <v-col cols="12" lg="6">
        <canvas id="element">
          <div id="dom-overlay">
            <div id="button-wrapper">
              <v-row no-gutters>
                <v-col cols="auto">
                  <v-btn
                    outlined
                    id="increase-button"
                    @click="increase($event)"
                  >
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
        </canvas>
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
/* eslint-disable no-undef */
/* eslint-disable no-unused-vars */
import * as BABYLON from "babylonjs";
import "babylonjs-loaders";
import products from "../data/products.json";
export default {
  name: "Product",
  data() {
    return {
      engine: null,
      scene: null,
      model: null,
    };
  },
  components: {},
  computed: {
    modelMesh() {
      if (this.model && this.model.meshes) {
        return this.model.meshes[0];
      }
      return null;
    },
    canvas() {
      return document.getElementById("element");
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
    increase(e) {
      this.modelMesh.scaling.x = this.modelMesh.scaling.x * 1.25;
      this.modelMesh.scaling.y = this.modelMesh.scaling.y * 1.25;
      this.modelMesh.scaling.z = this.modelMesh.scaling.z * 1.25;
    },
    decrease() {
      this.modelMesh.scaling.x = this.modelMesh.scaling.x / 1.25;
      this.modelMesh.scaling.y = this.modelMesh.scaling.y / 1.25;
      this.modelMesh.scaling.z = this.modelMesh.scaling.z / 1.25;
    },
    turnleft() {
      this.modelMesh.rotationQuaternion = null;
      this.modelMesh.rotation.y =
        this.modelMesh.rotation.y - BABYLON.Tools.ToRadians(90);
    },
    turnright() {
      this.modelMesh.rotationQuaternion = null;
      this.modelMesh.rotation.y =
        this.modelMesh.rotation.y + BABYLON.Tools.ToRadians(90);
    },
    async loadModel() {
      this.model = await BABYLON.SceneLoader.ImportMeshAsync(
        "",
        `/assets/${this.name}/`,
        "scene.gltf",
        this.scene
      );
    },
    setupAuto() {
      const camera = new BABYLON.FreeCamera(
        "camera1",
        new BABYLON.Vector3(0, 1, -5),
        this.scene
      );
      camera.setTarget(BABYLON.Vector3.Zero());
      camera.attachControl(this.canvas, true);

      const light = new BABYLON.HemisphericLight(
        "light",
        new BABYLON.Vector3(0, 1, 0),
        this.scene
      );
      light.intensity = 1.5;

      const dirLight = new BABYLON.DirectionalLight(
        "dirlight",
        new BABYLON.Vector3(0, -1, -0.5),
        this.scene
      );
      dirLight.position = new BABYLON.Vector3(0, 5, -5);
    },
    setupZimmerpflanze() {
      const camera = new BABYLON.FreeCamera(
        "camera1",
        new BABYLON.Vector3(0, 1, -5),
        this.scene
      );
      camera.setTarget(BABYLON.Vector3.Zero());
      camera.attachControl(this.canvas, true);

      const light = new BABYLON.HemisphericLight(
        "light",
        new BABYLON.Vector3(0, 1, 0),
        this.scene
      );
      light.intensity = 1.5;

      const dirLight = new BABYLON.DirectionalLight(
        "dirlight",
        new BABYLON.Vector3(0, -1, -0.5),
        this.scene
      );
      dirLight.position = new BABYLON.Vector3(0, 5, -5);

      this.modelMesh.scaling = new BABYLON.Vector3(0.1, 0.1, 0.1);
    },
    setupTischlampe() {
      const camera = new BABYLON.FreeCamera(
        "camera1",
        new BABYLON.Vector3(0, 1, -5),
        this.scene
      );
      camera.setTarget(BABYLON.Vector3.Zero());
      camera.attachControl(this.canvas, true);

      const light = new BABYLON.HemisphericLight(
        "light",
        new BABYLON.Vector3(0, 1, 0),
        this.scene
      );
      light.intensity = 0.7;

      const dirLight = new BABYLON.DirectionalLight(
        "dirlight",
        new BABYLON.Vector3(0, -1, -0.5),
        this.scene
      );
      dirLight.position = new BABYLON.Vector3(0, 5, -5);
    },
    setupVase() {
      const camera = new BABYLON.FreeCamera(
        "camera1",
        new BABYLON.Vector3(0, 1, -5),
        this.scene
      );
      camera.setTarget(BABYLON.Vector3.Zero());
      camera.attachControl(this.canvas, true);

      const light = new BABYLON.HemisphericLight(
        "light",
        new BABYLON.Vector3(0, 1, 0),
        this.scene
      );
      light.intensity = 0.7;

      const dirLight = new BABYLON.DirectionalLight(
        "dirlight",
        new BABYLON.Vector3(0, -1, -0.5),
        this.scene
      );
      dirLight.position = new BABYLON.Vector3(0, 5, -5);
    },
    setupModelLayout() {
      this[`setup${this.name}`]();
    },
    async createScene() {
      this.scene = new BABYLON.Scene(this.engine);
      await this.loadModel();
      this.setupModelLayout();

      const xr = await this.scene.createDefaultXRExperienceAsync({
        uiOptions: {
          sessionMode: "immersive-ar",
        },
        optionalFeatures: true,
      });

      xr.baseExperience.sessionManager.onXRSessionInit.add(() => {
        this.disableModel();
      });
      xr.baseExperience.sessionManager.onXRSessionEnded.add(() => {
        this.enableModel();
        ground.setEnabled(false);
      });

      const fm = xr.baseExperience.featuresManager;
      const hitTest = fm.enableFeature(BABYLON.WebXRHitTest.Name, "latest");

      const domOverlayFeature = fm.enableFeature(
        BABYLON.WebXRDomOverlay,
        "latest",
        { element: "#dom-overlay" },
        undefined,
        false
      );

      const ground = BABYLON.Mesh.CreateGround(
        "ground1",
        0.3,
        0.3,
        1,
        this.scene
      );
      const groundMaterial = new BABYLON.StandardMaterial(this.scene);
      groundMaterial.alpha = 0.5;
      groundMaterial.diffuseColor = new BABYLON.Color3(0.83, 0.83, 0.83);
      ground.material = groundMaterial;
      ground.setEnabled(false);

      let hitTestResult;
      hitTest.onHitTestResultObservable.add((results) => {
        if (results.length) {
          ground.setEnabled(true);
          hitTestResult = results[0];
          hitTestResult.transformationMatrix.decompose(
            undefined,
            ground.rotationQuaternion,
            ground.position
          );
        } else {
          ground.setEnabled(false);
          hitTestResult = undefined;
        }
      });

      this.scene.onPointerDown = (event) => {
        if (
          hitTestResult &&
          xr.baseExperience.state === BABYLON.WebXRState.IN_XR &&
          event.path.length > 0
        ) {
          // if button was clicked: do not move model, only execute corresponding action: resize/rotate model
          const buttonClicked = !!event.path.find(
            (pathEntry) => pathEntry.id === "button-wrapper"
          );
          if (buttonClicked) {
            return;
          }
          document.getElementById("button-wrapper").style.display = "block";
          hitTestResult.transformationMatrix.decompose(
            undefined,
            this.modelMesh.rotationQuaternion,
            this.modelMesh.position
          );
          this.enableModel();
        }
      };
    },
    enableModel() {
      this.model.meshes.forEach((mesh) => {
        mesh.setEnabled(true);
      });
    },
    disableModel() {
      this.model.meshes.forEach((mesh) => {
        mesh.setEnabled(false);
      });
    },
  },
  async mounted() {
    this.engine = new BABYLON.Engine(this.canvas, true);

    await this.createScene();

    this.engine.runRenderLoop(() => {
      this.scene.render();
    });
  },
};
</script>

<style scoped>
.credit {
  font-size: 0.8em;
}
#element {
  width: 100%;
  height: 100%;
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
#button-wrapper {
  display: none;
}
</style>