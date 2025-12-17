<template>
  <div class="h-screen overflow-y-scroll snap-y snap-mandatory scroll-smooth">
    <section v-for="(section, index) in sections" :key="index" ref="sectionRefs"
      class="h-screen snap-center flex items-center justify-center"
      :class="index % 2 === 0 ? 'section-bg-color-top' : 'section-bg-color-bottom'">
      <div class="section-content" :class="{ 'is-visible': visibleSections[index] }">
        <slot :section="section" :index="index" />
      </div>
    </section>
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
const sectionRefs = ref<HTMLElement[]>([])
const visibleSections = ref<boolean[]>(props.sections.map((_, index) => index === 0))

// Intersection Observer hanya di client-side
onMounted(() => {
  if (!import.meta.client) return

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        const index = sectionRefs.value.indexOf(entry.target as HTMLElement)
        if (index !== -1) {
          visibleSections.value[index] = entry.isIntersecting
        }
      })
    },
    {
      threshold: 0.5,
    }
  )

  sectionRefs.value.forEach((section) => {
    if (section) observer.observe(section)
  })

  onBeforeUnmount(() => {
    observer.disconnect()
  })
})
</script>

<style scoped>
/* Custom scrollbar (optional) */
::-webkit-scrollbar {
  width: 0;
  background: transparent;
}

/* Content transition with Intersection Observer */
.section-content {
  opacity: 0;
  transform: translateY(50px) scale(0.95);
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.section-content.is-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
}
</style>