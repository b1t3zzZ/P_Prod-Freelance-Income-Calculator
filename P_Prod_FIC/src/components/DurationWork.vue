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

</style>