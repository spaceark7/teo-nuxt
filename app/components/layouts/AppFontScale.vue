<template>
  <button class="teo-btn-light" type="button" @click="toggle($event)">
    <img :src="FontScale" alt="Font Scale" class="h-3" />
  </button>
  <Popover ref="op">
    <div class="flex flex-col items-center gap-4 w-5">
      <Slider v-model="scale" :min="12" orientation="vertical" :max="18" @change="onScaleChange" />
      <span>{{ scale }}</span>
    </div>
  </Popover>
</template>

<script setup lang="ts">
import FontScale from '~/assets/images/font-scale.png'

const op = ref();
const scale = ref<number>(14);


const toggle = (event: Event) => {
  op.value?.toggle(event);
}

const onScaleChange = (value: number | number[]) => {
  document.documentElement.style.fontSize = `${value}px`;
}
</script>

<style scoped>

</style>