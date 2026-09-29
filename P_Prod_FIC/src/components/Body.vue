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
let pureSome = ref(0);
let periode = ref(0);
let somme = ref(0);
let sommeTotal = ref(0)

function CalculerIncome() {
    if (choiseIncome.value === 'CHFheure' && (SetTax && SetCommission)) {

        grossIncome.value = somme.value * periode.value;
        sommeReduction.value = choiseTax.value + choiseCommission.value;
        pureSome.value = grossIncome.value * (1 - (sommeReduction.value / 100));

    }

    if (choiseIncome.value === 'Total' && (SetTax && SetCommission)) {
        grossIncome.value = sommeTotal.value;
        sommeReduction.value = choiseTax.value + choiseCommission.value;
        pureSome.value = grossIncome.value * (1 - (sommeReduction.value / 100));
    }

    return pureSome.value;
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
            <fieldset class="calculator-card firstField">
                <form>
                    <div class="family-block">
                        <div class="family-header">
                            <span class="step-badge">1</span>
                            <h3>Choose your Income Type</h3>
                        </div>

                        <div class="selector-box">
                            <IncomeForm :SetForm="SetForm"></IncomeForm>
                        </div>

                        <div v-if="choiseIncome === 'Total' || choiseIncome === 'CHFheure'" class="sub-family">

                            <!--                                                          Целый проэкт                                                                                               -->

                            <div v-if="choiseIncome === 'Total'" class="field-item">
                                <p class="field-label">Income amount</p>
                                <div class="input-wrapper">
                                    <input v-model="sommeTotal" placeholder="Entrez la somme du revenu" type="number">
                                    <span class="unit-tag">CHF</span>
                                </div>
                            </div>

                            <!--                                                          Дни/Часы                                                                                               -->

                            <div v-if="choiseIncome === 'CHFheure'" class="nested-flow">
                                <div class="field-item">
                                    <p class="field-label center-label">Choose Period of Payment</p>
                                    <div class="selector-box">
                                        <DurationWork :SetDuration="SetDuration"></DurationWork>
                                    </div>
                                </div>

                                <!--                                                          Для часов                                                                                               -->

                                <div v-if="choiseDuration === 'Heure'" class="field-item amount-appear">
                                    <p class="field-label">Income per Hour</p>
                                    <div class="input-wrapper">
                                        <input v-model="somme" placeholder="Ecrivez la somme par Heure" type="number">
                                        <span class="unit-tag">CHF</span>
                                    </div>
                                    <div class="input-wrapper">
                                        <input v-model="periode" placeholder="Indiquez le nombre d'heures"
                                            type="number">
                                        <span class="unit-tag">H</span>
                                    </div>
                                </div>

                                <!--                                                          Для дней                                                                                               -->

                                <div v-if="choiseDuration === 'Jour'" class="field-item amount-appear">
                                    <p class="field-label">Income per Day</p>
                                    <div class="input-wrapper">
                                        <input v-model="somme" placeholder="Ecrivez la somme par Jour" type="number">
                                        <span class="unit-tag">CHF</span>
                                    </div>
                                    <div class="input-wrapper">
                                        <input v-model="periode" placeholder="Indiquez le nombre de jours"
                                            type="number">
                                        <span class="unit-tag">J</span>
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>

                    <div class="family-block">
                        <div class="family-header">
                            <span class="step-badge">2</span>
                            <h3>Taxes & Commissions</h3>
                        </div>

                        <!--                                                          Для Такс                                                                                               -->

                        <div class="rates-row">
                            <div class="rate-item">
                                <p class="field-label">Tax Rate</p>
                                <div class="rate-control">
                                    <TaxPercent :SetTax="SetTax"></TaxPercent>
                                    <span class="percent-badge">%</span>
                                </div>
                            </div>

                            <!--                                                          Для коммисии                                                                                               -->

                            <div class="rate-item">
                                <p class="field-label">Commission Rate</p>
                                <div class="rate-control">
                                    <TaxPercent :SetTax="SetCommission"></TaxPercent>
                                    <span class="percent-badge">%</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <button @click="CalculerIncome" type="button">Calculer</button>
                </form>
            </fieldset>

            <fieldset class="secondField">
                <div>
                    <h2>Your income</h2>
                </div>
                <div class="table-de-valeurs">
                    <div class="row-de-valeur">
                        <img src="../assets/clock.svg" alt="Hourly">
                        <p class="ml-8">Hourly</p>
                        <span>CHF</span>

                    </div>
                    <div class="row-de-valeur">
                        <img src="../assets/calendar-trend.svg" alt="Weekly">
                        <p class="ml-8">Weekly</p>
                        <span>CHF</span>
                    </div>
                    <div class="row-de-valeur">
                        <img src="../assets/calendar-days.svg" alt="Monthly">
                        <p class="ml-8">Monthly</p>
                        <span>CHF</span>
                    </div>
                    <div class="row-de-valeur">
                        <img src="../assets/chart-bars.svg" alt="Annual">
                        <p class="ml-8 ">Annual</p>
                        <span>CHF</span>
                    </div>
                </div>

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
    background-color: white;
    border-radius: 18px;
    border: 1px solid #eaeaea;
    box-shadow: 0 8px 28px rgba(0, 0, 0, 0.05);
    padding: 26px 24px;
}

.firstField {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column
}

.secondField {
    display: flex;
    flex-direction: column;
}

form {
    display: flex;
    flex-direction: column;
    gap: 18px;
    width: 100%;
    max-width: 460px;
}

/* Контейнер для одной "семьи" элементов */
.family-block {
    display: flex;
    flex-direction: column;
    gap: 14px;
    padding: 18px;
    background-color: #fafafa;
    border: 1px solid #eeeeee;
    border-radius: 14px;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.family-block:hover {
    border-color: #e0e0e0;
}

/* Шапка группы с номерным бейджем */
.family-header {
    display: flex;
    align-items: center;
    gap: 10px;
    padding-bottom: 10px;
    border-bottom: 1px solid #ececec;
}

.step-badge {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 24px;
    height: 24px;
    border-radius: 7px;
    background-color: rgba(255, 0, 0, 0.1);
    color: red;
    font-size: 13px;
    font-weight: 700;
    flex-shrink: 0;
}

h3 {
    margin: 0;
    padding: 0;
    font-size: 15px;
    font-weight: 600;
    color: #1f2937;
}

/* Выравнивание кнопок выбора внутри семьи по одной сетке */
.selector-box :deep(ul) {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.selector-box :deep(li) {
    margin: 0;
    text-align: center;
    background-color: white;
    border-radius: 10px;
    font-weight: 500;
    transition: all 0.2s ease;
}

/* Вложенная подгруппа, визуально связанная с родителем красной линией слева */
.sub-family {
    display: flex;
    flex-direction: column;
    gap: 14px;
    padding: 14px 14px 14px 16px;
    background-color: white;
    border-radius: 10px;
    border: 1px solid #eeeeee;
    border-left: 3px solid red;
}

.nested-flow {
    display: flex;
    flex-direction: column;
    gap: 14px;
}

.field-item {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.field-item.amount-appear {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px 12px;
    padding-top: 12px;
    border-top: 1px dashed #e5e7eb;
}

.amount-appear .field-label {
    grid-column: 1 / -1;
}

.field-label {
    padding: 0;
    margin: 0;
    color: #4b5563;
    font-size: 13px;
    font-weight: 600;
}

.center-label {
    text-align: center;
    margin-bottom: 2px;
}

/* Поле ввода с индикатором валюты внутри */
.input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
    width: 100%;
}

.input-wrapper input {
    padding: 10px 48px 10px 12px;
    border-radius: 10px;
    border: 1.5px solid #d1d5db;
    font-size: 13.5px;
    width: 100%;
    box-sizing: border-box;
    background-color: #ffffff;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.input-wrapper input:hover {
    border-color: #9ca3af;
}

.input-wrapper input:focus {
    outline: none;
    border-color: red;
    box-shadow: 0 0 0 3px rgba(255, 0, 0, 0.12);
}

.unit-tag {
    position: absolute;
    right: 8px;
    padding: 3px 7px;
    background-color: rgba(255, 0, 0, 0.08);
    color: red;
    font-size: 11px;
    font-weight: 700;
    border-radius: 6px;
    pointer-events: none;
}

/* Сетка для семьи налогов и комиссий */
.rates-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.rate-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    padding: 14px 12px;
    background-color: white;
    border: 1px solid #eaeaea;
    border-radius: 12px;
    text-align: center;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.rate-item:hover {
    border-color: rgba(255, 0, 0, 0.35);
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.04);
}

.rate-control {
    display: flex;
    align-items: center;
    gap: 6px;
}

.rate-control :deep(input) {
    padding: 8px 10px;
    border-radius: 8px;
    border: 1.5px solid #d1d5db;
    font-size: 15px;
    font-weight: 600;
    text-align: center;
    width: 64px;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.rate-control :deep(input:focus) {
    outline: none;
    border-color: red;
    box-shadow: 0 0 0 3px rgba(255, 0, 0, 0.12);
}

.percent-badge {
    font-size: 14px;
    font-weight: 700;
    color: red;
}

/* Кнопка действия */
button {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 14px;
    margin-top: 4px;
    background-color: red;
    border: none;
    border-radius: 12px;
    color: white;
    font-size: 16px;
    font-weight: 600;
    letter-spacing: 0.5px;
    cursor: pointer;
    width: 100%;
    box-shadow: 0 4px 12px rgba(255, 0, 0, 0.25);
    transition: all 0.15s ease;
}

button:hover {
    background-color: rgb(230, 0, 0);
    box-shadow: 0 6px 18px rgba(255, 0, 0, 0.35);
    transform: translateY(-1px);
}

button:active {
    transform: scale(0.98) translateY(1px);
    box-shadow: 0 2px 6px rgba(255, 0, 0, 0.2);
}

.table-de-valeurs {
    display: flex;
    flex-direction: column;
}

.row-de-valeur {
    display: flex;
}
</style>
