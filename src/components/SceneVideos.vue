<template>
  <section class="scene-videos">
    <div class="scene-header">
      <h2 class="scene-title">看不同人群如何使用 Openclaw</h2>
    </div>
    <div class="container">
      <div class="scene-nav">
        <button
          v-for="category in categories"
          :key="category.id"
          class="nav-btn"
          :class="{ active: currentCategory === category.id }"
          @click="switchCategory(category.id)"
        >
          <el-icon :size="24"><component :is="category.icon" /></el-icon>
          <span class="nav-label">{{ category.label }}</span>
        </button>
      </div>
      <div class="scene-video-wrapper"
        @touchstart="handleTouchStart"
        @touchmove.prevent="handleTouchMove"
        @touchend="handleTouchEnd"
      >
        <video
          ref="sceneVideoRef"
          class="scene-video"
          :key="currentCategory"
          :poster="currentPoster"
          @loadedmetadata="captureFirstFrame"
          @canplay="captureFirstFrame"
          @play="isPlaying = true"
          @pause="isPlaying = false"
          @click="togglePlay"
        >
          <source :src="currentVideo" type="video/mp4" />
          您的浏览器不支持视频播放
        </video>
        <div class="scene-overlay" v-if="!isPlaying" @click="togglePlay">
          <div class="play-button">
            <el-icon :size="56"><VideoPlay /></el-icon>
          </div>
        </div>
        <button class="arrow-left" @click.stop="prevCategory">
          <el-icon><ArrowLeft /></el-icon>
        </button>
        <button class="arrow-right" @click.stop="nextCategory">
          <el-icon><ArrowRight /></el-icon>
        </button>
      </div>
      <p class="scene-desc">{{ currentDesc }}</p>
      <div class="scene-dots">
        <span
          v-for="category in categories"
          :key="category.id"
          :class="{ active: currentCategory === category.id }"
          @click="switchCategory(category.id)"
        ></span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'
import { VideoPlay, ArrowLeft, ArrowRight, Reading, Briefcase, OfficeBuilding, Promotion } from '@element-plus/icons-vue'

const categories = [
  {
    id: 'student',
    label: '学生',
    icon: Reading,
    video: '/video/video1.mp4',
    desc: 'Openclaw 帮助学生节省 50% 的学习时间，让学习更高效'
  },
  {
    id: 'office',
    label: '白领',
    icon: Briefcase,
    video: '/video/video1.mp4',
    desc: 'Openclaw 让白领的工作效率提升 300%，每天多出 2 小时'
  },
  {
    id: 'enterprise',
    label: '企业',
    icon: OfficeBuilding,
    video: '/video/video1.mp4',
    desc: 'Openclaw 帮助企业降低 30% 的运营成本，提升客户满意度'
  },
  {
    id: 'social',
    label: '社媒',
    icon: Promotion,
    video: '/video/video1.mp4',
    desc: 'Openclaw 让社媒运营效率提升 200%，粉丝增长更快'
  }
]

const currentCategory = ref('student')
const sceneVideoRef = ref(null)
const isPlaying = ref(false)
const currentPoster = ref('')
let touchStartX = 0
let touchEndX = 0

const currentVideo = computed(() => {
  const cat = categories.find(c => c.id === currentCategory.value)
  return cat ? cat.video : ''
})

const currentDesc = computed(() => {
  const cat = categories.find(c => c.id === currentCategory.value)
  return cat ? cat.desc : ''
})

const currentIndex = computed(() => {
  return categories.findIndex(c => c.id === currentCategory.value)
})

const switchCategory = (id) => {
  if (sceneVideoRef.value) {
    sceneVideoRef.value.pause()
    isPlaying.value = false
  }
  currentCategory.value = id
  currentPoster.value = ''
}

const prevCategory = () => {
  const idx = (currentIndex.value - 1 + categories.length) % categories.length
  switchCategory(categories[idx].id)
}

const nextCategory = () => {
  const idx = (currentIndex.value + 1) % categories.length
  switchCategory(categories[idx].id)
}

const captureFirstFrame = () => {
  if (!sceneVideoRef.value || currentPoster.value) return
  
  const video = sceneVideoRef.value
  if (video.readyState >= 2) {
    const canvas = document.createElement('canvas')
    canvas.width = video.videoWidth
    canvas.height = video.videoHeight
    const ctx = canvas.getContext('2d')
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height)
    currentPoster.value = canvas.toDataURL('image/jpeg', 0.8)
  }
}

const togglePlay = () => {
  if (!sceneVideoRef.value) return
  
  if (isPlaying.value) {
    sceneVideoRef.value.pause()
  } else {
    sceneVideoRef.value.play()
  }
}

const handleTouchStart = (e) => {
  touchStartX = e.touches[0].clientX
}

const handleTouchMove = (e) => {
  touchEndX = e.touches[0].clientX
  e.preventDefault()
}

const handleTouchEnd = () => {
  const diff = touchStartX - touchEndX
  if (Math.abs(diff) > 50) {
    if (diff > 0) {
      nextCategory()
    } else {
      prevCategory()
    }
  }
}
</script>

<style scoped>
.scene-videos {
  background: var(--bg-white);
}

.scene-header {
  background: var(--bg-gradient);
  padding: 24px 0;
  text-align: center;
}

.scene-title {
  color: white;
  font-size: 24px;
  font-weight: 600;
}

.scene-nav {
  display: flex;
  justify-content: center;
  gap: 32px;
  padding: 32px 0;
}

.nav-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  transition: all 0.3s ease;
}

.nav-btn .el-icon {
  width: 56px;
  height: 56px;
  background: linear-gradient(145deg, #F3F4F6 0%, #E5E7EB 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-secondary);
  transition: all 0.3s ease;
}

.nav-btn.active .el-icon,
.nav-btn:hover .el-icon {
  background: var(--bg-gradient);
  color: white;
}

.nav-label {
  font-size: 14px;
  color: var(--text-secondary);
  font-weight: 500;
}

.nav-btn.active .nav-label {
  color: var(--primary-color);
  font-weight: 600;
}

.scene-video-wrapper {
  position: relative;
  max-width: 900px;
  margin: 0 auto;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: var(--shadow-lg);
  aspect-ratio: 16 / 9;
  background: #000;
}

.scene-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.scene-overlay {
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

.arrow-left,
.arrow-right {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  background: rgba(255, 255, 255, 0.9);
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
  color: var(--text-secondary);
}

.arrow-left {
  left: 16px;
}

.arrow-right {
  right: 16px;
}

.arrow-left:hover,
.arrow-right:hover {
  background: white;
  transform: translateY(-50%) scale(1.1);
}

.scene-desc {
  text-align: center;
  font-size: 16px;
  color: var(--text-secondary);
  max-width: 600px;
  margin: 24px auto;
  line-height: 1.8;
}

.scene-dots {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 0px;
}

.scene-dots span {
  width: 10px;
  height: 10px;
  background: #E5E7EB;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.scene-dots span.active {
  background: var(--primary-color);
  width: 28px;
  border-radius: 5px;
}

@media (max-width: 768px) {
  .scene-title {
    font-size: 18px;
  }

  .scene-nav {
    gap: 16px;
    padding: 24px 0;
  }

  .nav-btn .el-icon {
    width: 48px;
    height: 48px;
  }

  .nav-label {
    font-size: 12px;
  }

  .scene-video-wrapper {
    border-radius: 12px;
  }

  .play-button {
    width: 64px;
    height: 64px;
  }

  .arrow-left,
  .arrow-right {
    display: none;
  }

  .scene-desc {
    font-size: 14px;
    padding: 0 16px;
  }

  .scene-dots {
    margin-bottom: 48px;
  }
}
</style>
