<script setup>
import {ref} from 'vue'

const props = defineProps({
  module: Object,
  moduleName: String,
  controllers: Array,
  firstControllerRange: Array,
  secondControllerRange: Array,
  firstControllerStep: Number,
  secondControllerStep: Number,
  firstControllerDefault: Number,
  secondControllerDefault: Number,
  firstControllerChannel: Number,
  secondControllerChannel: Number
})

const module = props.module
const controllers = props.controllers
const firstControllerValue = ref('')
const secondControllerValue = ref('')
firstControllerValue.value = props.firstControllerDefault
secondControllerValue.value = props.secondControllerDefault

//MIDI input
navigator.requestMIDIAccess()
    .then(onMIDISuccess, onMIDIFailure);
function onMIDIFailure() {
    console.log(" MIDI = ):");
}    
function onMIDISuccess(midiAccess) {
    const inputs = midiAccess.inputs;
    inputs.forEach(input => {
        input.onmidimessage = changeControllersValueMIDI;
    });
}

//controllers init
const set = {}
set[controllers[0]] = firstControllerValue.value
set[controllers[1]] = secondControllerValue.value
module.set(set)

function changeControllersValueMIDI(e){
  
  if (props.firstControllerChannel == e.data[0]-176) {
    firstControllerValue.value = e.data[2] / (127/props.firstControllerRange[1])
    const set = {}
    set[controllers[0]] = firstControllerValue.value
    module.set(set)
  }
  
  if (props.secondControllerChannel == e.data[0]-176) {
    secondControllerValue.value = e.data[2] / (127/props.secondControllerRange[1])
    if (controllers[1] == 'decay' && secondControllerValue.value < 0.01) {secondControllerValue.value = 0.01 }
    const set = {}
    set[controllers[1]] = secondControllerValue.value
    module.set(set)
  }
  
  
}

function changeFirstControllerValue(e){
  firstControllerValue.value = e.target.value
  const set = {}
  set[controllers[0]] = firstControllerValue.value
  module.set(set)
}

function changeSecondControllerValue(e){
  secondControllerValue.value = e.target.value
  const set = {}
  set[controllers[1]] = secondControllerValue.value
  module.set(set)
}

</script>

<template>
  
  <div class = 'horizontal-module'>
  <h3 class = 'module-name'> {{ moduleName }} </h3>
  
  <input class  = 'horizontal-range range' :value="firstControllerValue" @input="changeFirstControllerValue" type="range"
   :min = 'firstControllerRange[0]' :max = 'firstControllerRange[1]' :step = 'firstControllerStep'/>

  <div class = 'horizontal-label1'>  {{ controllers[0] }} </div>

  <input class = 'horizontal-range range' :value="secondControllerValue" @input="changeSecondControllerValue"  type="range"
   :min = 'secondControllerRange[0]' :max = 'secondControllerRange[1]' :step = 'secondControllerStep'/>

   <div class = 'horizontal-label2'>  {{ controllers[1] }}</div>

  </div>
</template>

<style> 
  .module-name {
    margin-left: 100px;
    margin-bottom: 10px;
    margin-top: 4px

  }
</style>