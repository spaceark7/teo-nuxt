<template>
  <div class="min-w-full relative min-h-[100dvh] overflow-hidden">
    <!-- Background Video/Image - Absolute, full coverage -->
    <div class="absolute inset-0 w-full h-full">
      <div class="absolute lg:right-0 lg:top-0 lg:bottom-0 bottom-0 left-0 right-0 lg:left-auto w-full h-1/2 lg:h-full">
        <!-- Video -->
        <video v-if="props.media.type === 'video'" autoplay muted loop playsinline class="w-full h-full object-cover">
          <source :src="props.media.src" type="video/mp4" />
          Your browser does not support the video tag.
        </video>
        <!-- Image -->
        <img v-else :src="props.media.src" alt="" class="w-full h-full object-cover" />

        <!-- Gradient Overlay: Left fade on desktop, Top fade on mobile -->
        <!-- Gradient Overlay: Left fade on desktop, Top fade on mobile -->
        <div
          class="absolute inset-0 bg-gradient-to-b from-[#D8E5F0] via-transparent to-transparent lg:bg-gradient-to-r lg:from-[#D8E5F0] lg:via-[#D8E5F0]/50 lg:to-transparent" :class="props.media.type === 'image' ? 'lg:hidden' : 'absolute'">
        </div>
      </div>
    </div>

    <!-- Content - Relative, on top of background -->
    <div class="relative z-10 h-full flex items-center md:items-center items-start">
      <div class="max-w-[1450px] mx-auto px-7 md:px-9 xl:px-16 w-full py-16 md:py-24">
        <div class="w-full md:w-1/2">
          <!-- Headline Text -->
          <p v-if="props.texts.headlineText" class="t-headline-large font-medium text-cyan-teo-600 mb-2">
            {{ props.texts.headlineText }}
          </p>

          <!-- Title -->
          <h1 v-if="props.texts.titleLine1 || props.texts.titleLine2"
            class="font-medium text-[40px] md:text-[50px] lg:text-[56px] leading-[1.15] tracking-tight mb-8">
            <span v-if="props.texts.titleLine1" class="block text-cool-gray-teo-900">
              {{ props.texts.titleLine1 }}
            </span>
            <span v-if="props.texts.titleLine2" class="block text-blue-teo-500">
              {{ props.texts.titleLine2 }}
            </span>
          </h1>

          <!-- CTA Buttons -->
          <div class="flex flex-col sm:flex-row gap-4">
            <button v-if="props.texts.cta1Text"
              class="teo-btn-dark teo-btn-size-lg flex items-center justify-center gap-1.5">
              <span>{{ props.texts.cta1Text }}</span>
              <i class="pi pi-arrow-right"></i>
            </button>
            <button v-if="props.texts.cta2Text"
              class="teo-btn-light teo-btn-size-lg flex items-center justify-center gap-1.5">
              <span>{{ props.texts.cta2Text }}</span>
              <i class="pi pi-eye"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  media: {
    type: string;
    src: string;
  },
  texts: {
    headlineText?: string;
    titleLine1: string;
    titleLine2: string;
    cta1Text: string;
    cta2Text: string;
  }
}>();
</script>

<style scoped></style>