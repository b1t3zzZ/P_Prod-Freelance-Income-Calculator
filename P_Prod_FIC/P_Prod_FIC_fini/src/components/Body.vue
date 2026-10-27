<script setup>
/**
 * Body.vue — Главный компонент калькулятора доходов (Gross vs Net)
 * Разработан в точности по макету SwissFinance.
 * 
 * Логика понятная, простая и полностью прозрачная:
 * 1. Пользователь вводит данные о ставке, периоде, рабочих часах и налогах.
 * 2. При клике на "Calculate income" (или при вводе в поля) пересчитываются
 *    доходы за час, неделю, месяц и год как до вычетов (Gross), так и чистыми (Net).
 * 3. Строится наглядный график сравнения годового дохода.
 */

import { ref, computed, onMounted } from 'vue';

// Импорт иконок из assets
import clockIcon from '@/assets/clock.svg';
import weeklyIcon from '@/assets/calendar-trend.svg';
import monthlyIcon from '@/assets/calendar-days.svg';
import annualIcon from '@/assets/chart-bars.svg';

// ==========================================
// 1. ПОЛЯ ВВОДА (Данные формы)
// ==========================================
const amount = ref('30.00');          // Сумма дохода (по умолчанию 30.00)
const period = ref('hour');           // Период: 'hour' | 'day' | 'week' | 'month' | 'year'
const currency = ref('CHF');          // Валюта: 'CHF' | 'EUR' | 'USD' | 'GBP'
const hoursPerWeek = ref(40);         // Рабочих часов в неделю
const daysPerWeek = ref(5);           // Рабочих дней в неделю
const weeksPerYear = ref(52);         // Рабочих недель в году
const taxRate = ref(15);              // Налог и отчисления в % (по умолчанию 15%)

// ==========================================
// 2. РЕЗУЛЬТАТЫ РАСЧЕТОВ
// ==========================================
const results = ref({
  hourly: { gross: 0, net: 0 },
  weekly: { gross: 0, net: 0 },
  monthly: { gross: 0, net: 0 },
  annual: { gross: 0, net: 0 }
});

// ==========================================
// 3. ФОРМАТИРОВАНИЕ ЧИСЕЛ
// ==========================================
// Швейцарский стандарт: разделитель тысяч — апостроф (например: CHF 62'400.00)
function formatMoney(num, curr = currency.value) {
  const value = Number(num) || 0;
  const parts = value.toFixed(2).split('.');
  parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, "'");
  return `${curr} ${parts[0]}.${parts[1]}`;
}

// ==========================================
// 4. ГЛАВНАЯ ФУНКЦИЯ РАСЧЕТА
// ==========================================
function calculateIncome() {
  const inputSum = parseFloat(amount.value) || 0;
  const hWeek = Number(hoursPerWeek.value) || 40;
  const dWeek = Number(daysPerWeek.value) || 5;
  const wYear = Number(weeksPerYear.value) || 52;
  const taxPercent = Number(taxRate.value) || 0;

  // Шаг 1: Приводим любой введенный доход к базовому годовому доходу (Gross Annual)
  let grossAnnual = 0;

  if (period.value === 'hour') {
    // Ввели ставку в час -> умножаем на часы в неделю и недели в году
    grossAnnual = inputSum * hWeek * wYear;
  } else if (period.value === 'day') {
    // Ввели ставку в день -> умножаем на дни в неделю и недели в году
    grossAnnual = inputSum * dWeek * wYear;
  } else if (period.value === 'week') {
    // Ввели доход в неделю -> умножаем на недели в году
    grossAnnual = inputSum * wYear;
  } else if (period.value === 'month') {
    // Ввели доход в месяц -> умножаем на 12 месяцев
    grossAnnual = inputSum * 12;
  } else if (period.value === 'year') {
    // Уже введен годовой доход
    grossAnnual = inputSum;
  }

  // Шаг 2: Вычисляем Gross (до вычета налогов) для всех 4 периодов
  const grossHourly = grossAnnual / (hWeek * wYear);
  const grossWeekly = grossAnnual / wYear;
  const grossMonthly = grossAnnual / 12;

  // Шаг 3: Вычисляем чистый множитель (например, 15% налога -> чистыми 0.85)
  const netMultiplier = Math.max(0, 1 - (taxPercent / 100));

  // Шаг 4: Записываем результаты (Gross и Net)
  results.value = {
    hourly: {
      gross: grossHourly,
      net: grossHourly * netMultiplier
    },
    weekly: {
      gross: grossWeekly,
      net: grossWeekly * netMultiplier
    },
    monthly: {
      gross: grossMonthly,
      net: grossMonthly * netMultiplier
    },
    annual: {
      gross: grossAnnual,
      net: grossAnnual * netMultiplier
    }
  };
}

// Рассчитываем сразу при загрузке страницы, чтобы отображались данные как на макете
onMounted(() => {
  calculateIncome();
});

// ==========================================
// 5. ДАННЫЕ ДЛЯ ГРАФИКА (ШКАЛА И ПОЛОСЫ)
// ==========================================
// Вычисляем деления шкалы (например: 0, 10K, 20K, ..., 70K)
const chartData = computed(() => {
  const gross = results.value.annual.gross;
  const net = results.value.annual.net;

  // Максимум шкалы кратен 10'000 (например 70'000 при доходе 62'400)
  let maxTick = 70000;
  if (gross > 0) {
    const rawMax = gross * 1.12;
    const step = rawMax > 200000 ? 50000 : rawMax > 100000 ? 20000 : 10000;
    maxTick = Math.max(step * 4, Math.ceil(rawMax / step) * step);
  }

  // Генерируем 7 промежутков шкалы (0, 10K, 20K, 30K, 40K, 50K, 60K, 70K)
  const ticks = [];
  const tickCount = 7;
  const tickStep = maxTick / tickCount;

  for (let i = 0; i <= tickCount; i++) {
    const val = Math.round(i * tickStep);
    const label = val === 0 ? '0' : val >= 1000 ? `${Math.round(val / 1000)}K` : `${val}`;
    ticks.push({ label, value: val });
  }

  // Процент заполнения полос
  const grossPercent = maxTick > 0 ? Math.min(100, (gross / maxTick) * 100) : 0;
  const netPercent = maxTick > 0 ? Math.min(100, (net / maxTick) * 100) : 0;

  return {
    maxTick,
    ticks,
    grossPercent,
    netPercent
  };
});
</script>

<template>
  <main class="max-w-7xl mx-auto px-6 py-10">
    
    <!-- Заголовок страницы -->
    <div class="mb-8">
      <h1 class="text-3xl md:text-4xl font-extrabold text-slate-900 tracking-tight">
        Income Calculator
      </h1>
      <p class="text-slate-500 text-base md:text-lg mt-1 font-normal">
        Calculate your gross and net income
      </p>
    </div>

    <!-- Основная сетка: слева карточка формы, справа карточка результатов -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
      
      <!-- ============================================== -->
      <!-- ЛЕВАЯ КАРТОЧКА: ФОРМА ВВОДА ПАРАМЕТРОВ        -->
      <!-- ============================================== -->
      <section class="lg:col-span-5 bg-white border border-gray-200 rounded-2xl p-6 md:p-7 shadow-sm">
        <form @submit.prevent="calculateIncome" class="space-y-4.5">
          
          <!-- Поле 1: Income amount -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Income amount
            </label>
            <input
              v-model="amount"
              @input="calculateIncome"
              type="number"
              step="any"
              placeholder="0.00"
              class="w-full h-11 px-3.5 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition"
            />
          </div>

          <!-- Поле 2: Income period -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Income period
            </label>
            <div class="relative">
              <select
                v-model="period"
                @change="calculateIncome"
                class="w-full h-11 px-3.5 pr-10 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm appearance-none focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition cursor-pointer"
              >
                <option value="hour">Per hour</option>
                <option value="day">Per day</option>
                <option value="week">Per week</option>
                <option value="month">Per month</option>
                <option value="year">Per year</option>
              </select>
              <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-slate-400">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path d="M19 9l-7 7-7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </div>
            </div>
          </div>

          <!-- Поле 3: Currency -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Currency
            </label>
            <div class="relative">
              <select
                v-model="currency"
                @change="calculateIncome"
                class="w-full h-11 px-3.5 pr-10 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm appearance-none focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition cursor-pointer"
              >
                <option value="CHF">CHF</option>
                <option value="EUR">EUR</option>
                <option value="USD">USD</option>
                <option value="GBP">GBP</option>
              </select>
              <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-slate-400">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path d="M19 9l-7 7-7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </div>
            </div>
          </div>

          <!-- Поле 4: Hours per week -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Hours per week
            </label>
            <input
              v-model="hoursPerWeek"
              @input="calculateIncome"
              type="number"
              min="1"
              max="168"
              class="w-full h-11 px-3.5 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition"
            />
          </div>

          <!-- Поле 5: Days per week -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Days per week
            </label>
            <input
              v-model="daysPerWeek"
              @input="calculateIncome"
              type="number"
              min="1"
              max="7"
              class="w-full h-11 px-3.5 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition"
            />
          </div>

          <!-- Поле 6: Weeks per year -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Weeks per year
            </label>
            <input
              v-model="weeksPerYear"
              @input="calculateIncome"
              type="number"
              min="1"
              max="52"
              class="w-full h-11 px-3.5 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition"
            />
          </div>

          <!-- Поле 7: Tax and deductions -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1.5">
              Tax and deductions
            </label>
            <div class="relative">
              <select
                v-model="taxRate"
                @change="calculateIncome"
                class="w-full h-11 px-3.5 pr-10 bg-white border border-gray-300 rounded-lg text-slate-900 text-sm appearance-none focus:outline-none focus:ring-2 focus:ring-red-500/20 focus:border-red-600 transition cursor-pointer"
              >
                <option :value="0">0%</option>
                <option :value="5">5%</option>
                <option :value="10">10%</option>
                <option :value="15">15%</option>
                <option :value="20">20%</option>
                <option :value="25">25%</option>
                <option :value="30">30%</option>
                <option :value="35">35%</option>
                <option :value="40">40%</option>
              </select>
              <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-slate-400">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path d="M19 9l-7 7-7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </div>
            </div>
          </div>

          <!-- Кнопка: Calculate income -->
          <div class="pt-2">
            <button
              type="submit"
              class="w-full h-12 bg-[#dc2626] hover:bg-[#b91c1c] active:bg-[#991b1b] text-white font-semibold rounded-xl text-base shadow-sm transition-all flex items-center justify-center cursor-pointer"
            >
              Calculate income
            </button>
          </div>

        </form>
      </section>

      <!-- ============================================== -->
      <!-- ПРАВАЯ КАРТОЧКА: РЕЗУЛЬТАТЫ И ГРАФИК           -->
      <!-- ============================================== -->
      <section class="lg:col-span-7 bg-white border border-gray-200 rounded-2xl p-6 md:p-8 shadow-sm">
        
        <!-- Заголовок карточки результатов -->
        <h2 class="text-xl font-bold text-slate-900 mb-5">
          Your income
        </h2>

        <!-- Список строк: Hourly, Weekly, Monthly, Annual -->
        <div class="divide-y divide-gray-100">
          
          <!-- 1. Hourly -->
          <div class="py-4.5 flex items-center justify-between">
            <div class="flex items-center gap-4 w-36 sm:w-44 shrink-0">
              <img :src="clockIcon" alt="Hourly" class="w-10 h-10 shrink-0" />
              <span class="text-base font-semibold text-slate-800">Hourly</span>
            </div>
            <div class="text-right flex-1 pr-4 sm:pr-8">
              <span class="text-base sm:text-lg font-bold text-slate-900">
                {{ formatMoney(results.hourly.gross) }}
              </span>
            </div>
            <div class="text-right w-36 sm:w-44 shrink-0">
              <span class="text-base sm:text-lg font-semibold text-[#16a34a]">
                {{ formatMoney(results.hourly.net) }} <span class="text-sm font-normal text-[#16a34a]/90">net</span>
              </span>
            </div>
          </div>

          <!-- 2. Weekly -->
          <div class="py-4.5 flex items-center justify-between">
            <div class="flex items-center gap-4 w-36 sm:w-44 shrink-0">
              <img :src="weeklyIcon" alt="Weekly" class="w-10 h-10 shrink-0" />
              <span class="text-base font-semibold text-slate-800">Weekly</span>
            </div>
            <div class="text-right flex-1 pr-4 sm:pr-8">
              <span class="text-base sm:text-lg font-bold text-slate-900">
                {{ formatMoney(results.weekly.gross) }}
              </span>
            </div>
            <div class="text-right w-36 sm:w-44 shrink-0">
              <span class="text-base sm:text-lg font-semibold text-[#16a34a]">
                {{ formatMoney(results.weekly.net) }} <span class="text-sm font-normal text-[#16a34a]/90">net</span>
              </span>
            </div>
          </div>

          <!-- 3. Monthly -->
          <div class="py-4.5 flex items-center justify-between">
            <div class="flex items-center gap-4 w-36 sm:w-44 shrink-0">
              <img :src="monthlyIcon" alt="Monthly" class="w-10 h-10 shrink-0" />
              <span class="text-base font-semibold text-slate-800">Monthly</span>
            </div>
            <div class="text-right flex-1 pr-4 sm:pr-8">
              <span class="text-base sm:text-lg font-bold text-slate-900">
                {{ formatMoney(results.monthly.gross) }}
              </span>
            </div>
            <div class="text-right w-36 sm:w-44 shrink-0">
              <span class="text-base sm:text-lg font-semibold text-[#16a34a]">
                {{ formatMoney(results.monthly.net) }} <span class="text-sm font-normal text-[#16a34a]/90">net</span>
              </span>
            </div>
          </div>

          <!-- 4. Annual -->
          <div class="py-4.5 flex items-center justify-between">
            <div class="flex items-center gap-4 w-36 sm:w-44 shrink-0">
              <img :src="annualIcon" alt="Annual" class="w-10 h-10 shrink-0" />
              <span class="text-base font-semibold text-slate-800">Annual</span>
            </div>
            <div class="text-right flex-1 pr-4 sm:pr-8">
              <span class="text-base sm:text-lg font-bold text-slate-900">
                {{ formatMoney(results.annual.gross) }}
              </span>
            </div>
            <div class="text-right w-36 sm:w-44 shrink-0">
              <span class="text-base sm:text-lg font-semibold text-[#16a34a]">
                {{ formatMoney(results.annual.net) }} <span class="text-sm font-normal text-[#16a34a]/90">net</span>
              </span>
            </div>
          </div>

        </div>

        <!-- ============================================== -->
        <!-- ГРАФИК: Gross vs net income                    -->
        <!-- ============================================== -->
        <div class="mt-8 pt-7 border-t border-gray-200">
          
          <h3 class="text-lg font-bold text-slate-900 mb-6">
            Gross vs net income
          </h3>

          <div class="space-y-4">
            
            <!-- Полоса 1: Gross income (Темно-синяя) -->
            <div class="flex items-center gap-3">
              <span class="w-24 sm:w-28 text-xs sm:text-sm font-medium text-slate-600 shrink-0">
                Gross income
              </span>
              <div class="flex-1 h-5.5 bg-slate-100 rounded-sm overflow-hidden">
                <div
                  class="h-full bg-[#0f172a] rounded-sm transition-all duration-300"
                  :style="{ width: `${chartData.grossPercent}%` }"
                ></div>
              </div>
              <span class="w-28 sm:w-32 text-xs sm:text-sm font-semibold text-slate-900 shrink-0 text-right">
                {{ formatMoney(results.annual.gross) }}
              </span>
            </div>

            <!-- Полоса 2: Net income (Зеленая) -->
            <div class="flex items-center gap-3">
              <span class="w-24 sm:w-28 text-xs sm:text-sm font-medium text-slate-600 shrink-0">
                Net income
              </span>
              <div class="flex-1 h-5.5 bg-slate-100 rounded-sm overflow-hidden">
                <div
                  class="h-full bg-[#16a34a] rounded-sm transition-all duration-300"
                  :style="{ width: `${chartData.netPercent}%` }"
                ></div>
              </div>
              <span class="w-28 sm:w-32 text-xs sm:text-sm font-semibold text-slate-900 shrink-0 text-right">
                {{ formatMoney(results.annual.net) }}
              </span>
            </div>

            <!-- Шкала (находится строго под полосой графика) -->
            <div class="flex items-center gap-3 pt-1">
              <div class="w-24 sm:w-28 shrink-0"></div> <!-- Отступ под левый лейбл -->
              <div class="flex-1">
                <!-- Засечки и метки -->
                <div class="w-full border-t border-gray-300 pt-1.5 flex justify-between">
                  <span
                    v-for="tick in chartData.ticks"
                    :key="tick.label"
                    class="text-[11px] text-slate-400 font-medium select-none"
                  >
                    {{ tick.label }}
                  </span>
                </div>
                <!-- Подпись шкалы по центру -->
                <div class="text-center mt-1.5">
                  <span class="text-xs text-slate-500 font-medium">
                    {{ currency }} per year
                  </span>
                </div>
              </div>
              <div class="w-28 sm:w-32 shrink-0"></div> <!-- Отступ под правую сумму -->
            </div>

          </div>

          <!-- Легенда графика в самом низу -->
          <div class="mt-6 flex items-center justify-center gap-8 text-xs sm:text-sm font-medium text-slate-700">
            <div class="flex items-center gap-2">
              <span class="w-3.5 h-3.5 bg-[#0f172a] rounded-sm inline-block"></span>
              <span>Gross income</span>
            </div>
            <div class="flex items-center gap-2">
              <span class="w-3.5 h-3.5 bg-[#16a34a] rounded-sm inline-block"></span>
              <span>Net income</span>
            </div>
          </div>

        </div>

      </section>

    </div>

  </main>
</template>

<style scoped>
.space-y-4\.5 > :not([hidden]) ~ :not([hidden]) {
  margin-top: 1.125rem;
}
.py-4\.5 {
  padding-top: 1.125rem;
  padding-bottom: 1.125rem;
}
.h-5\.5 {
  height: 1.375rem;
}
</style>
