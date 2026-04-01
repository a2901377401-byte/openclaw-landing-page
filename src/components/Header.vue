<template>
  <header class="header" :class="{ scrolled: isScrolled }">
    <div class="header-container">
      <div class="logo" @click="scrollToTop">
        <img src="/logo2.png" alt="Openclaw" class="logo-img" />
      </div>
      <nav class="nav" :class="{ active: mobileMenuOpen }">
        <a href="#features" class="nav-link">产品服务</a>
        <a href="#cases" class="nav-link">客户案例</a>
        <a href="#pricing" class="nav-link">价格方案</a>
        <a href="#faq" class="nav-link">常见问题</a>
        <el-button type="primary" class="cta-btn" @click="goToWechat">立即咨询</el-button>
      </nav>
      <button class="mobile-menu-btn" @click="mobileMenuOpen = !mobileMenuOpen">
        <el-icon :size="24"><Fold v-if="!mobileMenuOpen" /><Expand v-else /></el-icon>
      </button>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Promotion, Fold, Expand } from '@element-plus/icons-vue'

const isScrolled = ref(false)
const mobileMenuOpen = ref(false)

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/kfid/your-value', '_blank')
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
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: transparent;
  transition: var(--transition);
}

.header.scrolled {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  box-shadow: var(--shadow-sm);
}

.header-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 16px 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.logo-img {
  width: 190px;
  height: 50px;
  object-fit: contain;
}

.logo-text {
  font-size: 22px;
  font-weight: 700;
  color: var(--text-primary);
}

.nav {
  display: flex;
  align-items: center;
  gap: 32px;
}

.nav-link {
  font-size: 15px;
  color: var(--text-secondary);
  text-decoration: none;
  transition: var(--transition);
  position: relative;
}

.nav-link:hover {
  color: var(--primary-color);
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--primary-color);
  transition: var(--transition);
}

.nav-link:hover::after {
  width: 100%;
}

.cta-btn {
  background: var(--bg-gradient);
  border: none;
  padding: 10px 24px;
  font-weight: 600;
}

.mobile-menu-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--text-primary);
}

@media (max-width: 768px) {
  .nav {
    position: fixed;
    top: 72px;
    left: 0;
    right: 0;
    background: white;
    flex-direction: column;
    padding: 24px;
    gap: 20px;
    box-shadow: var(--shadow-md);
    transform: translateY(-100%);
    opacity: 0;
    visibility: hidden;
    transition: var(--transition);
  }

  .nav.active {
    transform: translateY(0);
    opacity: 1;
    visibility: visible;
  }

  .mobile-menu-btn {
    display: block;
  }

  .nav-link::after {
    display: none;
  }
}
</style>
