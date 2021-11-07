<template>
  <div class="element">
    <a-scene
      v-pre
      embedded
      vr-mode-ui="enterARButton: #ar-button;"
      ar-hit-test="target:#vase;"
    >
      <a-camera position="0 1.4 0.6"></a-camera>

      <a-gltf-model
        id="vase"
        src="/assets/vase/f1980_193-150k-4096.gltf"
        position="0 0 0"
        scale="1 1 1"
        hide-on-hit-test-start
      ></a-gltf-model>

      <a-light type="ambient" intensity="1"></a-light>
      <a-light
        type="directional"
        intensity="2"
        id="light"
        target="vase"
        position="-5 3 1.5"
      ></a-light>
    </a-scene>
  </div>
</template>

<script>
export default {
  name: "Vase",
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