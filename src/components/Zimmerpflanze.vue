<template>
  <div class="element">
    <a-scene
      v-pre
      embedded
      vr-mode-ui="enterARButton: #ar-button;"
      ar-hit-test="target:#zimmerpflanze;"
      webxr="optionalFeatures: dom-overlay; overlayElement: #dom-overlay"
      exit-ar-on-click
    >
      <a-camera position="0 1.4 0.6"></a-camera>

      <a-gltf-model
        id="zimmerpflanze"
        src="/assets/zimmerpflanze/scene.gltf"
        position="0 0 0"
        scale="0.1 0.1 0.1"
        hide-on-hit-test-start
        increase-on-click
        decrease-on-click
        turn-left-on-click
        turn-right-on-click
      ></a-gltf-model>

      <a-light type="hemisphere" intensity="3"></a-light>
      <a-light
        type="directional"
        intensity="2"
        id="light"
        target="zimmerpflanze"
        position="-5 3 1.5"
      ></a-light>
      <a-light
        type="directional"
        intensity="2"
        id="light"
        target="zimmerpflanze"
        position="5 3 1.5"
      ></a-light>
      <a-light
        type="directional"
        intensity="4"
        id="light"
        target="zimmerpflanze"
        position="-5 -3 -1.5"
      ></a-light>
    </a-scene>
  </div>
</template>

<script>
export default {
  name: "Zimmerpflanze",
  mounted() {
    const model = document.getElementById("zimmerpflanze");
    model.addEventListener("model-loaded", () => {
      this.$emit("modelLoaded");
    });
  },
};
</script>

<style scoped>
.element {
  width: 300px;
  height: 300px;
  border-style: ridge;
  border-color: lightgrey;
  border-radius: 15px;
}
a-scene {
  width: 300px;
  height: 300px;
}
</style>