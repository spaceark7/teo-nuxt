<template>
  <button class="teo-btn-link teo-btn-size-sm t-body-small lg:t-body-medium" @click="onClick">
    <template v-if="props.items">
      <span class="flex items-center" :class="isActive ? 'text-cyan-teo-500' : ''">
        <span>{{ props.label }}</span>
        <i :class="[
          'pi pi-chevron-down ml-1',
          isActive ? 'transition-transform -rotate-180 duration-300' : 'transition-transform rotate-0 duration-300'
        ]"></i>
      </span>

    </template>
    <template v-else>
      {{ props.label }}
    </template>
  </button>

</template>

<script setup lang="ts">

const props = defineProps<{
  label: string;
  icons?: string;
  route?: string;
  items?: Array<any>;
}>()

const emits = defineEmits<{
  (e: 'click', payload?: any): void;
}>()

//#region State
const isActive = ref<boolean>(false);
//#endregion

//#region Methods
const onClick = () => {
  if (props.route) {
    // Navigate to route

  } else if (props.items) {
    isActive.value = !isActive.value;
    emits('click', props.items);
  }

}

</script>

<style scoped>

</style>