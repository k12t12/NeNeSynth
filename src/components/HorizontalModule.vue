<script setup>
import {ref} from 'vue'

const props = defineProps({
  module: Object,
  moduleName: String,
  param: Array,
  range1: Array,
  range2: Array,
  step1: Number,
  step2: Number,
  default1: Number,
  default2: Number
})

const module = props.module
const param = props.param
const param1 = ref('')
const param2 = ref('')
param1.value = props.default1
param2.value = props.default2

//init
const set = {}

set[param[0]] = param1.value
set[param[1]] = param2.value

module.set(set)
function changeParam1(e){
  param1.value = e.target.value
  const set = {}
  set[param[0]] = param1.value
  module.set(set)
}

function changeParam2(e){
  param2.value = e.target.value
  const set = {}
  set[param[1]] = param2.value
  module.set(set)
}

</script>

<template>
  
  <div class = 'horizontal-module'>
  <h3 class = 'module-name'> {{ moduleName }} </h3>
  
  <input class  = 'horizontal-range range' :value="param1" @input="changeParam1" type="range"
   :min = 'range1[0]' :max = 'range1[1]' :step = 'step1'/>

  <div class = 'horizontal-label1'>  {{ param[0] }} </div>

  <input class = 'horizontal-range range' :value="param2" @input="changeParam2"  type="range"
   :min = 'range2[0]' :max = 'range2[1]' :step = 'step2'/>

   <div class = 'horizontal-label2'>  {{ param[1] }}</div>

  </div>
</template>

<style> 
  .module-name {
    margin-left: 100px;
    margin-bottom: 10px;
    margin-top: 4px

  }
</style>