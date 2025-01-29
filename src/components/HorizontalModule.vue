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
  secondControllerDefault: Number
})

const module = props.module
const controllers = props.controllers
const firstControllerValue = ref('')
const secondControllerValue = ref('')
firstControllerValue.value = props.firstControllerDefault
secondControllerValue.value = props.secondControllerDefault

//init
const set = {}

set[controllers[0]] = firstControllerValue.value
set[controllers[1]] = secondControllerValue.value

module.set(set)
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