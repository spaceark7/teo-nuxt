<template>
  <ClientOnly>
    <ScrollSnapOverlay :sections="sections">
      <template #default="{ section, index }">
        <!-- Render komponen yang berbeda berdasarkan index atau properti section -->
        <component :is="section.component" :data="section.data" :index="index" />
      </template>
    </ScrollSnapOverlay>
  </ClientOnly>
  <!-- <div class="snap-container">
    <div class="snap-section">

      <HeroCarousel />
    </div>
    <ScrollSnapOverlay :sections="[
      { title: 'Section 1', subtitle: 'Subtitle 1', bgClass: 'bg-red-500' },
      { title: 'Section 2', subtitle: 'Subtitle 2', bgClass: 'bg-green-500' },
      { title: 'Section 3', subtitle: 'Subtitle 3', bgClass: 'bg-blue-500' },
      { title: 'Section 4', subtitle: 'Subtitle 4', bgClass: 'bg-yellow-500' }
    ]" />
    <section v-for="n in 4" :key="n" class="snap-section">
      <div class="inner" :style="{ backgroundColor: colors[n - 1] }">
        <h2 class="text-white text-6xl font-bold">Section {{ n }}</h2>
      </div>
    </section>
  </div> -->
</template>

<script setup lang="ts">
import HeroCarousel from '~/components/Home/HeroCarousel.vue';
import MockComponent from '~/components/Home/MockComponent.vue';

const colors = ['#FF6B6B', '#4ECDC4', '#9B59B6', '#F39C12'];
type SectionType = {
  title: string;
  subtitle: string;
  bgClass: string;
  component: any;
  data: {
    heading: string;
    description: string;
  };
};

const sections: SectionType[] = [
  {
    title: 'Selamat Datang',
    subtitle: 'Scroll ke bawah untuk melihat efek',
    bgClass: 'section-bg-color-top',
    component: HeroCarousel,
    data: {
      // Data untuk HeroSection
      heading: 'Welcome to Our Site',
      description: 'Discover amazing things'
    }
  },
  {
    title: 'Bagian Kedua',
    subtitle: 'Menindih bagian pertama',
    bgClass: 'section-bg-color-bottom',
    component: MockComponent,
    data: {
      // Data untuk AboutSection
      heading: 'About Us',
      description: 'About us information...'
    }
  },
  {
    title: 'Bagian Ketiga',
    subtitle: 'Efek overlay yang smooth',
    bgClass: 'section-bg-color-top',
    component: MockComponent,
    data: {
      // Data untuk ServicesSection
      heading: 'Our Services',
      description: 'Details about our services',
    }
  },
  {
    title: 'Bagian Keempat',
    subtitle: 'Transisi yang natural',
    bgClass: 'section-bg-color-bottom',
    component: MockComponent,
    data: {
      // Data untuk PortfolioSection
      heading: 'Our Portfolio',
      description: 'Showcase of our work',
    }
  },
  {
    title: 'Bagian Terakhir',
    subtitle: 'Terima kasih telah melihat!',
    bgClass: 'section-bg-color-top',
    component: MockComponent,
    data: {
      // Data untuk ContactSection
      heading: 'Contact Us',
      description: 'Get in touch with us',
    }
  }
]
</script>

<style scoped>
.snap-container {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

.snap-section {
  position: relative;
  height: 100vh;
  scroll-snap-align: start;
}

.inner {
  position: sticky;
  top: 0;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  border-radius: 0 0 30px 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.snap-section:nth-child(1) .inner {
  z-index: 4;
}

.snap-section:nth-child(2) .inner {
  z-index: 3;
  top: 20px;
}

.snap-section:nth-child(3) .inner {
  z-index: 2;
  top: 40px;
}

.snap-section:nth-child(4) .inner {
  z-index: 1;
  top: 60px;
}


</style>