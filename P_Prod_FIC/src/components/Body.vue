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
    if (choiseIncome.value === 'CHFheure') {
        grossIncome.value = choiseIncome.value * choiseDuration.value;
    }
    else {
        grossIncome.value = choiseIncome.value;
    }

    if (SetTax && SetCommission) {
        sommeReduction.value = choiseTax.value + choiseCommission.value;
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
                <form>
                    <div>
                        <h3>Choise your Income Type</h3>
                        <IncomeForm :SetForm="SetForm"></IncomeForm>
                    </div>
                    <div>
                        <p v-if="choiseIncome === 'Total'">Income amount</p>
                        <input v-if="choiseIncome === 'Total'" placeholder="Entree la somme du revenue" type="number">

                        <h3 v-if="choiseIncome === 'CHFheure'">Choise Period of Payement </h3>
                        <DurationWork v-if="choiseIncome === 'CHFheure'" :SetDuration="SetDuration"></DurationWork>
                    </div>

                    <div>
                        <p v-if="choiseDuration === 'Heure' && choiseIncome === 'CHFheure'"> Income per Hour</p>
                        <input v-if="choiseDuration === 'Heure' && choiseIncome === 'CHFheure'"
                            placeholder="Ecrivez la somme par Heure" type="number">

                        <p v-if="choiseDuration === 'Jour' && choiseIncome === 'CHFheure'">Income per Day</p>
                        <input v-if="choiseDuration === 'Jour' && choiseIncome === 'CHFheure'"
                            placeholder="Ecrivez la somme par Jour" type="number">
                    </div>

                    <div>
                        <p>Choise your Tax Rate</p>
                        <TaxPercent :SetTax="SetTax"></TaxPercent>
                    </div>

                    <div>
                        <p>Choise your Commision Rate</p>
                        <TaxPercent :SetTax="SetCommission"></TaxPercent>
                    </div>

                    <button @click="CalculerIncome" type="button">Calculer</button>
                </form>
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
    gap: 20px;
}

section {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 15px;

}

fieldset {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    flex-direction: column;
    background-color: white;
    border-radius: 15px;

}
form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width:500px;
}

form>div {
    display: flex;
    flex-direction: column;
    gap: 5px;
}
input {
    padding: 5px;
    border-radius: 5px;
    border: 2px solid rgb(168, 168, 168);
    width: 97%;
}
h3 {
    margin: 0;
    padding: 0;
    text-align: center;
}

p {
    padding: 0;
    margin: 0;
}
button{
    display: flex;
    justify-content: center;
    padding: 15px;
    margin-top: 15px;
    background-color: red;
    border: none;
    border-radius: 5px;
    color: white;
    font-size: 16px;
    width: 100%;
    transition: transform 0.1s ease;
}
button:hover{
    background-color: rgb(236, 0, 0);
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.35);
}
button:active{
    transform: scale(0.97);
}
</style>
