<template>
  <div>

    <i class="pi pi-globe"></i>
    <Select
    v-model="selectedLanguage"
    :options="mockLanguages"
    optionLabel="code"
    optionValue="value"
    :pt:root="{
      class: 'border-none!'
    }"
    />
  </div>
</template>

<script setup lang="ts">



const mockLanguages = [
  { code: 'En', value: 'en' },
  { code: 'Es', value: 'es' },
  { code: 'Fr', value: 'fr' },
  { code: 'De', value: 'de' },
];

const selectedLanguage = ref<string>(mockLanguages[0]!.value);

</script>

<style scoped>

</style>