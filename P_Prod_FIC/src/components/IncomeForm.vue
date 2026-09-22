<script setup>

import { ref } from 'vue';

const activeType = ref('');


const props = defineProps({
  SetForm: {
    type: Function,
    required: true
  }
})

function ChangeActive(val) {
  if (val === 'Total') {
    activeType.value = 'Total';
  }
  else if (val === 'CHFheure') {
    activeType.value = 'CHFheure';
  }
}
</script>

<template>
  <nav>
    <ul>

      <li @click="SetForm('Total'), ChangeActive('Total')" 
      :class="{ StayRed: activeType === 'Total' }">Price Total</li>

      <li @click="SetForm('CHFheure'), ChangeActive('CHFheure')" 
      :class="{ StayRed: activeType === 'CHFheure' }">CHF per
        hour</li>

    </ul>
  </nav>
</template>

<style scoped>
ul {
  display: flex;
  justify-content: space-around;
  gap: 20px;
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  cursor: pointer;
  font-size: 16px;
  margin: 10px 0;
  padding: 10px 20px;
  border: 1px solid black;
  border-radius: 15px;
}

li:hover {
  color: red;
  border: 1px solid red;
  box-shadow: 0px 0px 6px red;
}

li.StayRed {
  color: red;
  border: 1px solid red;
  box-shadow: 0px 0px 6px red;
}
</style>