<template>
  <div class="relative w-full">
    <div class="relative z-20 bg-white">
      <!-- Logo and Nav -->
      <div class="h-[80px] lg:px-8 xl:px-0 xl:max-w-9/12 mx-auto flex items-center justify-between">
        <img :src="Logo" alt="Teo Logo" class="h-9" />
        <AppNav @item-click="handleOverlay" />
        <div class="flex items-center gap-8">
          <div class="flex gap-2 items-center">
            <AppLang />
            <AppFontScale />
            <div>
              <button>
                <i class="pi pi-search"></i>
              </button>
            </div>
          </div>
          <button class="teo-btn-dark teo-btn-size-lg t-body-medium xl:t-body-large">
            Book an Appointment
          </button>
        </div>
      </div>
      <div class="bg-blue-teo-500 p-4 text-white">Change Condition</div>
    </div>
    <div class="absolute transition-all duration-500 left-0 w-full z-10 bg-white shadow-lg p-8"
      :class="showOverlay ? 'top-full' : 'top-0 -translate-y-full'">
      Overlay for Resouces
    </div>
  </div>

</template>

<script setup lang="ts">
import Logo from '~/assets/images/Logo.png';
import AppFontScale from './AppFontScale.vue';
import AppLang from './AppLang.vue';
import AppNav from './AppNav.vue';



const showOverlay = ref<boolean>(false);

const handleOverlay = (items:any) => {
  console.log('Overlay items:', items);
  showOverlay.value = !showOverlay.value;
}

</script>

<style scoped>

</style>