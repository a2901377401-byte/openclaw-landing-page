<template>
  <section class="hero">
    <div class="hero-bg">
      <div class="hero-circle circle-1"></div>
      <div class="hero-circle circle-2"></div>
      <div class="hero-circle circle-3"></div>
    </div>
    <div class="container">
      <div class="hero-content">
        <div class="hero-badge">
          <el-icon><Star /></el-icon>
          <span>已服务 500+ 企业客户</span>
        </div>
        <h1 class="hero-title">
          把 Openclaw 卖成<span class="highlight">「数字员工岗位」</span><br>
          而不是 AI 工具
        </h1>
        <p class="hero-subtitle">
          不讲概念：今天下单，今晚就在企微/飞书里拥有你的 AI 数字员工同事。
        </p>
        <p class="hero-desc">
          别再追着 AI 更新跑了。你需要的是把每周 2 小时的搬运工作，<br class="pc-only">
          压缩成 2 分钟的自动化流程。
        </p>
        <div class="hero-actions">
          <el-button type="primary" size="large" class="cta-button" @click="goToWechat">
            <el-icon><Promotion /></el-icon>
            立即部署你的第一个数字员工
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
        <div class="hero-video-wrapper">
          <video
            ref="heroVideoRef"
            class="hero-video"
            :poster="videoPoster"
            @loadedmetadata="captureFirstFrame"
            @canplay="captureFirstFrame"
            @play="isPlaying = true"
            @pause="isPlaying = false"
            @click="togglePlay"
          >
            <source src="/video/video1.mp4" type="video/mp4" />
            您的浏览器不支持视频播放
          </video>
          <div class="hero-video-overlay" v-if="!isPlaying" @click="togglePlay">
            <div class="play-button">
              <el-icon :size="56"><VideoPlay /></el-icon>
            </div>
          </div>
        </div>
        <div class="floating-card card-1">
          <el-icon><SuccessFilled /></el-icon>
          <span>周报生成完成</span>
        </div>
        <div class="floating-card card-2">
          <el-icon><Clock /></el-icon>
          <span>耗时 2 分钟</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { Star, Promotion, VideoPlay, SuccessFilled, Clock } from '@element-plus/icons-vue'

const heroVideoRef = ref(null)
const isPlaying = ref(false)
const videoPoster = ref('')

const captureFirstFrame = () => {
  if (!heroVideoRef.value || videoPoster.value) return
  
  const video = heroVideoRef.value
  if (video.readyState >= 2) {
    const canvas = document.createElement('canvas')
    canvas.width = video.videoWidth
    canvas.height = video.videoHeight
    const ctx = canvas.getContext('2d')
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height)
    videoPoster.value = canvas.toDataURL('image/jpeg', 0.8)
  }
}

const togglePlay = () => {
  if (!heroVideoRef.value) return
  
  if (isPlaying.value) {
    heroVideoRef.value.pause()
  } else {
    heroVideoRef.value.play()
  }
}

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/kfid/your-value', '_blank')
}
</script>

<style scoped>
.hero {
  min-height: 100vh;
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
  background: radial-gradient(circle, rgba(0, 102, 255, 0.15) 0%, transparent 70%);
  top: -200px;
  right: -100px;
}

.circle-2 {
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(0, 212, 255, 0.12) 0%, transparent 70%);
  bottom: -100px;
  left: -100px;
}

.circle-3 {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(0, 102, 255, 0.1) 0%, transparent 70%);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.hero .container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
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
  background: rgba(0, 102, 255, 0.1);
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

.hero-video-wrapper {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: var(--shadow-lg);
  aspect-ratio: 16 / 9;
  background: #000;
}

.hero-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero-video-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.hero-video-overlay .play-button {
  width: 80px;
  height: 80px;
  background: var(--bg-gradient);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  transition: transform 0.3s ease;
}

.hero-video-overlay .play-button:hover {
  transform: scale(1.1);
}

.floating-card {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 8px;
  background: white;
  padding: 12px 20px;
  border-radius: 12px;
  box-shadow: var(--shadow-md);
  font-size: 14px;
  font-weight: 500;
  animation: float 3s ease-in-out infinite;
}

.floating-card .el-icon {
  font-size: 18px;
}

.card-1 {
  top: 20%;
  right: -20px;
  color: #67c23a;
}

.card-2 {
  bottom: 20%;
  left: -20px;
  color: var(--primary-color);
  animation-delay: 1.5s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
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
