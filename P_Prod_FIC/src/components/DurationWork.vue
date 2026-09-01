<script setup>
const props = defineProps({
  SetDuration:{
    type:Function,
    required:true
  }
})
</script>

<template>
  <nav>
    <ul>
      <li @click="SetDuration('Jour')">Jour</li>
      <li @click="SetDuration('Heure')">Heure</li>
    </ul>
  </nav>
</template>

<style scoped>
nav{
  display: flex;
  justify-content: center;
}
ul{
  display: flex;
  justify-content: center;
  gap: 15px;
  background-color: #f4f4f4;
  padding: 25px;
  border-radius: 10px;
}

li{
  list-style: none;
  background-color: #e2e2e2;
  padding: 15px;
  border-radius: 15px;

}
li:hover, li:active{
  background-color: #d3d3d3;
  cursor: pointer;
  color: white;
}
</style>