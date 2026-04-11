<template>
  <section class="hero">
    <div class="container">
      <div class="hero-content">
        <div class="hero-badge">
          <el-icon><Star /></el-icon>
          <span>已服务 500+ 企业客户</span>
        </div>
        <h1 class="hero-title">
          <span class="highlight">「数字员工岗位」</span><br>
          而不是 AI 工具
        </h1>
        <p class="hero-subtitle">
          不讲概念：今天下单，今晚就在企微/飞书里拥有你的 AI 数字员工同事。
        </p>
        <div class="hero-actions">
          <el-button type="primary" size="large" class="cta-button" @click="goToWechat">
            <el-icon><Promotion /></el-icon>
            即刻行动，部署你的首位数字员工
          </el-button>
        </div>
        <div class="hero-stats">
          <div class="stat-item">
            <span class="stat-number">500+</span>
            <span class="stat-label">企业客户</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">10K+</span>
            <span class="stat-label">数字员工</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">99.9%</span>
            <span class="stat-label">可用率</span>
          </div>
        </div>
      </div>
      <div class="hero-visual">
        <ImageSlider />
      </div>
    </div>
  </section>
</template>

<script setup>
import { Star, Promotion } from '@element-plus/icons-vue'
import ImageSlider from './ImageSlider.vue'
import CONTACT_INFO from '../config/contact.js'

const goToWechat = () => {
  window.location.href = `weixin://dl/addfriend/${CONTACT_INFO.wechat}`
}
</script>

<style scoped>
.hero {
  display: flex;
  align-items: center;
  padding: 120px 0 80px;
  position: relative;
  overflow: hidden;
  background: linear-gradient(180deg, #ffffff 0%, #ffffff 100%);
}

.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.hero-circle {
  position: absolute;
  border-radius: 50%;
  opacity: 0.4;
}

.circle-1 {
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, rgba(168, 85, 247, 0.15) 0%, transparent 70%);
  top: -200px;
  right: -100px;
}

.circle-2 {
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(192, 132, 252, 0.12) 0%, transparent 70%);
  bottom: -100px;
  left: -100px;
}

.circle-3 {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(168, 85, 247, 0.1) 0%, transparent 70%);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.hero .container {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 40px;
  align-items: center;
  position: relative;
  z-index: 1;
}

.hero-content {
  max-width: 600px;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(168, 85, 247, 0.1);
  color: var(--primary-color);
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 24px;
}

.hero-title {
  font-size: 52px;
  font-weight: 800;
  line-height: 1.2;
  margin-bottom: 20px;
  color: var(--text-primary);
}

.hero-title .highlight {
  background: var(--bg-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 22px;
  font-weight: 600;
  color: var(--primary-color);
  margin-bottom: 16px;
  line-height: 1.5;
}

.hero-desc {
  font-size: 16px;
  color: var(--text-secondary);
  line-height: 1.8;
  margin-bottom: 32px;
}

.pc-only {
  display: inline;
}

.hero-actions {
  display: flex;
  gap: 16px;
  margin-bottom: 48px;
}

.cta-button {
  background: var(--bg-gradient) !important;
  border: none !important;
  padding: 16px 32px !important;
  font-size: 16px !important;
  font-weight: 600 !important;
  height: auto !important;
  border-radius: 12px !important;
}

.cta-button .el-icon {
  margin-right: 8px;
}

.secondary-button {
  padding: 16px 32px !important;
  font-size: 16px !important;
  font-weight: 600 !important;
  height: auto !important;
  border-radius: 12px !important;
  border: 2px solid #e0e6ed !important;
}

.hero-stats {
  display: flex;
  align-items: center;
  gap: 32px;
}

.stat-item {
  display: flex;
  flex-direction: column;
}

.stat-number {
  font-size: 32px;
  font-weight: 700;
  color: var(--text-primary);
}

.stat-label {
  font-size: 14px;
  color: var(--text-secondary);
}

.stat-divider {
  width: 1px;
  height: 40px;
  background: #e0e6ed;
}

.hero-visual {
  position: relative;
}

.hero-visual :deep(.image-slider) {
  border-radius: 16px;
  box-shadow: var(--shadow-lg);
  max-width: none !important;
  width: 100%;
}

.hero-visual :deep(.slider-container) {
  overflow: visible;
}

@media (max-width: 1024px) {
  .hero .container {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .hero-content {
    max-width: 100%;
    text-align: center;
  }

  .hero-title {
    font-size: 36px;
  }

  .hero-subtitle {
    font-size: 18px;
  }

  .hero-desc {
    font-size: 15px;
  }

  .pc-only {
    display: none;
  }

  .hero-desc br {
    display: none;
  }

  .hero-actions {
    justify-content: center;
  }

  .hero-stats {
    justify-content: center;
  }

  .hero-visual {
    max-width: 500px;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .hero {
    padding: 100px 0 60px;
    min-height: auto;
  }

  .hero-title {
    font-size: 28px;
  }

  .hero-subtitle {
    font-size: 16px;
  }

  .hero-actions {
    flex-direction: column;
  }

  .cta-button,
  .secondary-button {
    width: 100%;
  }

  .hero-stats {
    flex-wrap: wrap;
    gap: 20px;
  }

  .stat-divider {
    display: none;
  }

  .floating-card {
    display: none;
  }
}
</style>
