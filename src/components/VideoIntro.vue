<template>
  <section class="video-intro">
    <div class="container">
      <div class="video-layout">
        <div class="video-content">
          <h2 class="video-title">看 AI 如何改变你的工作</h2>
          <p class="video-desc">3分钟了解 Openclaw 的核心能力</p>
          <div class="video-wrapper">
            <video
              ref="videoRef"
              class="promo-video"
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
            <div class="video-overlay" v-if="!isPlaying" @click="togglePlay">
              <div class="play-button">
                <el-icon :size="48"><VideoPlay /></el-icon>
              </div>
            </div>
            <button class="play-btn-mobile" v-if="!isPlaying" @click="togglePlay">
              <el-icon><VideoPlay /></el-icon>
              <span>播放视频</span>
            </button>
          </div>
        </div>
        <div class="video-stats">
          <div class="stat-item">
            <span class="stat-number">500+</span>
            <span class="stat-label">企业客户</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">99.9%</span>
            <span class="stat-label">可用率</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">10万+</span>
            <span class="stat-label">服务用户</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">7×24</span>
            <span class="stat-label">技术支持</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { VideoPlay } from '@element-plus/icons-vue'

const videoRef = ref(null)
const isPlaying = ref(false)
const videoPoster = ref('')

const captureFirstFrame = () => {
  if (!videoRef.value || videoPoster.value) return
  
  const video = videoRef.value
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
  if (!videoRef.value) return
  
  if (isPlaying.value) {
    videoRef.value.pause()
  } else {
    videoRef.value.play()
  }
}
</script>

<style scoped>
.video-intro {
  padding: 80px 0;
  background: var(--bg-white);
}

.video-layout {
  display: grid;
  grid-template-columns: 1fr 280px;
  gap: 60px;
  align-items: center;
}

.video-title {
  font-size: 32px;
  font-weight: 700;
  color: var(--primary-dark);
  margin-bottom: 12px;
}

.video-desc {
  font-size: 16px;
  color: var(--text-secondary);
  margin-bottom: 24px;
}

.video-wrapper {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: var(--shadow-lg);
  background: #000;
  aspect-ratio: 16 / 9;
}

.promo-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.video-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.play-button {
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

.play-button:hover {
  transform: scale(1.1);
}

.play-btn-mobile {
  display: none;
  position: absolute;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  padding: 12px 24px;
  background: var(--bg-gradient);
  color: white;
  border: none;
  border-radius: 24px;
  font-size: 14px;
  cursor: pointer;
  align-items: center;
  gap: 8px;
}

.video-stats {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.stat-item {
  background: linear-gradient(145deg, #FFFFFF 0%, #F3F4F6 100%);
  border: 1px solid #E5E7EB;
  border-radius: 12px;
  padding: 20px;
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 28px;
  font-weight: 700;
  color: var(--primary-color);
  margin-bottom: 4px;
}

.stat-label {
  font-size: 14px;
  color: var(--text-secondary);
}

@media (max-width: 1024px) {
  .video-layout {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .video-stats {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }

  .stat-item {
    flex: 1;
    min-width: 140px;
  }
}

@media (max-width: 768px) {
  .video-intro {
    padding: 48px 0;
  }

  .video-title {
    font-size: 24px;
  }

  .video-desc {
    font-size: 14px;
  }

  .play-btn-mobile {
    display: flex;
  }

  .play-button {
    width: 64px;
    height: 64px;
  }

  .video-stats {
    flex-direction: column;
    gap: 12px;
  }

  .stat-item {
    min-width: 100%;
  }
}
</style>
