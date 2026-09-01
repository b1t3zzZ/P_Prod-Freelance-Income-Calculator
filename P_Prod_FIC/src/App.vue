<script setup>
import { ref } from 'vue'
import IncomeForm from "@/components/IncomeForm.vue";
import DurationWork from "@/components/DurationWork.vue";
import TaxPercent from "@/components/TaxPercent.vue";

let choiseIncome = ref(0);
let choiseDuration = ref(0);
let choiseTax = ref(0);
let choiseCommission = ref(0);

function SetForm(val) {
  choiseIncome.value = val;
}
function SetDuration(val) {
  choiseDuration.value = val;
}
function SetTax(val) {
  choiseTax.value = val;
}
function SetCommission(val) {
  choiseCommission.value = val;
}

</script>
<template>

  <h1>Calculator of income</h1>

  <IncomeForm :SetForm="SetForm"></IncomeForm>
  <DurationWork :SetDuration="SetDuration"></DurationWork>
  <TaxPercent :SetTax="SetTax"></TaxPercent>
  <TaxPercent :SetTax="SetCommission"></TaxPercent>



</template>
<style scoped>
h1 {
  text-align: center;
}
</style>
