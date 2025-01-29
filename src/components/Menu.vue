<script setup>
const props = defineProps({
  Tone: Object
})

function play() {
  props.Tone.start()
}
function stop() {
  props.Tone.stop()
}
</script>

<template>
    <button @click="play()"> play </button>
    <button @click="stop()"> stop </button>
</template>