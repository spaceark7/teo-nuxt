<template>
  <div class=" w-full fixed top-0 left-0 z-30">
    <div class="relative z-20 transition-all duration-500 "
      :class="isMobileNavVisible ? 'bg-blue-teo-600 text-white' : 'bg-white'">
      <!-- Logo and Nav -->
      <div
        class="h-[56px] lg:h-[80px] px-7 md:px-9 xl:px-16  2xl:max-w-10/12 mx-auto flex items-center justify-between">
        <img :src="isMobileNavVisible ? LogoWhite : Logo" alt="Teo Logo" class="h-[25px] md:h-8" />
        <AppNav @item-click="handleOverlay" class="hidden xl:block" />
        <div class="flex items-center gap-8">
          <div class="flex gap-2 lg:gap-4 items-center">
            <AppLang :mobileToggle="isMobileNavVisible" class="hidden xl:block" />
            <div>
              <button>
                <i class="text-xl! lg:text-base! pi pi-search"></i>
              </button>
            </div>
          </div>
          <button
            class="hidden xl:block teo-btn-dark teo-btn-size-sm t-body-small lg:t-body-small xl:t-body-large outline-8 px-6 outline-cyan-teo-200">
            Book an Appointment
          </button>
          <div class="xl:hidden">
            <button @click="toggle($event)" class="relative w-8 h-8 flex items-center justify-center">
              <Transition name="rotate-icon" mode="out-in">
                <i v-if="!isMobileNavVisible" key="bars" class="pi pi-bars text-2xl!"></i>
                <i v-else key="times" class="pi pi-times text-2xl!"></i>
              </Transition>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="absolute transition-all duration-500 h-60 flex gap-12 pr-16 left-0 w-full z-10 bg-white shadow-lg "
      :class="showOverlay ? 'top-full' : 'top-0 -translate-y-full'">
      <div class="relative flex items-center justify-center px-12 py-8 w-3/12">
        <img :src="ResourcesBg" alt="Resources Banner" class="absolute inset-0 w-full h-full  object-cover" />
        <h2 class="t-headline-medium text-cyan-teo-700 z-2 font-regular">Resources</h2>
      </div>
      <!-- Articles Category -->
      <!-- <div class="pt-8 pb-12 flex mx-auto justify-baseline gap-3 w-9/12">
        <div v-for="(category, index) in articlesCategories" :key="index" class="w-[216px] flex flex-col gap-3">
          <div class="w-full h-24">
            <img :src="category.media.src" :alt="category.label" class=" self-start object-contain" />

          </div>
          <h3 class="t-body-medium  text-blue-teo-500 font-medium">{{ category.label }}</h3>
          <p class="t-label-medium  text-cool-gray-teo-500">{{ category.description }}</p>
        </div>
      </div> -->

      <div class="pt-8 pb-12 relative w-9/12">
        <!-- Navigation Arrows -->
        <button @click="scrollCarousel('left')" v-show="canScrollLeft"
          class="absolute left-0 top-1/2 -translate-y-1/2 z-10 w-10 h-10 bg-white rounded-full shadow-lg flex items-center justify-center text-blue-teo-500 hover:bg-blue-teo-50 transition-colors">
          <i class="pi pi-chevron-left"></i>
        </button>

        <button @click="scrollCarousel('right')" v-show="canScrollRight"
          class="absolute right-0 top-1/2 -translate-y-1/2 z-10 w-10 h-10 bg-white rounded-full shadow-lg flex items-center justify-center text-blue-teo-500 hover:bg-blue-teo-50 transition-colors">
          <i class="pi pi-chevron-right"></i>
        </button>

        <!-- Scrollable Container -->
        <div ref="carouselContainer" @scroll="updateScrollState"
          class="flex gap-6 overflow-x-auto scroll-smooth scrollbar-hide px-12">
          <div v-for="(category, index) in articlesCategories" :key="index"
            class="max-w-[216px] shrink-0 flex flex-col gap-3 cursor-pointer group">
            <div class="w-full h-24 overflow-hidden rounded-lg">
              <img :src="category.media.src" :alt="category.label"
                class="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105" />
            </div>
            <h3 class="t-body-medium text-blue-teo-500 font-medium group-hover:text-cyan-teo-500 transition-colors">
              {{ category.label }}
            </h3>
            <p class="t-label-medium text-cool-gray-teo-500">
              {{ category.description }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- Mobile Nav -->
    <div
      class="fixed top-[56px] lg:top-[80px] right-0 w-full h-[calc(100dvh-56px)] lg:h-[calc(100dvh-80px)] z-10 overflow-hidden pointer-events-none">
      <div
        class="absolute transition-all duration-500 ease-in-out w-full h-full bg-blue-teo-600 text-white shadow-lg overflow-y-auto pointer-events-auto"
        :class="isMobileNavVisible ? 'right-0' : '-right-full'">
        <div class="px-7 md:px-9 py-8">
          <AppNav :is-mobile-nav-visible="isMobileNavVisible" @item-click="handleOverlay" />
        </div>
      </div>
    </div>
  </div>

</template>

<script setup lang="ts">
import Logo from '~/assets/images/Logo.png';
import LogoWhite from '~/assets/images/Logo-White.png';
import ResourcesBg from '~/assets/images/resources-banner.png';
import AppLang from './AppLang.vue';
import AppNav from './AppNav.vue';


const op = ref();
const showOverlay = ref<boolean>(false);
const isMobileNavVisible = ref<boolean>(false);
const articlesCategories = ref<any[]>([

]);

// Carousel refs
const carouselContainer = ref<HTMLElement | null>(null);
const canScrollLeft = ref(false);
const canScrollRight = ref(false);

const handleOverlay = (items: any[]) => {
  console.log('Overlay items:', items);

  articlesCategories.value = items;
  showOverlay.value = !showOverlay.value;

  // Update scroll state after DOM update
  nextTick(() => {
    updateScrollState();
  });
}

const toggle = (event: Event) => {
  op.value?.toggle(event);
  isMobileNavVisible.value = !isMobileNavVisible.value;
}

const scrollCarousel = (direction: 'left' | 'right') => {
  if (!carouselContainer.value) return;

  const scrollAmount = 240; // Width of card (216px) + gap (24px)
  const currentScroll = carouselContainer.value.scrollLeft;

  carouselContainer.value.scrollTo({
    left: direction === 'left'
      ? currentScroll - scrollAmount
      : currentScroll + scrollAmount,
    behavior: 'smooth'
  });
}

const updateScrollState = () => {
  if (!carouselContainer.value) return;

  const { scrollLeft, scrollWidth, clientWidth } = carouselContainer.value;

  canScrollLeft.value = scrollLeft > 0;
  canScrollRight.value = scrollLeft < scrollWidth - clientWidth - 1;
}

// Watch for overlay visibility to update scroll state
watch(showOverlay, (newVal) => {
  if (newVal) {
    nextTick(() => {
      updateScrollState();
    });
  }
});

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
/* Hide scrollbar */
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}

.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>