<template>
  <section class="pricing" id="pricing">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">选择你的起步方式</h2>
        <p class="section-subtitle">总有一款适合你</p>
      </div>
      <div class="pricing-slider-wrapper">
        <div class="pricing-slider">
          <div 
            class="pricing-card" 
            v-for="(plan, index) in plans" 
            :key="index"
            :class="{ featured: plan.featured }"
          >
            <div class="pricing-badge" v-if="plan.badge">{{ plan.badge }}</div>
            <div class="pricing-header">
              <h3 class="plan-name">{{ plan.name }}</h3>
              <p class="plan-desc">{{ plan.desc }}</p>
            </div>
            <div class="pricing-price" v-if="plan.price">
              <span class="price-amount">{{ plan.price }}</span>
              <span class="price-period" v-if="plan.period">/{{ plan.period }}</span>
            </div>
            <div class="pricing-contact" v-else @click="goToWechat">
              <el-icon :size="40"><Headset /></el-icon>
              <span class="contact-text">联系定制</span>
            </div>
            <ul class="pricing-features">
              <li v-for="(feature, fIndex) in plan.features" :key="fIndex">
                <el-icon><Check /></el-icon>
                <span>{{ feature }}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="slider-dots">
          <span 
            v-for="(_, index) in plans" 
            :key="index"
            :class="{ active: currentSlide === index }"
            @click="goToSlide(index)"
          ></span>
        </div>
      </div>
      <div class="pricing-footer">
        <p>所有方案均提供 <strong>7 天无理由退款</strong>，不满意全额退</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Check, Headset } from '@element-plus/icons-vue'
import CONTACT_INFO from '../config/contact.js'

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/ca/cawcde427f39be25db', '_blank')
}

const plans = [
  {
    name: '体验包',
    desc: '快速体验',
    price: '¥29.9',
    period: '起',
    badge: '热销第一',
    featured: true
  },
  {
    name: '基础包',
    desc: '快速体验成果',
    price: '¥99.9',
    period: '起',
    featured: false
  },
  {
    name: '定制包',
    desc: '个人/小团队首选',
    featured: false
  },
]

const currentSlide = ref(0)

const goToSlide = (index) => {
  currentSlide.value = index
  scrollToSlide(index)
}

const scrollToSlide = (index) => {
  const slider = document.querySelector('.pricing-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.pricing-card')?.offsetWidth || 300
    const gap = window.innerWidth <= 900 ? 16 : 24
    slider.scrollTo({
      left: index * (cardWidth + gap),
      behavior: 'auto'
    })
  }
}

const handleScroll = () => {
  const slider = document.querySelector('.pricing-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.pricing-card')?.offsetWidth || 300
    const gap = window.innerWidth <= 900 ? 16 : 24
    const scrollLeft = slider.scrollLeft
    const newIndex = Math.round(scrollLeft / (cardWidth + gap))
    if (newIndex !== currentSlide.value && newIndex >= 0 && newIndex < plans.length) {
      currentSlide.value = newIndex
    }
  }
}

onMounted(() => {
  const slider = document.querySelector('.pricing-slider')
  if (slider) {
    slider.addEventListener('scroll', handleScroll)
  }
})
</script>

<style scoped>
.pricing {
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

.pricing-slider-wrapper {
  position: relative;
}

.pricing-slider {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  padding-top: 20px;
}

.pricing-card {
  background: white;
  border: 1px solid #e8eaf0;
  border-radius: 20px;
  padding: 40px 32px;
  text-align: center;
  position: relative;
  transition: var(--transition);
  overflow: visible;
}

.pricing-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.pricing-card.featured {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 2px rgba(168, 85, 247, 0.1);
  transform: scale(1.05);
}

.pricing-card.featured:hover {
  transform: scale(1.05) translateY(-4px);
}

.pricing-badge {
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--bg-gradient);
  color: white;
  padding: 6px 20px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.pricing-header {
  margin-bottom: 24px;
}

.plan-name {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 8px;
}

.plan-desc {
  font-size: 14px;
  color: var(--text-secondary);
}

.pricing-price {
  display: flex;
  align-items: baseline;
  justify-content: center;
  margin-bottom: 32px;
}

.price-currency {
  font-size: 20px;
  font-weight: 600;
  color: var(--text-secondary);
}

.price-amount {
  font-size: 48px;
  font-weight: 800;
  color: var(--text-primary);
}

.pricing-card.featured .price-amount {
  background: var(--bg-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.price-period {
  font-size: 14px;
  color: var(--text-secondary);
  margin-left: 4px;
}

.pricing-contact {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  margin-bottom: 32px;
  padding: 24px;
  background: linear-gradient(135deg, #F3E8FF 0%, #E9D5FF 100%);
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.pricing-contact:hover {
  transform: scale(1.05);
  box-shadow: var(--shadow-md);
}

.pricing-contact .el-icon {
  color: #A855F7;
}

.contact-text {
  font-size: 18px;
  font-weight: 700;
  color: #A855F7;
}

.pricing-features {
  list-style: none;
  text-align: left;
  margin-bottom: 32px;
}

.pricing-features li {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 14px;
  color: var(--text-secondary);
  padding: 10px 0;
  border-bottom: 1px solid #f0f2f5;
}

.pricing-features li:last-child {
  border-bottom: none;
}

.pricing-features .el-icon {
  color: #A855F7;
  font-weight: 700;
  font-size: 16px;
}

.pricing-btn {
  width: 100%;
  height: 48px !important;
  font-size: 16px !important;
  font-weight: 600 !important;
  border-radius: 12px !important;
}

.pricing-card.featured .pricing-btn {
  background: var(--bg-gradient) !important;
  border: none !important;
}

.pricing-footer {
  text-align: center;
  margin-top: 40px;
  font-size: 14px;
  color: var(--text-secondary);
}

.pricing-footer strong {
  color: #A855F7;
}

@media (max-width: 1024px) {
  .pricing-slider {
    grid-template-columns: repeat(2, 1fr);
  }

  .pricing-card.featured {
    transform: none;
  }

  .pricing-card.featured:hover {
    transform: translateY(-4px);
  }
}

@media (max-width: 900px) {
  .pricing-slider-wrapper {
    margin: 0 -20px;
    padding: 0 20px;
  }
  
  .pricing-slider {
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
  }

  .pricing-slider::-webkit-scrollbar {
    display: none;
  }

  .pricing-card {
    width: calc(100vw - 60px);
    max-width: 340px;
    flex-shrink: 0;
    scroll-snap-align: center;
    border-radius: 20px;
  }

  .pricing-card.featured {
    transform: none;
  }

  .pricing-card.featured:hover {
    transform: translateY(-4px);
  }

  .pricing-card .pricing-badge {
    border-radius: 20px;
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
  .pricing {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }


  .pricing-card {
    width: calc(100vw - 52px);
    max-width: 320px;
    padding: 32px 24px;
  }

  .price-amount {
    font-size: 40px;
  }
}
</style>
