<script setup>

import {ref} from 'vue'
const props = defineProps({
  module: Object,
  moduleName: String,
  defaultAmp: Number,
  defaultFreq: Number,
  MIDIchannel: Number
})

const osc = props.module
const freq = ref('')
const amp = ref('')
freq.value = props.defaultFreq
amp.value = props.defaultAmp

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

//init
osc.set({
    frequency: freq.value,
    volume: amp.value
  })
function changeControllersValueMIDI(e){
  if (props.MIDIchannel == e.data[0]-176) {
    amp.value = e.data[2] - 127
    console.log(e.data[2]-127)
    osc.set({
    volume: amp.value
    })
  }
}

function changeFrequency(e){
  freq.value = e.target.value
  osc.set({
    frequency: freq.value
  })

}

function changeAmplitude(e){
amp.value = e.target.value
osc.set({
  volume: amp.value
})

}
</script>

<template>
  <div class = 'module'>
  
  <h3 class="module-osc-name"> {{ moduleName }} </h3>
  <input class = 'vertical-range1 range' :value="amp" @input="changeAmplitude"   type="range" min = '-100' max = '0' step = '1'/>
  <div class = 'label-osc-amp'> amp </div>
  
  <input class = 'vertical-range2 range' :value="freq" @input="changeFrequency"  type="range" min = '10' max = '300' step = '10'/>
   <div class = 'label-osc-freq'> freq </div>
  </div>
</template>

<style> 
  .module-osc-name {
    margin-left: 25px
  }
</style>

