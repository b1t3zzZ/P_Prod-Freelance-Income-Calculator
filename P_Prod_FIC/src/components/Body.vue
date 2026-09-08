<script setup>
import { ref } from 'vue'
import IncomeForm from "@/components/IncomeForm.vue";
import DurationWork from "@/components/DurationWork.vue";
import TaxPercent from "@/components/TaxPercent.vue";

let choiseIncome = ref(0);
let choiseDuration = ref(0);
let choiseTax = ref(0);
let choiseCommission = ref(0);
let grossIncome = ref(0);
let sommeReduction = ref(0);

function CalculerIncome() {
    if (SetForm === 'CHFheure') {
        grossIncome = choiseIncome * choiseDuration;
    }
    else {
        grossIncome = choiseIncome;
    }

    if (SetTax && SetCommission) {
        sommeReduction = choiseTax + choiseCommission;
    }


}

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

    <main>

        <div>
            <h1>Income Calculator</h1>
            <h6>Calculate your gross and net income</h6>
        </div>

        <section>
            <fieldset>
                <IncomeForm :SetForm="SetForm"></IncomeForm>
                <input v-if="choiseIncome === 'Total'" placeholder="Entree la somme du revenue" type="number">
                <DurationWork v-if="choiseIncome === 'CHFheure'" :SetDuration="SetDuration"></DurationWork>
                <input v-if="choiseDuration === 'Heure' && choiseIncome === 'CHFheure'"
                    placeholder="Ecrivez la somme par heure" type="number">
                <input v-if="choiseDuration === 'Jour' && choiseIncome === 'CHFheure'"
                    placeholder="Ecrivez la somme par heure" type="number">
                <TaxPercent :SetTax="SetTax"></TaxPercent>
                <TaxPercent :SetTax="SetCommission"></TaxPercent>
                <button @click="CalculerIncome">Calculer</button>
            </fieldset>


            <fieldset>

            </fieldset>
        </section>



    </main>

</template>
<style scoped>
template {
    padding: 0;
    margin: 0;
    width: 100%;
}

main {
    display: flex;
    flex-direction: column;
    margin: 35px;
}
section{
    display:grid;
    grid-template-columns: 1fr 2fr;

}
fieldset{
    display:flex;
    flex-direction:column;
    background-color: white;
}

</style>
