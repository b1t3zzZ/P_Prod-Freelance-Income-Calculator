<script setup>
import {ref} from 'vue';

const activeType = ref('');

const props = defineProps({
  SetDuration: {
    type: Function,
    required: true
  }
})

function ChangeActive(val) {
  if (val === 'Jour') {
    activeType.value = 'Jour';
  }
  else if (val === 'Heure') {
    activeType.value = 'Heure';
  }
}
</script>

<template>
  <nav>
    <ul>
      <li @click="SetDuration('Jour'), ChangeActive('Jour')" 
      :class="{ StayRed: activeType === 'Jour' }">Day</li>

      <li @click="SetDuration('Heure'), ChangeActive('Heure')" 
      :class="{ StayRed: activeType === 'Heure' }">Hour</li>
    </ul>
  </nav>
</template>

<style scoped>
ul {
  padding: 0;
  margin: 0;
  text-decoration: none;
  display: flex;
  justify-content: space-around;
  gap: 20px;
  list-style: none;
}
li{
  cursor: pointer;
  font-size: 16px;
  margin: 10px 0;
  padding: 10px 20px;
  border: 1px solid black;
  border-radius: 15px;
}
li:hover{
  color: red;
  border: 1px solid red;
  box-shadow: 0px 0px 6px red;
}
li.StayRed{
  color: red;
  border: 1px solid red;
  box-shadow: 0px 0px 6px red;
}
</style>