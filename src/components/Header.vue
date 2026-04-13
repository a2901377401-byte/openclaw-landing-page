<template>
  <header class="header" :class="{ scrolled: isScrolled }">
    <div class="header-container">
      <div class="logo" @click="handleLogoClick">
        <img src="/logo2.png" alt="Openclaw" class="logo-img" />
      </div>
      <nav class="nav" :class="{ active: mobileMenuOpen }">
        <a href="#features" class="nav-link" @click="handleNavClick">产品服务</a>
        <a class="nav-link knowledge-link" @click="goToKnowledge">知识库</a>
        <a href="#cases" class="nav-link" @click="handleNavClick">客户案例</a>
        <a href="#pricing" class="nav-link" @click="handleNavClick">价格方案</a>
        <a href="#faq" class="nav-link" @click="handleNavClick">常见问题</a>
        <el-button type="primary" class="cta-btn" @click="goToWechat">立即咨询</el-button>
      </nav>
      <button class="mobile-menu-btn" @click="mobileMenuOpen = !mobileMenuOpen">
        <el-icon :size="24">
          <Fold v-if="!mobileMenuOpen" />
          <Expand v-else />
        </el-icon>
      </button>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Fold, Expand } from '@element-plus/icons-vue'
import CONTACT_INFO from '../config/contact.js'

const emit = defineEmits(['navigate'])

const isScrolled = ref(false)
const mobileMenuOpen = ref(false)

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/ca/cawcde427f39be25db', '_blank')
}

const handleLogoClick = () => {
  emit('navigate', 'home')
  mobileMenuOpen.value = false
}

const handleNavClick = () => {
  mobileMenuOpen.value = false
}

const goToKnowledge = () => {
  mobileMenuOpen.value = false
  const element = document.getElementById('knowledge')
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: transparent;
  transition: all 0.3s ease;
}

.header.scrolled {
  background: rgba(255, 255, 255, 0.98);
  backdrop-filter: blur(12px);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.08);
}

.header-container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 18px 32px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.logo:hover {
  transform: scale(1.02);
}

.logo-img {
  width: 180px;
  height: 46px;
  object-fit: contain;
}

.nav {
  display: flex;
  align-items: center;
  gap: 36px;
}

.nav-link {
  font-size: 15px;
  color: var(--text-secondary);
  text-decoration: none;
  transition: all 0.3s ease;
  position: relative;
  cursor: pointer;
  font-weight: 500;
  padding: 4px 0;
}

.nav-link:hover {
  color: var(--primary-color);
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #A855F7, #C084FC);
  border-radius: 1px;
  transition: width 0.3s ease;
}

.nav-link:hover::after {
  width: 100%;
}

.cta-btn {
  background: linear-gradient(135deg, #A855F7 0%, #C084FC 100%) !important;
  border: none !important;
  padding: 10px 28px !important;
  font-weight: 600 !important;
  font-size: 14px !important;
  border-radius: 8px !important;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.25) !important;
  transition: all 0.3s ease !important;
}

.cta-btn:hover {
  transform: translateY(-2px) !important;
  box-shadow: 0 6px 20px rgba(168, 85, 247, 0.35) !important;
}

.mobile-menu-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--text-primary);
  padding: 8px;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.mobile-menu-btn:hover {
  background: rgba(168, 85, 247, 0.1);
}

@media (max-width: 1024px) {
  .header-container {
    padding: 16px 24px;
  }

  .nav {
    gap: 28px;
  }

  .nav-link {
    font-size: 14px;
  }
}

@media (max-width: 768px) {
  .header-container {
    padding: 14px 20px;
  }

  .logo-img {
    width: 150px;
    height: 38px;
  }

  .nav {
    position: fixed;
    top: 70px;
    left: 0;
    right: 0;
    background: rgba(255, 255, 255, 0.98);
    backdrop-filter: blur(12px);
    flex-direction: column;
    padding: 28px 24px;
    gap: 24px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
    transform: translateY(-120%);
    opacity: 0;
    visibility: hidden;
    transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
    border-radius: 0 0 16px 16px;
  }

  .nav.active {
    transform: translateY(0);
    opacity: 1;
    visibility: visible;
  }

  .mobile-menu-btn {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .nav-link {
    font-size: 16px;
    width: 100%;
    text-align: center;
    padding: 12px 0;
  }

  .nav-link::after {
    display: none;
  }

  .cta-btn {
    width: 100%;
    padding: 14px 24px !important;
    font-size: 16px !important;
  }
}
</style>
