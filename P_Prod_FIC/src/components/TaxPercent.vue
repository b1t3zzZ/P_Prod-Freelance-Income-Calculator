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
div{
  display: flex;
  justify-content: center;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  /* display: none; <- Crashes Chrome on hover */
  -webkit-appearance: none;
  margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
}
</style>