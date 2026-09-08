<script setup>

const props = defineProps({
  SetForm: {
    type: Function,
    required: true
  }
})

</script>

<template>
  <nav>
    <ul>
      <li @click="SetForm('Total')">Prix Total</li>
      <li @click="SetForm('CHFheure')">CHF par heure</li>
    </ul>
  </nav>
</template>

<style scoped>

ul{
  display:flex;
  justify-content:space-around;
  gap:20px;
  list-style:none;
  margin:0;
  padding:0;
}
li{
  cursor: pointer;
  font-size:16px;
  margin:10px 0;
  padding:10px 20px;
  border:1px solid black;
  border-radius:15px;
}
li:hover{
  color:red;
  border:1px solid red;
  box-shadow: 0px 0px 6px red;
}

</style>