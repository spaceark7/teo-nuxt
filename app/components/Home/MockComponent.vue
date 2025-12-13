<template>
  <div class="section-content">
    <div class="section-number text-blue-teo-500">{{ String(index + 1).padStart(2, '0') }}</div>
    <div class="section-title text-blue-teo-500 text-4xl">{{ data.heading }}</div>
    <div class="section-subtitle text-blue-teo-500">{{ data.description }}</div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  index: number,
  data: {
    heading: string,
    description: string
  }
}>()
</script>

<style scoped>
    
</style>