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
              >
                <div class="proof-content">
                  <!-- 左侧图片区域 -->
                  <div class="proof-image-section">
                    <img :src="'/' + proof.bgImage" :alt="proof.title" class="proof-image" />
                  </div>
                  
                  <!-- 右侧文字区域 -->
                  <div class="proof-info-section">
                    <div class="proof-header">
                      <h3 class="proof-title">{{ proof.title }}</h3>
                      <p class="proof-desc">{{ proof.desc }}</p>
                    </div>
                    <div class="proof-features">
                      <div class="feature-item" v-for="(feature, fIndex) in proof.features" :key="fIndex">
                        <el-icon class="feature-icon"><Check /></el-icon>
                        <span>{{ feature }}</span>
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
import { Document, Timer, Cpu, Check, ArrowLeft, ArrowRight } from '@element-plus/icons-vue'

const proofs = [
  {
    icon: Cpu,
    title: 'AI驱动的工厂合规智检系统',
    desc: '把专业安全员放进每个工人的口袋：只需拍照，即可完成智能隐患排查与报告生成',
    features: [
      '🚀 全员合规：无需专业背景，一线员工即可精准识别安全隐患',
      '⏱️ 即时预警：拍下即分析，AI实时提醒并引导解决问题',
      '📜 一键报告：检查完成即生成合规报告，节省90%以上的时间',
      '💼 成本优化：大幅减少人力和时间成本，显著提升检查频率'
    ],
    bgImage: '安全助手.jpg'
  },
  {
    icon: Cpu,
    title: 'AI 加油站智慧安全排查系统',
    desc: 'AI 识图监测，智能推送隐患，助力安全合规，省时省力',
    features: [
      '🛡️ 24/7 实时监测',
      '🔍 智能隐患识别',
      '📱 即时手机推送',
      '✅ 减少人工巡检'
    ],
    bgImage: '消防助手.jpg'
  },
  {
    icon: Document,
    title: '自媒体案例',
    desc: '单篇笔记耗时从 4.5 小时降至 23 分钟',
    features: [
      '全平台内容一键生成',
      '智能选题与热点追踪',
      '自动排版与发布',
      '效率提升 92%'
    ],
    bgImage: '自媒体案例.jpg'
  },
  {
    icon: Timer,
    title: '职场案例',
    desc: '周报整理从 120 分钟降至 2 分钟',
    features: [
      '智能会议纪要生成',
      '自动工作总结',
      '数据可视化报表',
      '效率提升 98%'
    ],
    bgImage: '职场案例.jpg'
  },
  {
    icon: Cpu,
    title: '企业案例',
    desc: 'IT 运维响应从 30 分钟降至 3 分钟',
    features: [
      '智能故障诊断',
      '自动化运维流程',
      '实时监控预警',
      '效率提升 90%'
    ],
    bgImage: '企业案例.jpg'
  },
  
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
  background: #f9fafb;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 48px;
}

.section-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 12px;
  color: #1a1a1a;
}

.section-subtitle {
  font-size: 18px;
  color: #6b7280;
}

.carousel-wrapper {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
}

.proof-carousel {
  width: 100%;
  position: relative;
  border-radius: 24px;
  overflow: visible;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  outline: none;
}

.proof-carousel:focus-visible {
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04), 0 0 0 3px rgba(168, 85, 247, 0.5);
}

.carousel-viewport {
  width: 100%;
  overflow: hidden;
  border-radius: 24px;
  cursor: grab;
  background: white;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.carousel-viewport::-webkit-scrollbar {
  display: none;
}

.carousel-viewport:active {
  cursor: grabbing;
}

.carousel-track {
  display: flex;
  height: 100%;
}

.proof-card {
  max-width: 100%;
  flex-shrink: 0;
  user-select: none;
  -webkit-user-drag: none;
  position: relative;
  background: white;
  overflow: hidden;
  border-radius: 24px;
}

.proof-content {
  display: grid;
  grid-template-columns: 2fr 1fr; /* 左右各占一半 */
  min-height: 560px;
  width: 100%;
}

.proof-image-section {
  position: relative;
  overflow: hidden;
  background: #f8f9fa;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
}

.proof-image {
  width: auto;
  height: auto;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  object-position: center;
}

.proof-info-section {
  padding: 56px 22px;
  height: 580px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: white;
  border-left: 1px solid #e5e7eb;
}

.proof-header {
  margin-bottom: 32px;
}

.proof-title {
  font-size: 26px;
  font-weight: 700;
  margin: 0 0 14px 0;
  color: #1a1a1a;
  line-height: 1.3;
}

.proof-desc {
  font-size: 16px;
  color: #666;
  line-height: 1.7;
  margin: 0;
}

.proof-features {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.feature-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 15px;
  background: #f9fafb;
  border-radius: 10px;
  border: 1px solid #e8e8e8;
  transition: all 0.3s ease;
}

.feature-item:hover {
  border-color: #a855f7;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.1);
  transform: translateX(4px);
}

.feature-icon {
  color: #a855f7;
  font-size: 18px;
  flex-shrink: 0;
  margin-top: 2px;
}

.feature-item span {
  font-size: 14px;
  color: #333;
  line-height: 1.6;
}

/* 箭头按钮 */
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
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  z-index: 10;
  font-size: 18px;
  color: #333;
}

.proof-carousel:hover .carousel-arrow,
.proof-carousel:focus-within .carousel-arrow {
  opacity: 1;
}

.carousel-arrow:hover {
  background: white;
  transform: translateY(-50%) scale(1.1);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
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

/* 指示点 */
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
  background: #a855f7;
  width: 28px;
  border-radius: 5px;
}

/* 响应式设计 */
@media (max-width: 1024px) {
  .container {
    padding: 0 16px;
  }
  
  .proof-content {
    min-height: 500px;
    grid-template-columns: 1fr 1fr;
  }
  
  .proof-info-section {
    padding: 40px 32px;
  }

  .proof-title {
    font-size: 22px;
  }
  
  .proof-desc {
    font-size: 14px;
  }
  
  .feature-item {
    padding: 13px;
  }
  
  .feature-item span {
    font-size: 13px;
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

  .carousel-wrapper {
    padding: 0 16px;
  }

  .proof-carousel {
    border-radius: 20px;
  }

  .carousel-viewport {
    border-radius: 20px;
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
    grid-template-columns: 1fr; /* 移动端垂直布局 */
    min-height: auto;
  }

  .proof-image-section {
    height: 240px;
  }

  .proof-info-section {
    padding: 32px 24px;
    border-left: none;
    border-top: 1px solid #e5e7eb;
  }

  .proof-header {
    margin-bottom: 24px;
  }

  .proof-title {
    font-size: 20px;
  }

  .proof-desc {
    font-size: 14px;
  }

  .proof-features {
    gap: 12px;
  }

  .feature-item {
    padding: 13px;
  }

  .feature-icon {
    font-size: 17px;
  }

  .feature-item span {
    font-size: 13px;
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
