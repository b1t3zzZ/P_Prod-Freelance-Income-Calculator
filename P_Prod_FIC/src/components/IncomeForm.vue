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

</style>