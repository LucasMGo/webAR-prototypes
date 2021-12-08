<template>
  <canvas id="element"></canvas>
</template>

<script>
/* eslint-disable no-undef */
/* eslint-disable no-unused-vars */
import * as BABYLON from "babylonjs";
import "babylonjs-loaders";
export default {
  name: "Tischlampe",
  data() {
    return {
      engine: null,
      scene: null,
      model: null,
    };
  },
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
  },
  methods: {
    async createScene() {
      const scene = new BABYLON.Scene(this.engine);

      const camera = new BABYLON.FreeCamera(
        "camera1",
        new BABYLON.Vector3(0, 1, -5),
        scene
      );
      camera.setTarget(BABYLON.Vector3.Zero());
      camera.attachControl(this.canvas, true);

      const light = new BABYLON.HemisphericLight(
        "light",
        new BABYLON.Vector3(0, 1, 0),
        scene
      );
      light.intensity = 0.7;

      const dirLight = new BABYLON.DirectionalLight(
        "dirlight",
        new BABYLON.Vector3(0, -1, -0.5),
        scene
      );
      dirLight.position = new BABYLON.Vector3(0, 5, -5);

      const xr = await scene.createDefaultXRExperienceAsync({
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

      this.model = await BABYLON.SceneLoader.ImportMeshAsync(
        "",
        "/assets/lampe/",
        "scene.gltf",
        scene
      );

      const ground = BABYLON.Mesh.CreateGround("ground1", 0.3, 0.3, 1, scene);
      const groundMaterial = new BABYLON.StandardMaterial(scene);
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

      scene.onPointerDown = () => {
        if (
          hitTestResult &&
          xr.baseExperience.state === BABYLON.WebXRState.IN_XR
        ) {
          hitTestResult.transformationMatrix.decompose(
            undefined,
            this.modelMesh.rotationQuaternion,
            this.modelMesh.position
          );
          this.enableModel();
        }
      };

      return scene;
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

    this.scene = await this.createScene();

    this.engine.runRenderLoop(() => {
      this.scene.render();
    });
  },
};
</script>

<style scoped>
#element {
  width: 100%;
  height: 100%;
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
</style>