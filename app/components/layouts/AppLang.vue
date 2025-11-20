<template>
  <div class="flex items-center">
    <i class="pi pi-globe"></i>
    <Select v-model="selectedLanguage" :options="mockLanguages" optionLabel="code" optionValue="value" :pt:root="{
      class: `border-none! shadow-none!  items-center  bg-transparent! h-auto! w-auto! min-w-0! ${props.mobileToggle ? 'text-white' : ''}`
    }" :pt:label="{
      class: `px-1! bg-transparent! ${props.mobileToggle ? 'text-white!' : ''}`
    }" :pt:dropdown="{
      class: 'w-fit!'
    }" />
  </div>
</template>

<script setup lang="ts">

const props = defineProps<{
  mobileToggle?: boolean;
}>()



const mockLanguages = [
  { code: 'EN', value: 'en' },
  { code: 'ID', value: 'es' },
  { code: 'FR', value: 'fr' },
  { code: 'DE', value: 'de' },
];

const selectedLanguage = ref<string>(mockLanguages[0]!.value);

</script>

<style scoped>

</style>