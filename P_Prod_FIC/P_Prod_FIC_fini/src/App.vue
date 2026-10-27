<script setup>
/**
 * App.vue — Корневой компонент приложения
 * Объединяет верхнее меню (Header) и основной экран калькулятора (Body).
 */
import Header from "@/components/Header.vue";
import Body from "@/components/Body.vue";
</script>

<template>
  <div class="min-h-screen bg-[#f8fafc] text-slate-800 font-sans antialiased">
    <Header />
    <Body />
  </div>
</template>

<style scoped>
</style>
