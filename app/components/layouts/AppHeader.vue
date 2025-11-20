<template>
  <div class="relative w-full">
    <div class="relative z-20 transition-all duration-500 "
      :class="isMobileNavVisible ? 'bg-blue-teo-600 text-white' : 'bg-white'">
      <!-- Logo and Nav -->
      <div
        class="h-[56px] lg:h-[80px] px-7 md:px-9 xl:px-16  2xl:max-w-10/12 mx-auto flex items-center justify-between">
        <img :src="isMobileNavVisible ? LogoWhite : Logo" alt="Teo Logo" class="h-[25px] md:h-8" />
        <AppNav @item-click="handleOverlay" class="hidden lg:block" />
        <div class="flex items-center gap-8">
          <div class="flex gap-2 lg:gap-4 items-center">
            <AppLang :mobileToggle="isMobileNavVisible" class="hidden lg:block" />
            <div>
              <button>
                <i class="text-xl! lg:text-base! pi pi-search"></i>
              </button>
            </div>
          </div>
          <button
            class="hidden lg:block teo-btn-dark teo-btn-size-sm t-body-small lg:t-body-small xl:t-body-large outline-8 px-6 outline-cyan-teo-200">
            Book an Appointment
          </button>
          <div class="lg:hidden">
            <button @click="toggle($event)" class="relative w-8 h-8 flex items-center justify-center">
              <Transition name="rotate-icon" mode="out-in">
                <i v-if="!isMobileNavVisible" key="bars" class="pi pi-bars text-2xl!"></i>
                <i v-else key="times" class="pi pi-times text-2xl!"></i>
              </Transition>
            </button>
          </div>
        </div>
        <!-- Mobile Nav -->
      </div>
    </div>
    <div class="absolute transition-all duration-500 left-0 w-full z-10 bg-white shadow-lg p-8"
      :class="showOverlay ? 'top-full' : 'top-0 -translate-y-full'">
      Overlay for Resouces
    </div>
    <!-- Mobile Nav -->
    <div
      class=" absolute transition-all duration-500  w-full min-h-max h-[calc(100dvh-56px)] z-10 bg-blue-teo-600 text-white shadow-lg p-8"
      :class="isMobileNavVisible ? 'right-0' : '-right-full '">
      <div class="container">
        <!-- <AppNav @item-click="handleOverlay" /> -->
        <AppNav :is-mobile-nav-visible="isMobileNavVisible" @item-click="handleOverlay" />
      </div>
    </div>
  </div>

</template>

<script setup lang="ts">
import Logo from '~/assets/images/Logo.png';
import LogoWhite from '~/assets/images/Logo-White.png';
import AppLang from './AppLang.vue';
import AppNav from './AppNav.vue';


const op = ref();
const showOverlay = ref<boolean>(false);
const isMobileNavVisible = ref<boolean>(false);

const handleOverlay = (items:any) => {
  console.log('Overlay items:', items);
  showOverlay.value = !showOverlay.value;
}

const toggle = (event: Event) => {
  op.value?.toggle(event);
  isMobileNavVisible.value = !isMobileNavVisible.value;
}

</script>

<style scoped>
.rotate-icon-enter-active,
.rotate-icon-leave-active {
  transition: all 0.3s ease;
}

.rotate-icon-enter-from {
  opacity: 0;
  transform: rotate(-90deg) scale(0.8);
}

.rotate-icon-leave-to {
  opacity: 0;
  transform: rotate(90deg) scale(0.8);
}

.rotate-icon-enter-to,
.rotate-icon-leave-from {
  opacity: 1;
  transform: rotate(0deg) scale(1);
}
</style>