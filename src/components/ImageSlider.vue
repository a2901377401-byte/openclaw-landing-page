<template>
  <section class="slider-section">
    <div class="container">
      <div class="slider-wrapper">
        <div
          class="image-slider"
          ref="sliderRef"
          role="region"
          aria-roledescription="carousel"
          aria-label="图片轮播"
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
          <div class="slider-container" ref="containerRef">
            <!-- 
              优化点：使用 displaySlides 计算属性，自动包含首尾克隆图
              结构变为：[克隆的最后一张, ...真实图片, 克隆的第一张]
            -->
            <div 
              class="slider-track" 
              ref="trackRef"
              :style="trackStyle"
              @transitionend="handleTransitionEnd"
            >
              <div
                class="slide"
                v-for="(img, index) in displaySlides"
                :key="index"
                role="group"
                :aria-roledescription="`第 ${getRealIndex(index) + 1} 张，共 ${images.length} 张`"
              >
                <img :src="img" :alt="`轮播图 ${getRealIndex(index) + 1}`" draggable="false" />
              </div>
            </div>
          </div>

          <button
            class="slider-arrow arrow-left"
            @click="prevSlide"
            aria-label="上一张"
          >
            <el-icon><ArrowLeft /></el-icon>
          </button>
          <button
            class="slider-arrow arrow-right"
            @click="nextSlide"
            aria-label="下一张"
          >
            <el-icon><ArrowRight /></el-icon>
          </button>

          <div class="slider-dots" role="tablist" aria-label="轮播导航">
            <button
              v-for="(_, index) in images"
              :key="index"
              class="slider-dot"
              :class="{ active: index === realIndex }"
              role="tab"
              :aria-selected="index === realIndex"
              :aria-label="`切换到第 ${index + 1} 张`"
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
import { ArrowLeft, ArrowRight } from '@element-plus/icons-vue'

const images = [
  '/Image_slider/轮播01.png',
  '/Image_slider/轮播02.png',
  '/Image_slider/轮播03.png',
  '/Image_slider/轮播04.png',
  '/Image_slider/轮播05.png',
]

// ==================== 状态定义 ====================
const realIndex = ref(0) // 用户视角的真实索引 (0 ~ length-1)
// 内部索引：0=克隆尾图, 1~N=真实图, N+1=克隆首图
const currentIndex = ref(1) 
const isAnimating = ref(false)
const AUTO_INTERVAL = 5000
const ANIMATION_DURATION = 420
let timer = null

// ==================== DOM 引用 ====================
const sliderRef = ref(null)
const containerRef = ref(null)
const trackRef = ref(null)
const containerWidth = ref(0)

// ==================== 拖拽状态 ====================
const isDragging = ref(false)
const dragOffset = ref(0)
const pointerStartX = ref(0)
const pointerStartTime = ref(0)

// 控制是否启用过渡动画（用于无缝瞬移时关闭动画）
const transitionEnabled = ref(true)

// ==================== 数据计算 ====================
// 生成包含克隆节点的列表
const displaySlides = computed(() => {
  if (images.length === 0) return []
  const first = images[0]
  const last = images[images.length - 1]
  // [克隆尾, ...真实图, 克隆头]
  return [last, ...images, first]
})

// 根据 DOM 索引获取真实索引（用于 ARIA）
const getRealIndex = (displayIndex) => {
  if (displayIndex === 0) return images.length - 1 // 克隆尾
  if (displayIndex === images.length + 1) return 0 // 克隆头
  return displayIndex - 1
}

// ==================== 容器宽度 ====================
const updateContainerWidth = () => {
  if (containerRef.value) {
    containerWidth.value = containerRef.value.offsetWidth
  }
}

// ==================== Track 样式计算 ====================
const trackStyle = computed(() => {
  // 基础偏移量：当前索引 * 宽度
  const baseOffset = -currentIndex.value * containerWidth.value
  // 加上拖拽偏移
  const totalOffset = baseOffset + dragOffset.value

  return {
    transform: `translateX(${totalOffset}px)`,
    // 只有在非拖拽、且启用过渡时才添加动画
    transition: (isDragging.value || !transitionEnabled.value)
      ? 'none'
      : `transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94)`,
    willChange: isDragging.value ? 'transform' : 'auto',
  }
})

// ==================== 边界阻尼 ====================
const getClampedOffset = (offset) => {
  if (!containerWidth.value) return 0
  
  // 判断是否处于逻辑边界
  // 内部索引 1 是第一张真实图，内部索引 images.length 是最后一张真实图
  const isAtStart = currentIndex.value === 1
  const isAtEnd = currentIndex.value === images.length

  // 循环模式下，只有拖拽方向与边界相反时才加阻尼（允许拖出边界看到克隆图，但带阻力）
  // 如果在第一张往右拖（offset > 0），或在最后一张往左拖（offset < 0）
  if ((isAtStart && offset > 0) || (isAtEnd && offset < 0)) {
    const damping = Math.max(0.15, 1 - Math.abs(offset) / (containerWidth.value * 2))
    return offset * damping
  }
  
  return offset
}

// ==================== 核心切换逻辑 ====================
const slideTo = (targetIndex) => {
  if (isAnimating.value || isDragging.value) return
  isAnimating.value = true
  transitionEnabled.value = true
  
  currentIndex.value = targetIndex
  
  // 更新真实索引（用于导航点高亮）
  // 如果滑到了克隆图（index 0 或 length+1），真实索引需要映射
  if (targetIndex === 0) {
    realIndex.value = images.length - 1
  } else if (targetIndex === images.length + 1) {
    realIndex.value = 0
  } else {
    realIndex.value = targetIndex - 1
  }

  resetTimer()
}

const nextSlide = () => slideTo(currentIndex.value + 1)
const prevSlide = () => slideTo(currentIndex.value - 1)

// 点击导航点：真实索引转为内部索引
const goToSlide = (index) => slideTo(index + 1)

// ==================== 无缝瞬移处理 ====================
// 动画结束时检查是否需要瞬移
const handleTransitionEnd = () => {
  isAnimating.value = false
  
  // 如果在克隆图上，瞬间跳回对应的真实图
  if (currentIndex.value === 0) {
    fixPosition(images.length) // 跳到最后一张真实图
  } else if (currentIndex.value === images.length + 1) {
    fixPosition(1) // 跳到第一张真实图
  }
}

const fixPosition = (targetIndex) => {
  transitionEnabled.value = false
  
  // 强制浏览器在下一帧再执行位置变更
  requestAnimationFrame(() => {
    currentIndex.value = targetIndex
    
    // 再下一帧恢复动画
    requestAnimationFrame(() => {
      transitionEnabled.value = true
    })
  })
}


// ==================== 自动播放 ====================
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

// ==================== 拖拽事件处理 ====================
const getPointerX = (e) => (e.touches ? e.touches[0].clientX : e.clientX)

const handlePointerDown = (e) => {
  if (isAnimating.value) return
  isDragging.value = true
  dragOffset.value = 0
  pointerStartX.value = getPointerX(e)
  pointerStartTime.value = Date.now()
  stopAutoPlay()
  transitionEnabled.value = true // 确保拖拽开始时动画状态正常

  if (!e.touches) {
    document.addEventListener('mousemove', handlePointerMove)
    document.addEventListener('mouseup', handlePointerUp)
  }
}

const handlePointerMove = (e) => {
  if (!isDragging.value) return
  const currentX = e.touches ? e.touches[0].clientX : e.clientX
  const diff = currentX - pointerStartX.value
  
  // 应用阻尼
  dragOffset.value = getClampedOffset(diff)
}

const handlePointerUp = () => {
  if (!isDragging.value) return

  const duration = Date.now() - pointerStartTime.value
  const distance = dragOffset.value
  const velocity = Math.abs(distance) / duration

  // 判定切换阈值
  const shouldSlide = Math.abs(distance) > 50 || velocity > 0.3

  isDragging.value = false
  dragOffset.value = 0 // 重置拖拽偏移，依靠 CSS 动画回弹或切换

  if (shouldSlide) {
    distance > 0 ? prevSlide() : nextSlide()
  } else {
    // 如果未触发切换，重置定时器即可，位置会自动回弹
    resetTimer()
  }

  document.removeEventListener('mousemove', handlePointerMove)
  document.removeEventListener('mouseup', handlePointerUp)
}

// ==================== 键盘导航 ====================
const handleKeydown = (e) => {
  if (!sliderRef.value?.contains(document.activeElement)) return
  if (e.key === 'ArrowLeft') { e.preventDefault(); prevSlide() }
  if (e.key === 'ArrowRight') { e.preventDefault(); nextSlide() }
}

// ==================== 生命周期 ====================
onMounted(() => {
  updateContainerWidth()
  window.addEventListener('resize', updateContainerWidth)
  window.addEventListener('keydown', handleKeydown)
  startAutoPlay()
})

onUnmounted(() => {
  stopAutoPlay()
  window.removeEventListener('resize', updateContainerWidth)
  window.removeEventListener('keydown', handleKeydown)
  document.removeEventListener('mousemove', handlePointerMove)
  document.removeEventListener('mouseup', handlePointerUp)
})
</script>

<style scoped>
/* 样式保持不变，稍微增加了对隐藏溢出的控制 */
.slider-section {
  padding: 60px 0;
  background: #fff;
}

.slider-wrapper {
  display: flex;
  justify-content: center;
}

.image-slider {
  width: 100%;
  max-width: 800px;
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: var(--shadow-lg);
  outline: none;
}

.image-slider:focus-visible {
  box-shadow: var(--shadow-lg), 0 0 0 3px rgba(64, 158, 255, 0.5);
}

.slider-container {
  width: 100%;
  overflow: hidden;
  border-radius: 20px;
  cursor: grab;
  /* 优化：防止部分浏览器下的滚动条出现 */
  -ms-overflow-style: none; 
  scrollbar-width: none;
}

.slider-container:active {
  cursor: grabbing;
}

.slider-track {
  display: flex;
  height: 100%;
}

.slide {
  width: 100%;
  flex-shrink: 0;
  user-select: none;
  -webkit-user-drag: none;
}

.slide img {
  width: 100%;
  height: auto;
  display: block;
  pointer-events: none;
  -webkit-user-drag: none;
}

/* ---- 箭头 ---- */
.slider-arrow {
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

.image-slider:hover .slider-arrow,
.image-slider:focus-visible .slider-arrow {
  opacity: 1;
}

.slider-arrow:hover {
  background: white;
  transform: translateY(-50%) scale(1.1);
  box-shadow: var(--shadow-lg);
}

.slider-arrow:active {
  transform: translateY(-50%) scale(0.95);
}

.arrow-left {
  left: 16px;
}

.arrow-right {
  right: 16px;
}

/* ---- 指示点 ---- */
.slider-dots {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 10;
}

.slider-dot {
  width: 10px;
  height: 10px;
  background: rgba(255, 255, 255, 0.4);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.slider-dot:hover {
  background: rgba(255, 255, 255, 0.7);
}

.slider-dot.active {
  background: white;
  width: 28px;
  border-radius: 5px;
}

/* ---- 移动端适配 ---- */
@media (max-width: 768px) {
  .slider-section {
    padding: 40px 0;
  }

  .image-slider {
    max-width: 100%;
    border-radius: 12px;
  }

  .slider-container {
    border-radius: 12px;
  }

  .slider-arrow {
    width: 40px;
    height: 40px;
    opacity: 0.8;
    font-size: 16px;
  }

  .image-slider:hover .slider-arrow {
    opacity: 0.8;
  }

  .slider-arrow:active {
    transform: translateY(-50%) scale(0.9);
  }

  .arrow-left {
    left: 8px;
  }

  .arrow-right {
    right: 8px;
  }

  .slider-dots {
    bottom: 12px;
  }

  .slider-dot {
    width: 8px;
    height: 8px;
  }

  .slider-dot.active {
    width: 22px;
  }
}
</style>
