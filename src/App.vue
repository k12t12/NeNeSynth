<script setup>
import Oscillator from './components/Oscillator.vue'
import HorizontalModule from './components/HorizontalModule.vue'
import {ref} from 'vue'
import * as Tone from "tone";

// MIDI channels nums
const osc1AmpMIDIChannelNum = ref('-1')
const osc2AmpMIDIChannelNum = ref('-1')
const osc3AmpMIDIChannelNum = ref('-1')
const vibratoFreqMIDIChannelNum = ref('-1')
const vibratoDepthMIDIChannelNum = ref('-1')
const filterFreqMIDIChannelNum = ref('-1')
const filterQMIDIChannelNum = ref('-1')
const reverbWetMIDIChannelNum = ref('-1')
const reverbDecayMIDIChannelNum = ref('-1')

const gain = ref('')
gain.value = 1
//create and connect sound objects
const gainNode  = new Tone.Gain(1)

const reverb = new Tone.Reverb(5).connect(gainNode)
const filter = new Tone.Filter(null, "lowpass").connect(reverb)
const vibrato = new Tone.Vibrato(null, null).connect(filter)
const generator1 = new Tone.Oscillator(null, "square").connect(vibrato)
const generator2 = new Tone.Oscillator(null, "square").connect(vibrato)
const generator3 = new Tone.Oscillator(null, "square").connect(vibrato)

gainNode.toDestination()

function play() {
    Tone.start()
    generator1.start()
    generator2.start()
    generator3.start()
}

function stop() {
  generator1.stop()
  generator2.stop()
  generator3.stop()
}

function setGain(e){
  console.log(osc1AmpMIDIChannelNum.value)
  gain.value = e.target.value
  gainNode.set({
    gain: gain.value
  })
}

</script>

<template>

  
  <div class = 'synth'> 
    
  <h1> NENESYNTH </h1>

  <div class = 'oscs'> 
  <Oscillator :module = "generator1" :moduleName = " 'osc1' " :defaultAmp = -20 :defaultFreq = 20 :MIDIchannel ="osc1AmpMIDIChannelNum">  </Oscillator>
  <Oscillator :module = "generator2" :moduleName = " 'osc2' " :defaultAmp = -100 :defaultFreq = 10 :MIDIchannel ="osc2AmpMIDIChannelNum">  </Oscillator>
  <Oscillator :module = "generator3" :moduleName = " 'osc3' " :defaultAmp = -100 :defaultFreq = 10 :MIDIchannel ="osc3AmpMIDIChannelNum">  </Oscillator>
  </div>

 <HorizontalModule :module = "vibrato" :moduleName = " 'vibrato' " :controllers = "['depth', 'frequency']"
  :firstControllerRange = [0,1] :secondControllerRange =[0,4] :firstControllerStep = 0.01
   :secondControllerStep = 0.1 :firstControllerDefault = 1 :secondControllerDefault = 0.3
    :firstControllerChannel = "vibratoDepthMIDIChannelNum" :secondControllerChannel = "vibratoFreqMIDIChannelNum">
 </HorizontalModule>

 <HorizontalModule :module = "filter" :moduleName = " 'low filter' " :controllers = "['Q', 'frequency']"
  :firstControllerRange = [0,10] :secondControllerRange =[0,5000] :firstControllerStep = 0.1
   :secondControllerStep = 1 :firstControllerDefault = 10 :secondControllerDefault = 500
    :firstControllerChannel = "filterQMIDIChannelNum" :secondControllerChannel = "filterFreqMIDIChannelNum">
 </HorizontalModule>

 <HorizontalModule :module = "reverb" :moduleName = " 'reverb' " :controllers = "['wet', 'decay']"
  :firstControllerRange = [0,1] :secondControllerRange =[1,10] :firstControllerStep = 0.01
   :secondControllerStep = 0.01 :firstControllerDefault = 1 :secondControllerDefault = 100
    :firstControllerChannel = "reverbWetMIDIChannelNum" :secondControllerChannel = "reverbDecayMIDIChannelNum">
 </HorizontalModule> 
  
</div>

<div class = 'menu'>
    <button @click="play()"> PLAY </button>
    <button @click="stop()"> STOP </button>
    <div>
    <div> GAIN </div>
    <input class = 'gain-range' type = 'range' min = '0' max ='2' step = '0.2' :value = "gain" @input="setGain"> 
    <div> {{ gain*10 }} </div>
    </div>

    <div class="MIDI-channels-change">    
      <div> MIDI CHANNELS: </div> 
      <span> osc1 amp</span> <input type = 'number' v-model ="osc1AmpMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> osc2 amp</span> <input type = 'number' v-model ="osc2AmpMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> osc3 amp</span> <input type = 'number' v-model ="osc3AmpMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> vibrato freq</span> <input type = 'number' v-model ="vibratoFreqMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> vibrato depth</span> <input type = 'number' v-model ="vibratoDepthMIDIChannelNum" min = '-1' max = '15' value="-1">  
      <span> filter freq</span> <input type = 'number' v-model ="filterFreqMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> filter Q</span> <input type = 'number' v-model ="filterQMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> reverb wet</span> <input type = 'number' v-model ="reverbWetMIDIChannelNum" min = '-1' max = '15' value="-1"> 
      <span> reverb decay</span> <input type = 'number' v-model ="reverbDecayMIDIChannelNum" min = '-1' max = '15' value="-1"> 
    </div>


</div>
</template>
<style>

</style>