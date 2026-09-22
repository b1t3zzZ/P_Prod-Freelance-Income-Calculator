<script setup>
import {ref} from 'vue';

let numberOfInput = ref(0);

const props = defineProps({
  SetTax: {
    type: Function,
    required: true
  },
  SetCommission: {
    type: Function,
    required: true
  }
})

function BlockInput(){
  if(numberOfInput.value > 100){
    numberOfInput.value = 100;
  }
  if(numberOfInput.value < 0 || numberOfInput.value == null || numberOfInput.value == undefined || numberOfInput.value == ""){
    numberOfInput.value = 0;
  }

  props.SetTax(numberOfInput.value);
  props.SetCommission(numberOfInput.value);
}

</script>

<template>

  <div>
    <input v-model.number="numberOfInput" type="number" min="0" max="100" @input="BlockInput" placeholder="0">
  </div>
  

</template>

<style scoped>

input {
    padding: 5px;
    border-radius: 5px;
    border: 2px solid rgb(168, 168, 168);
    width: 97%;
}

</style>