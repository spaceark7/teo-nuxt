<template>
  <section class="care-section ">
    <div class="inner py-32">

      <h2 class="title">
        Everything <span>You Need</span> to Care for Your Eyes
      </h2>

      <div class="grid">
        <div v-for="(item, index) in items" :key="index" class="card" :class="{ large: index === 6 }">
          <div class="img-wrap">
            <img :src="item.icon" alt="" />
          </div>

          <p class="label">{{ item.label }}</p>
        </div>

      </div>

      <div ref="textRevealRef" class="text-reveal-wrapper">
        <p class="reveal-text base-text t-headline-large font-medium">
          Every eye is special. The more you understand yours, the easier it become to
          take the right steps, at the right time.
        </p>
        <p class="reveal-text filled-text t-headline-large font-medium" 
           :style="{ clipPath: `inset(0 ${100 - scrollProgress}% 0 0)` }">
          Every eye is special. The more you understand yours, the easier it become to
          take the right steps, at the right time.
        </p>
      </div>

    </div>
  </section>
</template>

<script setup lang="ts">
const items = [
  { label: "Eye Test", icon: "/images/care/eye-test.png" },
  { label: "Ask the Expert", icon: "/images/care/ask-expert.png" },
  { label: "Dry Eye Articles", icon: "/images/care/dry-eye.png" },
  { label: "Expert Opinion", icon: "/images/care/expert-opinion.png" },
  { label: "Eye Stories", icon: "/images/care/stories.png" },
  { label: "Forum", icon: "/images/care/forum.png" },
  { label: "Book an Appointment", icon: "/images/care/appointment.png" },
];

const textRevealRef = ref<HTMLElement | null>(null);
const scrollProgress = ref(0);

onMounted(() => {
  if (!import.meta.client) return;

  const handleScroll = () => {
    if (!textRevealRef.value) return;

    const element = textRevealRef.value;
    const rect = element.getBoundingClientRect();
    const windowHeight = window.innerHeight;
    
    // Animasi sangat cepat - selesai dalam range viewport yang sangat pendek
    const startPoint = windowHeight * 0.85; // Mulai saat 85% dari viewport
    const endPoint = windowHeight * 0.55;   // Selesai saat 55% viewport (range 30% saja)
    const animationRange = startPoint - endPoint;
    
    if (rect.top <= startPoint && rect.top >= endPoint) {
      // Progress dengan multiplier untuk lebih cepat
      const current = startPoint - rect.top;
      const progress = Math.min(Math.max((current / animationRange) * 100, 0), 100);
      scrollProgress.value = progress;
    } else if (rect.top > startPoint) {
      scrollProgress.value = 0;
    } else {
      scrollProgress.value = 100;
    }
  };

  // Listen ke scroll di window dan juga cari parent scroll container
  const scrollContainer = document.querySelector('.snap-y');
  
  if (scrollContainer) {
    scrollContainer.addEventListener('scroll', handleScroll, { passive: true });
  }
  window.addEventListener('scroll', handleScroll, { passive: true });
  
  handleScroll(); // Initial check

  onBeforeUnmount(() => {
    if (scrollContainer) {
      scrollContainer.removeEventListener('scroll', handleScroll);
    }
    window.removeEventListener('scroll', handleScroll);
  });
});
</script>

<style scoped>
/* ===== Background ===== */
.care-section {
  padding: 60px 0 85px;
  background: linear-gradient(180deg,
      #d5edff 0%,
      #d7efff 20%,
      #c5e7ff 50%,
      #bde4ff 75%,
      #a3d7ff 100%);
  display: flex;
  justify-content: center;
  border-radius: 45px;
}

/* ===== Inner container ===== */
.inner {
  max-width: 1180px;
  width: 100%;
  text-align: center;
}

/* ===== Title ===== */
.title {
  font-size: 30px;
  font-weight: 700;
  color: #1A3057;
  margin-bottom: 40px;
}

.title span {
  color: #0A4EBE;
}

/* ===== GRID LIKE IN DESIGN ===== */
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 22px;
  justify-items: stretch;
}

/* CARD STANDARD */
.card {
  background: rgba(255, 255, 255, 0.171);
  backdrop-filter: blur(12px);
  width: 274px;
  height: 324px;
  border-radius: 12px;
  padding: 26px 22px 18px;
  box-shadow: 0 8px 24px rgba(110, 140, 170, 0.23);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.25s ease;
  cursor: pointer;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(110, 140, 170, 0.28);
}

/* IMAGE WRAPPER */
.img-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
  padding-top: 20px;
}

.img-wrap img {
  max-height: 150px;
  max-width: 100%;
  object-fit: contain;
}

/* LABEL TEXT */
.label {
  font-size: 20px;
  font-weight: 500;
  color: #004099;
  margin-top: 28px;
  text-align: left;
}

/* SPECIAL CARD — BOOK AN APPOINTMENT */
.card.large {
  grid-column: span 2;
  width: 574px;
  height: 324px;
  border-radius: 18px;
  background:
    linear-gradient(180deg, rgba(199, 228, 255, 0.737) 0%, rgba(170, 213, 255, 0.65) 100%),
    url('/images/care/bg-appointment.png');
  background-size: cover;
  background-position: center;

  backdrop-filter: blur(10px);
  box-shadow: 0 8px 25px rgba(110, 140, 170, 0.25);

  position: relative;
  overflow: hidden;

  display: flex;
  justify-content: center;
  align-items: center;
}

/* CONTENT CONTAINER */

/* ICON-GROUP */
.card.large .img-wrap {
  display: flex;
  gap: 18px;
  align-items: center;
  justify-content: center;
}

/* ICON + DOCTOR IMAGE SIZE EXACT */
.card.large .img-wrap img {
  max-height: 150px;
  max-width: 100%;
  object-fit: contain;
}

/* BOOK AN APPOINTMENT TEXT */
.card.large .label {
  position: absolute;
  bottom: 26px;
  left: 32px;
  font-size: 20px;
  font-weight: 500;
  color: #004099;
  text-align: left;
}


/* ===== Subtitle ===== */
.subtitle {
  margin-top: 50px;
  font-size: 20px;
  font-weight: 500;
  color: #ffffff;
  opacity: 0.95;
  text-align: center;
  line-height: 1.6;
  width: 100%;
  max-width: 820px;
  margin-left: auto;
  margin-right: auto;
}

/* ===== Text Reveal Animation ===== */
.text-reveal-wrapper {
  position: relative;
  max-width: 700px;
  margin: 4rem auto 0;
  padding: 0 20px;
}

.reveal-text {
  text-align: center;
  margin: 0;
  padding: 0;
  line-height: inherit;
  letter-spacing: inherit;
  word-spacing: inherit;
}

.base-text {
  color: rgba(255, 255, 255, 0.35);
  position: relative;
}

.filled-text {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
  background: linear-gradient(135deg, #0A4EBE 0%, #1E88E5 50%, #2196F3 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  pointer-events: none;
}
</style>
