<template>
  <section class="social-proof" id="cases">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">效率提升，看得见</h2>
        <p class="section-subtitle">真实数据，不玩虚的</p>
      </div>
      <div class="carousel-wrapper">
        <div
          class="proof-carousel"
          ref="carouselRef"
          role="region"
          aria-roledescription="carousel"
          aria-label="案例轮播"
          tabindex="0"
          @mouseenter="stopAutoPlay"
          @mouseleave="resumeAutoPlay"
          @focus="stopAutoPlay"
          @blur="resumeAutoPlay"
          @touchstart="handlePointerDown"
          @touchmove.prevent="handlePointerMove"
          @touchend="handlePointerUp"
          @touchcancel="handlePointerUp"
          @mousedown.prevent="handlePointerDown"
        >
          <div class="carousel-viewport" ref="viewportRef">
            <div 
              class="carousel-track" 
              ref="trackRef"
              :style="trackStyle"
              @transitionend="handleTransitionEnd"
            >
              <div
                class="proof-card"
                v-for="(proof, index) in displayProofs"
                :key="index"
                role="group"
                :aria-roledescription="`第 ${getRealIndex(index) + 1} 个案例，共 ${proofs.length} 个`"
                :style="{ backgroundImage: proof.bgImage ? `url(${proof.bgImage})` : 'none' }"
              >
                <div class="proof-content">
                  <div class="proof-card-content">
                    <div class="proof-header">
                      <h3 class="proof-title">{{ proof.title }}</h3>
                      <p class="proof-desc">{{ proof.desc }}</p>
                    </div>
                    <div class="proof-body">
                      <div class="proof-visual">
                        <div class="comparison">
                          <div class="before">
                            <span class="label">Before</span>
                            <span class="time">{{ proof.before }}</span>
                          </div>
                          <div class="arrow">
                            <el-icon><Right /></el-icon>
                          </div>
                          <div class="after">
                            <span class="label">After</span>
                            <span class="time highlight">{{ proof.after }}</span>
                          </div>
                        </div>
                      </div>
                      <div class="proof-stat">
                        <span class="stat-value">{{ proof.improvement }}</span>
                        <span class="stat-label">效率提升</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <button
            class="carousel-arrow arrow-left"
            @click="prevSlide"
            aria-label="上一个案例"
          >
            <el-icon><ArrowLeft /></el-icon>
          </button>

          <button
            class="carousel-arrow arrow-right"
            @click="nextSlide"
            aria-label="下一个案例"
          >
            <el-icon><ArrowRight /></el-icon>
          </button>

          <div class="carousel-dots" role="tablist" aria-label="案例导航">
            <button
              v-for="(_, index) in proofs"
              :key="index"
              class="carousel-dot"
              :class="{ active: index === realIndex }"
              role="tab"
              :aria-selected="index === realIndex"
              :aria-label="`切换到第 ${index + 1} 个案例`"
              @click.stop="goToSlide(index)"
            ></button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'
import { Document, Timer, Cpu, Right, ArrowLeft, ArrowRight } from '@element-plus/icons-vue'

const proofs = [
  {
    icon: Document,
    title: '自媒体案例',
    desc: '单篇笔记耗时从 4.5 小时降至 23 分钟',
    before: '4.5 小时',
    after: '23 分钟',
    improvement: '92%',
    bgImage: '自媒体案例.jpg'
  },
  {
    icon: Timer,
    title: '职场案例',
    desc: '周报整理从 120 分钟降至 2 分钟',
    before: '120 分钟',
    after: '2 分钟',
    improvement: '98%',
    bgImage: '职场案例.jpg'
  },
  {
    icon: Cpu,
    title: '企业案例',
    desc: 'IT 运维响应从 30 分钟降至 3 分钟',
    before: '30 分钟',
    after: '3 分钟',
    improvement: '90%',
    bgImage: '企业案例.jpg'
  }
]

const realIndex = ref(0)
const currentIndex = ref(1)
const isAnimating = ref(false)
const AUTO_INTERVAL = 5000
const ANIMATION_DURATION = 420
let timer = null

const carouselRef = ref(null)
const viewportRef = ref(null)
const trackRef = ref(null)
const viewportWidth = ref(0)

const isDragging = ref(false)
const dragOffset = ref(0)
const pointerStartX = ref(0)
const pointerStartTime = ref(0)

const transitionEnabled = ref(true)

const displayProofs = computed(() => {
  if (proofs.length === 0) return []
  const first = proofs[0]
  const last = proofs[proofs.length - 1]
  return [last, ...proofs, first]
})

const getRealIndex = (displayIndex) => {
  if (displayIndex === 0) return proofs.length - 1
  if (displayIndex === proofs.length + 1) return 0
  return displayIndex - 1
}

const updateViewportWidth = () => {
  if (viewportRef.value) {
    viewportWidth.value = viewportRef.value.offsetWidth
  }
}

const trackStyle = computed(() => {
  const baseOffset = -currentIndex.value * viewportWidth.value
  const totalOffset = baseOffset + dragOffset.value

  return {
    transform: `translateX(${totalOffset}px)`,
    transition: (isDragging.value || !transitionEnabled.value)
      ? 'none'
      : `transform ${ANIMATION_DURATION}ms cubic-bezier(0.25, 0.46, 0.45, 0.94)`,
    willChange: isDragging.value ? 'transform' : 'auto',
  }
})

const getClampedOffset = (offset) => {
  if (!viewportWidth.value) return 0
  
  const isAtStart = currentIndex.value === 1
  const isAtEnd = currentIndex.value === proofs.length

  if ((isAtStart && offset > 0) || (isAtEnd && offset < 0)) {
    const damping = Math.max(0.15, 1 - Math.abs(offset) / (viewportWidth.value * 2))
    return offset * damping
  }
  
  return offset
}

const slideTo = (targetIndex) => {
  if (isAnimating.value || isDragging.value) return
  isAnimating.value = true
  transitionEnabled.value = true
  
  currentIndex.value = targetIndex
  
  if (targetIndex === 0) {
    realIndex.value = proofs.length - 1
  } else if (targetIndex === proofs.length + 1) {
    realIndex.value = 0
  } else {
    realIndex.value = targetIndex - 1
  }

  resetTimer()
}

const nextSlide = () => slideTo(currentIndex.value + 1)
const prevSlide = () => slideTo(currentIndex.value - 1)
const goToSlide = (index) => slideTo(index + 1)

const handleTransitionEnd = () => {
  isAnimating.value = false
  
  if (currentIndex.value === 0) {
    fixPosition(proofs.length)
  } else if (currentIndex.value === proofs.length + 1) {
    fixPosition(1)
  }
}

const fixPosition = (targetIndex) => {
  transitionEnabled.value = false
  
  requestAnimationFrame(() => {
    currentIndex.value = targetIndex
    
    requestAnimationFrame(() => {
      transitionEnabled.value = true
    })
  })
}

const startAutoPlay = () => {
  stopAutoPlay()
  timer = setInterval(nextSlide, AUTO_INTERVAL)
}

const stopAutoPlay = () => {
  if (timer) {
    clearInterval(timer)
    timer = null
  }
}

const resetTimer = () => {
  stopAutoPlay()
  startAutoPlay()
}

const resumeAutoPlay = () => {
  if (!isDragging.value) startAutoPlay()
}

const getPointerX = (e) => (e.touches ? e.touches[0].clientX : e.clientX)

const handlePointerDown = (e) => {
  if (isAnimating.value) return
  isDragging.value = true
  dragOffset.value = 0
  pointerStartX.value = getPointerX(e)
  pointerStartTime.value = Date.now()
  stopAutoPlay()
  transitionEnabled.value = true

  if (!e.touches) {
    document.addEventListener('mousemove', handlePointerMove)
    document.addEventListener('mouseup', handlePointerUp)
  }
}

const handlePointerMove = (e) => {
  if (!isDragging.value) return
  const currentX = e.touches ? e.touches[0].clientX : e.clientX
  const diff = currentX - pointerStartX.value
  
  dragOffset.value = getClampedOffset(diff)
}

const handlePointerUp = () => {
  if (!isDragging.value) return

  const duration = Date.now() - pointerStartTime.value
  const distance = dragOffset.value
  const velocity = Math.abs(distance) / duration

  const shouldSlide = Math.abs(distance) > 50 || velocity > 0.3

  isDragging.value = false
  dragOffset.value = 0

  if (shouldSlide) {
    distance > 0 ? prevSlide() : nextSlide()
  } else {
    resetTimer()
  }

  document.removeEventListener('mousemove', handlePointerMove)
  document.removeEventListener('mouseup', handlePointerUp)
}

const handleKeydown = (e) => {
  if (!carouselRef.value?.contains(document.activeElement)) return
  if (e.key === 'ArrowLeft') { e.preventDefault(); prevSlide() }
  if (e.key === 'ArrowRight') { e.preventDefault(); nextSlide() }
}

onMounted(() => {
  updateViewportWidth()
  window.addEventListener('resize', updateViewportWidth)
  window.addEventListener('keydown', handleKeydown)
  startAutoPlay()
})

onUnmounted(() => {
  stopAutoPlay()
  window.removeEventListener('resize', updateViewportWidth)
  window.removeEventListener('keydown', handleKeydown)
  document.removeEventListener('mousemove', handlePointerMove)
  document.removeEventListener('mouseup', handlePointerUp)
})
</script>

<style scoped>
.social-proof {
  padding: 80px 0;
}

.section-header {
  text-align: center;
  margin-bottom: 48px;
}

.section-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 12px;
}

.section-subtitle {
  font-size: 18px;
  color: var(--text-secondary);
}

.carousel-wrapper {
  position: relative;
  max-width: 900px;
  margin: 0 auto;
}

.proof-carousel {
  width: 100%;
  position: relative;
  border-radius: 20px;
  overflow: visible;
  box-shadow: var(--shadow-lg);
  outline: none;
}

.proof-carousel:focus-visible {
  box-shadow: var(--shadow-lg), 0 0 0 3px rgba(168, 85, 247, 0.5);
}

.carousel-viewport {
  width: 100%;
  overflow: hidden;
  border-radius: 20px;
  cursor: grab;
  background: white;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.carousel-viewport:active {
  cursor: grabbing;
}

.carousel-track {
  display: flex;
  height: 100%;
}

.proof-card {
  min-width: 100%;
  flex-shrink: 0;
  user-select: none;
  -webkit-user-drag: none;
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
}

.proof-content {
  position: relative;
  z-index: 2;
  padding: 60px 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 480px;
}

.proof-card-content {
  width: 60%;
  max-width: 720px;
}

.proof-header {
  margin-bottom: 32px;
  text-align: center;
}

.proof-title {
  font-size: 26px;
  font-weight: 700;
  margin: 0 0 10px 0;
  color: #7c3aed;
}

.proof-desc {
  font-size: 15px;
  color: #a78bfa;
  line-height: 1.6;
  margin: 0;
}

.proof-body {
  display: flex;
  align-items: center;
  gap: 36px;
}

.proof-visual {
  flex: 1;
  background: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(8px);
  padding: 28px 24px;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.5);
}

.comparison {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 24px;
}

.before,
.after {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.before .label,
.after .label {
  font-size: 12px;
  color: #9ca3af;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 500;
}

.before .time {
  font-size: 24px;
  font-weight: 700;
  color: #dc2626;
}

.after .time {
  font-size: 28px;
  font-weight: 800;
}

.after .time.highlight {
  background: linear-gradient(135deg, #7c3aed 0%, #a78bfa 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.arrow {
  color: #7c3aed;
  font-size: 28px;
  font-weight: 600;
}

.proof-stat {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(135deg, rgba(124, 58, 237, 0.12) 0%, rgba(167, 139, 250, 0.12) 100%);
  backdrop-filter: blur(8px);
  padding: 18px 32px;
  border-radius: 16px;
  border: 1px solid rgba(124, 58, 237, 0.15);
}

.stat-value {
  font-size: 32px;
  font-weight: 800;
  color: #6d28d9;
}

.stat-label {
  font-size: 13px;
  color: #7c3aed;
  margin-top: 4px;
  font-weight: 500;
}

/* ---- 箭头按钮 ---- */
.carousel-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  background: rgba(255, 255, 255, 0.85);
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  opacity: 0;
  transition: all 0.3s ease;
  box-shadow: var(--shadow-md);
  z-index: 10;
  font-size: 18px;
  color: #333;
}

.proof-carousel:hover .carousel-arrow,
.proof-carousel:focus-visible .carousel-arrow {
  opacity: 1;
}

.carousel-arrow:hover {
  background: white;
  transform: translateY(-50%) scale(1.1);
  box-shadow: var(--shadow-lg);
}

.carousel-arrow:active {
  transform: translateY(-50%) scale(0.95);
}

.arrow-left {
  left: 16px;
}

.arrow-right {
  right: 16px;
}

/* ---- 指示点 ---- */
.carousel-dots {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 10;
}

.carousel-dot {
  width: 10px;
  height: 10px;
  background: rgba(168, 85, 247, 0.3);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.carousel-dot:hover {
  background: rgba(168, 85, 247, 0.6);
}

.carousel-dot.active {
  background: var(--primary-color);
  width: 28px;
  border-radius: 5px;
}

@media (max-width: 1024px) {
  .carousel-wrapper {
    max-width: 800px;
  }
  
  .proof-content {
    padding: 48px 36px;
    min-height: 440px;
  }
  
  .proof-card-content {
    padding: 36px 32px;
  }

  .proof-body {
    gap: 28px;
  }
  
  .proof-visual {
    padding: 24px 20px;
  }
  
  .comparison {
    gap: 20px;
  }
  
  .before .time {
    font-size: 22px;
  }
  
  .after .time {
    font-size: 26px;
  }

  .proof-title {
    font-size: 24px;
  }
}

@media (max-width: 768px) {
  .social-proof {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }

  .proof-carousel {
    border-radius: 16px;
  }

  .carousel-viewport {
    border-radius: 16px;
  }

  .carousel-arrow {
    width: 40px;
    height: 40px;
    opacity: 0.85;
    font-size: 16px;
  }

  .proof-carousel:hover .carousel-arrow {
    opacity: 0.85;
  }

  .carousel-arrow:active {
    transform: translateY(-50%) scale(0.9);
  }

  .arrow-left {
    left: 8px;
  }

  .arrow-right {
    right: 8px;
  }

  .proof-content {
    padding: 40px 24px;
    min-height: auto;
  }

  .proof-card-content {
    padding: 32px 24px;
  }

  .proof-header {
    margin-bottom: 28px;
  }

  .proof-body {
    flex-direction: column;
    gap: 24px;
  }

  .proof-visual {
    padding: 24px 18px;
  }

  .comparison {
    flex-direction: column;
    gap: 16px;
  }

  .arrow {
    transform: rotate(90deg);
  }

  .before .time {
    font-size: 22px;
  }

  .after .time {
    font-size: 26px;
  }

  .proof-title {
    font-size: 22px;
  }

  .proof-desc {
    font-size: 14px;
  }

  .proof-stat {
    padding: 16px 28px;
  }

  .stat-value {
    font-size: 28px;
  }

  .carousel-dots {
    bottom: 16px;
  }

  .carousel-dot {
    width: 8px;
    height: 8px;
  }

  .carousel-dot.active {
    width: 22px;
  }
}
</style>
