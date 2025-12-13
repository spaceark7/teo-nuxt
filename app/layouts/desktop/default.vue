
<template>
  <section class="min-h-screen flex flex-col">
    <AppHeader />
    <main class="flex-1">
      <slot />
    </main>
    <!-- <AppFooter /> -->
  </section>
</template>
<script setup lang="ts">
import AppHeader from '~/components/layouts/AppHeader.vue';

</script>


