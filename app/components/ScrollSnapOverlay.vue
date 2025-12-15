<template>
  <div class="relative bg-[#8DD6F6]">
    <!-- Fixed Sections -->
    <section v-for="(section, index) in sections" :key="index" :data-section="index" :style="{
      zIndex: index + 1,
      transform: sectionTransforms[index],
      opacity: sectionOpacities[index]
    }"
      class="fixed top-0 left-0 right-0 h-dvh flex items-center justify-center text-white font-bold pointer-events-none transition-all duration-300 ease-out"
      :class="index % 2 === 0 ? 'section-bg-color-top' : 'section-bg-color-bottom'">
      <div class="w-full h-full flex items-center justify-center relative z-10 pointer-events-none">
        <!-- Slot untuk konten custom per section -->
        <slot :section="section" :index="index">
          <!-- Default content jika slot tidak diisi -->
          <div class="text-center">
            <div class="text-[8rem] opacity-30 mb-4">{{ String(index + 1).padStart(2, '0') }}</div>
            <div class="text-5xl mb-2">{{ section.title }}</div>
            <div class="text-xl opacity-80 font-normal">{{ section.subtitle }}</div>
          </div>
        </slot>
      </div>
    </section>

    <!-- Scrollable Container with Spacers -->
    <div ref="scrollContainer" class="h-dvh overflow-y-auto scroll-smooth"
      style="scroll-snap-type: y mandatory; -webkit-overflow-scrolling: touch;" @scroll="handleScroll">
      <div v-for="index in sections.length" :key="index" class="h-dvh relative" style="scroll-snap-align: start;" />
    </div>

    <!-- Scroll Indicator -->
    <div :style="{ opacity: scrollIndicatorOpacity }"
      class="fixed bottom-8 left-1/2 -translate-x-1/2 z-[1000] pointer-events-none transition-opacity duration-300 animate-bounce">
      <svg class="w-8 h-8 fill-white drop-shadow-md" viewBox="0 0 24 24">
        <path d="M7.41 8.59L12 13.17l4.59-4.58L18 10l-6 6-6-6 1.41-1.41z" />
      </svg>
    </div>

    <!-- Navigation Dots -->
    <!-- <div class="fixed right-8 top-1/2 -translate-y-1/2 z-[1000] flex flex-col gap-4">
      <button v-for="(section, index) in sections" :key="index" :data-index="index" @click="scrollToSection(index)"
        class="w-3 h-3 rounded-full border-2 border-white transition-all duration-300 pointer-events-auto"
        :class="currentSection === index ? 'bg-white scale-125' : 'bg-white/50 hover:bg-white hover:scale-110'" />
    </div> -->
  </div>
</template>

<script setup lang="ts">
interface Section {
  title: string
  subtitle: string
  bgClass: string
}

// Props
const props = defineProps<{
  sections: any[]
}>()

// Refs
const scrollContainer = ref<HTMLElement | null>(null)
const currentSection = ref(0)
const scrollIndicatorOpacity = ref(0.7)
const sectionTransforms = ref<string[]>([])
const sectionOpacities = ref<number[]>([])

// Initialize transforms and opacities
onMounted(() => {
  sectionTransforms.value = props.sections.map(() => 'translateY(100vh)')
  sectionOpacities.value = props.sections.map(() => 0)
  updateSections()
})

// Update sections based on scroll
const updateSections = () => {
  if (!scrollContainer.value) return

  const scrollTop = scrollContainer.value.scrollTop
  const viewportHeight = window.innerHeight

  const transforms: string[] = []
  const opacities: number[] = []

  props.sections.forEach((_, index) => {
    const sectionScroll = index * viewportHeight
    const progress = (scrollTop - sectionScroll) / viewportHeight

    // Clamp progress between 0 and 1
    const clampedProgress = Math.max(0, Math.min(1, progress + 1))

    // Calculate transform based on progress
    const translateY = (1 - clampedProgress) * 100

    transforms.push(`translateY(${translateY}vh)`)
    opacities.push(clampedProgress)
  })

  sectionTransforms.value = transforms
  sectionOpacities.value = opacities

  // Update current section
  currentSection.value = Math.round(scrollTop / viewportHeight)

  // Update scroll indicator
  scrollIndicatorOpacity.value = scrollTop > 100 ? 0 : 0.7
}

// Handle scroll event
const handleScroll = () => {
  updateSections()
}

// Scroll to specific section
const scrollToSection = (index: number) => {
  if (!scrollContainer.value) return

  const targetScroll = index * window.innerHeight
  scrollContainer.value.scrollTo({
    top: targetScroll,
    behavior: 'smooth'
  })
}

// Keyboard navigation
onMounted(() => {

  // Keyboard navigation
  const handleKeydown = (e: KeyboardEvent) => {
    if (e.key === 'ArrowDown' || e.key === 'PageDown') {
      e.preventDefault()
      if (currentSection.value < props.sections.length - 1) {
        scrollToSection(currentSection.value + 1)
      }
    } else if (e.key === 'ArrowUp' || e.key === 'PageUp') {
      e.preventDefault()
      if (currentSection.value > 0) {
        scrollToSection(currentSection.value - 1)
      }
    }
  }

  document.addEventListener('keydown', handleKeydown)

  onBeforeUnmount(() => {
    document.removeEventListener('keydown', handleKeydown)
  })
})
</script>

<style scoped>
/* Custom scrollbar (optional) */
::-webkit-scrollbar {
  width: 0;
  background: transparent;
}


</style>