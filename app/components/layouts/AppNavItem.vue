<template>
  <div v-if="props.isMobile">
    <Panel v-if="props.items" :header="props.label" @toggle="toggleActive" :collapsed="true" :pt="{
      root: {
        class: 't-body-small font-semibold! px-3! py-4! bg-transparent! text-white! border-t-0! border-x-0 border-l-0! border-r-0! border-b! rounded-none py-5 border-dashed! border-blue-teo-400! w-full text-start'
      },
      header: {
        class: 't-body-small font-semibold! px-0! py-0!   bg-transparent! text-white! border-none! '
      },
      title: {
        class: 't-body-small font-semibold!! px-0! py-0!  bg-transparent! text-white! border-none!'
      },
      content: {
        class: 'bg-transparent!'
      },
      pcToggleButton: {
        root: {
          class: 'hover:bg-transparent!'
        }
      }
    }" toggleable>
      <template #toggleicon>
        <i :class="[
          'pi pi-chevron-up ml-1 text-white',
          isActive ? 'transition-transform -rotate-180 duration-300' : 'transition-transform rotate-0 duration-300'
        ]"></i>
      </template>
      <div class="max-h-56 overflow-y-auto ">

        <div v-for="(item, index) in props.items" :key="index" class="py-2 t-body-large! font-regular!">{{ item.label }}
        </div>
      </div>
    </Panel>
    <button v-else
      class="teo-btn-ghost t-body-small font-semibold! text-white  border-b rounded-none py-5 border-dashed border-blue-teo-400 w-full text-start t-body-small font-medium">
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
  </div>
  <button v-else class="teo-btn-link teo-btn-size-sm t-body-small xl:t-body-medium" @click="onClick">
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
  isMobile?: boolean;
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

const toggleActive = () => {
  isActive.value = !isActive.value;
}

</script>

<style scoped>

</style>