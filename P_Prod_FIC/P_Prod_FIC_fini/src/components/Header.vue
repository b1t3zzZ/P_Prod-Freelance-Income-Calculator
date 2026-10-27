<script setup>
// Шапка сайта SwissFinance с навигацией и переключателем языка
import { ref } from 'vue';

// Список ссылок в меню навигации
const navLinks = [
  { name: 'Dashboard', active: false },
  { name: 'Calculator', active: true }, // Текущая активная страница
  { name: 'Planning', active: false },
  { name: 'Resources', active: false },
  { name: 'About', active: false }
];

// Выбранный язык
const selectedLanguage = ref('English');
</script>

<template>
  <header class="bg-white border-b border-gray-200">
    <div class="max-w-7xl mx-auto px-6 h-18 flex items-center justify-between">
      
      <!-- Левая часть: Логотип SwissFinance -->
      <div class="flex items-center gap-12">
        <a href="#" class="flex items-center gap-2.5 text-slate-900 font-bold text-xl tracking-tight select-none">
          <!-- Швейцарский крест -->
          <span class="w-8 h-8 rounded-md bg-[#dc2626] flex items-center justify-center text-white shrink-0 shadow-sm">
            <svg class="w-4 h-4 fill-current" viewBox="0 0 24 24">
              <path d="M10 4h4v6h6v4h-6v6h-4v-6H4v-4h6V4z" />
            </svg>
          </span>
          <span>SwissFinance</span>
        </a>

        <!-- Меню навигации по центру -->
        <nav class="hidden md:flex items-center gap-8 h-18">
          <a
            v-for="link in navLinks"
            :key="link.name"
            href="#"
            class="relative py-6 text-sm font-medium transition-colors"
            :class="link.active ? 'text-slate-900 font-semibold' : 'text-slate-600 hover:text-slate-900'"
          >
            {{ link.name }}
            <!-- Красная черта под активным пунктом (Calculator) как на макете -->
            <span
              v-if="link.active"
              class="absolute bottom-0 left-0 right-0 h-0.75 bg-[#dc2626] rounded-t-full"
            ></span>
          </a>
        </nav>
      </div>

      <!-- Правая часть: Переключатель языка с иконкой глобуса -->
      <div class="flex items-center gap-2 text-slate-700 text-sm font-medium cursor-pointer hover:text-slate-900">
        <!-- Иконка глобуса -->
        <svg class="w-4.5 h-4.5 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <circle cx="12" cy="12" r="10" stroke-width="1.8" />
          <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z" stroke-width="1.8" />
        </svg>
        <span>{{ selectedLanguage }}</span>
        <!-- Стрелочка вниз -->
        <svg class="w-3.5 h-3.5 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path d="M19 9l-7 7-7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </div>

    </div>
  </header>
</template>

<style scoped>
.h-18 {
  height: 4.5rem;
}
.h-0\.75 {
  height: 3px;
}
</style>
