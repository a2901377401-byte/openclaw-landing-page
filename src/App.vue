<template>
  <div class="landing-page">
    <Header />
    <main>
      <HeroSection />
      <PainAndSolution />
      <ImageSlider />
      <ProductSection />
      <SocialProof />
      <SceneVideos />
      <PricingSection />
      <FAQSection />
    </main>
    <Footer />
    <transition name="fade">
      <div class="back-to-top" v-show="showBackToTop" @click="scrollToTop">
        <el-icon><Top /></el-icon>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Top } from '@element-plus/icons-vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import HeroSection from './components/HeroSection.vue'
import PainAndSolution from './components/PainAndSolution.vue'
import ProductSection from './components/ProductSection.vue'
import ImageSlider from './components/ImageSlider.vue'
import SocialProof from './components/SocialProof.vue'
import SceneVideos from './components/SceneVideos.vue'
import FAQSection from './components/FAQSection.vue'
import PricingSection from './components/PricingSection.vue'

const showBackToTop = ref(false)

const handleScroll = () => {
  showBackToTop.value = window.scrollY > 300
}

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  position: relative;
}

.back-to-top {
  position: fixed;
  bottom: 40px;
  right: 40px;
  width: 48px;
  height: 48px;
  background: var(--bg-gradient);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  cursor: pointer;
  box-shadow: var(--shadow-md);
  transition: all 0.3s ease;
  z-index: 1000;
}

.back-to-top:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@media (max-width: 768px) {
  .back-to-top {
    bottom: 24px;
    right: 24px;
    width: 44px;
    height: 44px;
  }
}
</style>
