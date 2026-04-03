<template>
  <section class="social-proof" id="cases">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">效率提升，看得见</h2>
        <p class="section-subtitle">真实数据，不玩虚的</p>
      </div>
      <div class="proof-slider-wrapper">
        <div class="proof-slider">
          <div class="proof-card" v-for="(proof, index) in proofs" :key="index">
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
            <div class="proof-content">
              <div class="proof-icon">
                <el-icon :size="24"><component :is="proof.icon" /></el-icon>
              </div>
              <h3 class="proof-title">{{ proof.title }}</h3>
              <p class="proof-desc">{{ proof.desc }}</p>
              <div class="proof-stat">
                <span class="stat-value">{{ proof.improvement }}</span>
                <span class="stat-label">效率提升</span>
              </div>
            </div>
          </div>
        </div>
        <div class="slider-dots">
          <span 
            v-for="(_, index) in proofs" 
            :key="index"
            :class="{ active: currentSlide === index }"
            @click="goToSlide(index)"
          ></span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Document, Timer, Cpu, Right } from '@element-plus/icons-vue'

const proofs = [
  {
    icon: Document,
    title: '自媒体案例',
    desc: '单篇笔记耗时从 4.5 小时降至 23 分钟',
    before: '4.5 小时',
    after: '23 分钟',
    improvement: '92%'
  },
  {
    icon: Timer,
    title: '职场案例',
    desc: '周报整理从 120 分钟降至 2 分钟',
    before: '120 分钟',
    after: '2 分钟',
    improvement: '98%'
  },
  {
    icon: Cpu,
    title: '企业案例',
    desc: 'IT 运维响应从 30 分钟降至 3 分钟',
    before: '30 分钟',
    after: '3 分钟',
    improvement: '90%'
  }
]

const currentSlide = ref(0)

const goToSlide = (index) => {
  currentSlide.value = index
  scrollToSlide(index)
}

const scrollToSlide = (index) => {
  const slider = document.querySelector('.proof-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.proof-card')?.offsetWidth || 360
    const gap = window.innerWidth <= 900 ? 16 : 24
    slider.scrollTo({
      left: index * (cardWidth + gap),
      behavior: 'auto'
    })
  }
}

const handleScroll = () => {
  const slider = document.querySelector('.proof-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.proof-card')?.offsetWidth || 360
    const gap = window.innerWidth <= 900 ? 16 : 24
    const scrollLeft = slider.scrollLeft
    const newIndex = Math.round(scrollLeft / (cardWidth + gap))
    if (newIndex !== currentSlide.value && newIndex >= 0 && newIndex < proofs.length) {
      currentSlide.value = newIndex
    }
  }
}

onMounted(() => {
  const slider = document.querySelector('.proof-slider')
  if (slider) {
    slider.addEventListener('scroll', handleScroll)
  }
})
</script>

<style scoped>
.social-proof {
  padding: 80px 0;
  background: linear-gradient(180deg, #ffffff 0%, #ffffff 100%);
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

.proof-slider-wrapper {
  position: relative;
}

.proof-slider {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  padding-top: 20px;
  background: white;
}

.proof-card {
    width: calc(100vw - 60px);
    max-width: 340px;
    flex-shrink: 0;
    scroll-snap-align: center;
    overflow: visible;
    box-shadow: var(--shadow-md);
    border-radius: 20px;
  }

.proof-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.proof-visual {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  padding: 32px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
}

.comparison {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
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
  color: #888;
  margin-bottom: 8px;
}

.before .time {
  font-size: 24px;
  font-weight: 700;
  color: #ff6b6b;
}

.after .time {
  font-size: 28px;
  font-weight: 800;
}

.after .time.highlight {
  background: var(--bg-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.arrow {
  color: #67c23a;
  font-size: 24px;
  font-weight: 700;
}

.proof-content {
  padding: 28px;
  text-align: center;
}

.proof-icon {
  width: 56px;
  height: 56px;
  background: linear-gradient(135deg, #e8f8e8 0%, #d4f5d4 100%);
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #67c23a;
  margin: 0 auto 16px;
}

.proof-title {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
}

.proof-desc {
  font-size: 14px;
  color: var(--text-secondary);
  margin-bottom: 20px;
}

.proof-stat {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(180deg, #ffffff 0%, #ffffff 100%);
  padding: 12px 24px;
  border-radius: 12px;
}

.stat-value {
  font-size: 28px;
  font-weight: 800;
  color: #e6a23c;
}

.stat-label {
  font-size: 12px;
  color: #e6a23c;
}

@media (max-width: 1024px) {
  .proof-slider {
    grid-template-columns: repeat(2, 1fr);
  }

  .proof-card:last-child {
    grid-column: span 2;
    max-width: 400px;
    margin: 0 auto;
    border-radius: 20px;
  }

  .proof-card:last-child .proof-visual {
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
  }

  
}

@media (max-width: 900px) {
  .proof-slider-wrapper {
    margin: 0 -20px;
    padding: 0 20px;
  }
  
  .proof-slider {
    display: flex;
    overflow-x: auto;
    overflow-y: hidden;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scroll-behavior: smooth;
    padding: 20px 10px;
    gap: 16px;
    scrollbar-width: none;
    -ms-overflow-style: none;
    background: white;
  }

  .proof-slider::-webkit-scrollbar {
    display: none;
  }

  

  .proof-card .proof-visual {
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
  }

  .slider-dots {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 20px;
  }

  .slider-dots span {
    width: 8px;
    height: 8px;
    background: #E5E7EB;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .slider-dots span.active {
    background: var(--primary-color);
    width: 24px;
    border-radius: 4px;
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

  .proof-slider {
    padding: 20px 10px;
    gap: 16px;
    background: white;
  }

  

  .proof-card .proof-visual {
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
  }

  .comparison {
    flex-direction: column;
    gap: 12px;
  }

  .arrow {
    transform: rotate(90deg);
  }
}
</style>
