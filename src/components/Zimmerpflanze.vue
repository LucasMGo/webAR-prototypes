<template>
  <div class="element">
    <a-scene
      v-pre
      embedded
      vr-mode-ui="enterARButton: #ar-button;"
      ar-hit-test="target:#zimmerpflanze;"
    >
      <a-camera position="0 1.4 0.6"></a-camera>

      <a-gltf-model
        id="zimmerpflanze"
        src="/assets/zimmerpflanze/scene.gltf"
        position="0 0 0"
        scale="0.1 0.1 0.1"
        hide-on-hit-test-start
      ></a-gltf-model>

      <a-light type="ambient" intensity="1"></a-light>
      <a-light
        type="point"
        intensity="4"
        id="light"
        target="zimmerpflanze"
        position="-5 3 1.5"
      ></a-light>
    </a-scene>
  </div>
</template>

<script>
export default {
  name: "Zimmerpflanze",
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