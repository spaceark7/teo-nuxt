<template>
  <div class="hidden gap-1 md:gap-2 lg:gap-4 lg:flex xl:gap-6">
    <AppNavItem v-for="(item, index) in mockData" :key="index" :label="item.label" :icon="item.icon"
      :items="item?.items" @click="onItemClick" />
  </div>
  <div class="flex gap-1 md:gap-2 lg:gap-4 lg:hidden ">
    <AppNavItem v-for="(item, index) in mockData" :key="index" :label="item.label" :icon="item.icon"
      :items="item?.items" @click="onItemClick" />
  </div>
</template>

<script setup lang="ts">
import AppNavItem from './AppNavItem.vue';


const emits = defineEmits<{
  (e: 'itemClick', payload?: any): void;
}>()


const mockData = [
  {
    label: 'Home',
    icon: '',
  },
  {
    label: 'Resources',
    icon: '',
    items: [
      {
        label: 'Documentation',
        icon: 'pi pi-file',
      },
      {
        label: 'Tutorials',
        icon: 'pi pi-book',
      },
      {
        label: 'Community',
        icon: 'pi pi-users',
      },
    ],
  },
  {
    label: 'Ask the Expert',
    icon: '',
  }, {
    label: 'Eye test'
  },
  {
    label: 'Forum'
  }
]

const onItemClick = (items: any) => {
  console.log('Clicked item with subitems:', items);
  emits('itemClick', items);
}

</script>

<style scoped></style>