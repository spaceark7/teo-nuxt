<template>
  <div class="relative w-full min-h-[100dvh] bg-hero-gradient  overflow-hidden">
    <!-- Slides Container -->
    <div class="relative h-full overflow-hidden">
      <div class="flex transition-transform duration-500 ease-in-out"
        :style="{ transform: `translateX(-${currentSlide * 100}%)` }">
        <!-- Slide 1 -->
        <HeroCarouselSlider v-for="(slide, index) in mockSliderData" :key="slide.id" :media="slide.media" :texts="{
            headlineText: slide.headlineText,
            titleLine1: slide.titleLine1,
            titleLine2: slide.titleLine2,
            cta1Text: slide.cta1Text,
            cta2Text: slide.cta2Text
          }" />


      </div>

      <!-- Number Indicators -->
      <div class="absolute bottom-6 md:bottom-8 left-0 w-full z-20">
        <div class="max-w-[1450px] mx-auto px-7 md:px-9 xl:px-16 flex gap-3">
          <button v-for="(index, idx) in totalSlidesArray" :key="index" @click="goToSlide(idx)"
            class="relative w-12 h-12 rounded-full flex items-center justify-center text-sm font-semibold transition-colors duration-300"
            :class="currentSlide === idx ? 'text-blue-teo-500' : 'text-white/80'">

            <!-- Only show border for active slide -->
            <span v-if="currentSlide === idx"
              class="absolute inset-0 rounded-full border-[3px] border-white transition-colors duration-300"></span>

            <template v-if="currentSlide === idx">
              <svg class="absolute inset-0 w-12 h-12 transform -rotate-90" viewBox="0 0 48 48"
                style="filter: drop-shadow(0 3px 8px rgba(0,0,0,0.3))">
                <!-- Background circle (white outline) - static -->
                <circle cx="24" cy="24" r="20" fill="none" stroke="white" stroke-width="3" />
                <!-- Progress circle (blue that fills in) - animated -->
                <circle cx="24" cy="24" r="20" fill="none" stroke="#1e6df3" stroke-width="6"
                  :stroke-dasharray="2 * Math.PI * 20" :stroke-dashoffset="getProgressOffset(progress)"
                  class="transition-all duration-100 ease-linear" />
              </svg>
              <span class="relative z-10 text-blue-teo-500" style="text-shadow: 0 2px 4px rgba(0,0,0,0.15)">{{
                formatSlideNumber(idx) }}</span>
            </template>

            <template v-else>
              <span class="relative z-10 text-white" style="text-shadow: 0 2px 4px rgba(0,0,0,0.2)">{{
                formatSlideNumber(idx) }}</span>
            </template>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from 'vue';
import HeroCarouselSlider from './HeroCarouselSlider.vue';
import HeroSlide1Video from '~/assets/video/f1.mp4';
import Slide2 from '~/assets/images/slide-2.webp';
import Slide3 from '~/assets/images/slide-3.webp';
import Slide4Video from '~/assets/video/f4.mp4';


const mockSliderData = [
  {
    id: 1,
    headlineText: undefined,
    titleLine1: 'Do You Have',
    titleLine2: 'Dry Eye Symptoms?',
    cta1Text: 'Find Eye Doctor',
    cta2Text: 'Try the Eye Test!',
    media: {
      type: 'video',
      src: HeroSlide1Video
    }
  },
  {
    id: 2,
    headlineText: 'Got a Question?',
    titleLine1: 'Our Experts Are',
    titleLine2: 'Taking Questions This Month',
    cta1Text: 'Ask Now',
    cta2Text: 'Learn More',
    media: {
      type: 'image',
      src: Slide2
    }
  },
  {
    id: 3,
    headlineText: 'July is Dry Eye Awareness Month',
    titleLine1: 'Take a Step Toward',
    titleLine2: 'Better Eye Health',
    cta1Text: 'Get Started',
    cta2Text: 'Learn More',
    media: {
      type: 'image',
      src: Slide3
    }
  },
  
  {
    id: 4,
    headlineText: 'Got a Question?',
    titleLine1: 'Small Steps Can',
    titleLine2: 'Mean Healthier Eyes',
    cta1Text: 'Find Out How',
    cta2Text: 'Book Appointment',
    media: {
      type: 'video',
      src: Slide4Video
    }
  }
  // Additional slides can be added here
];

const currentSlide = ref(0);
const totalSlides = ref(mockSliderData.length);
const totalSlidesArray = Array.from({ length: totalSlides.value }, (_, i) => i );
const autoPlayInterval = ref<ReturnType<typeof setTimeout> | null>(null);
const autoPlayDelay = 7000;
const progressInterval = ref<ReturnType<typeof setTimeout> | null>(null);
const progress = ref(0);
const progressStartTime = ref(0);

const goToSlide = (index: number) => {
  stopProgress();
  currentSlide.value = index;
};

const nextSlide = () => {
  stopProgress();
  currentSlide.value = (currentSlide.value + 1) % totalSlides.value;
};

const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + totalSlides.value) % totalSlides.value;
  resetAutoPlay();
};

const startAutoPlay = () => {
  startProgress();
};

const stopAutoPlay = () => {
  if (autoPlayInterval.value) {
    clearInterval(autoPlayInterval.value);
    autoPlayInterval.value = null;
  }
};

const resetAutoPlay = () => {
  stopAutoPlay();
  startAutoPlay();
};

const startProgress = () => {
  progress.value = 0;
  progressStartTime.value = Date.now();

  const updateProgress = () => {
    const elapsed = Date.now() - progressStartTime.value;
    const newProgress = Math.min((elapsed / autoPlayDelay) * 100, 100);
    progress.value = newProgress;

    if (newProgress >= 100) {
      nextSlide();
    } else {
      progressInterval.value = setTimeout(updateProgress, 16);
    }
  };

  updateProgress();
};

const stopProgress = () => {
  if (progressInterval.value) {
    clearTimeout(progressInterval.value);
    progressInterval.value = null;
  }
};

const resetProgress = () => {
  stopProgress();
  progress.value = 0;
  startProgress();
};

const formatSlideNumber = (index: number) => {
  return String(index + 1).padStart(2, '0');
};

const getProgressOffset = (progressPercent: number) => {
  const circumference = 2 * Math.PI * 20;
  const offset = circumference - (progressPercent / 100) * circumference;
  return offset;
};

onMounted(() => {
  startAutoPlay();
});

onUnmounted(() => {
  stopAutoPlay();
  stopProgress();
});

watch(currentSlide, () => {
  stopProgress();
  progress.value = 0;
  startProgress();
});
</script>

<style scoped>

</style>