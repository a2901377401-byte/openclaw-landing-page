<template>
  <section class="products" id="features">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">5 大产品线，对应你的真实场景</h2>
        <p class="section-subtitle">不是功能清单，是岗位能力包</p>
      </div>
      <div class="product-slider-wrapper">
        <div class="product-slider">
          <div 
            class="product-card" 
            v-for="(product, index) in products" 
            :key="index"
            :class="{ 'featured': product.featured }"
          >
            <div class="product-badge" v-if="product.badge">{{ product.badge }}</div>
            <div class="product-icon">
              <el-icon :size="28"><component :is="product.icon" /></el-icon>
            </div>
            <h3 class="product-name">{{ product.name }}</h3>
            <p class="product-value">{{ product.value }}</p>
            <div class="product-divider"></div>
            <ul class="product-features">
              <li v-for="(feature, fIndex) in product.features" :key="fIndex">
                <el-icon><Check /></el-icon>
                <span>{{ feature }}</span>
              </li>
            </ul>
            <el-button 
              :type="product.featured ? 'primary' : 'default'" 
              class="product-btn"
              @click="goToWechat"
            >
              了解更多
            </el-button>
          </div>
        </div>
        <div class="slider-dots">
          <span 
            v-for="(_, index) in products" 
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
import { User, Document, Timer, Shop, OfficeBuilding, Check } from '@element-plus/icons-vue'

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/kfid/your-value', '_blank')
}

const products = [
  {
    icon: User,
    name: '岗位式数字员工库',
    value: '买的不是知识，是装上就能用的数字同事。',
    features: ['10 个岗位包', '权限/技能/任务模板', '验收标准'],
    badge: '最受欢迎',
    featured: true
  },
  {
    icon: Document,
    name: '内容矩阵流水线',
    value: '单篇内容 4.5 小时 → 23 分钟，省下时间换产出。',
    features: ['小红书/抖音模板', '7 天排期生成器', '内容质量检测'],
    featured: false
  },
  {
    icon: Timer,
    name: '本地提效套件',
    value: '把每周固定加班的 2 小时，压缩成 2 分钟。',
    features: ['周报生成器', '会议行动项闭环', '资料检索'],
    featured: false
  },
  {
    icon: Shop,
    name: 'AaaS 接单系统',
    value: '从按小时卖命，升级为按系统卖结果。',
    features: ['3 个行业 Demo', '报价单模板', '交付清单模板'],
    featured: false
  },
  {
    icon: OfficeBuilding,
    name: '企业级定制/托管',
    value: '让 AI 交付结果，而不是交付聊天。',
    features: ['企微/飞书接入', '私有化 RAG 知识库', '日志审计'],
    featured: false
  }
]

const currentSlide = ref(0)

const goToSlide = (index) => {
  currentSlide.value = index
  scrollToSlide(index)
}

const scrollToSlide = (index) => {
  const slider = document.querySelector('.product-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.product-card')?.offsetWidth || 260
    const gap = window.innerWidth <= 900 ? 12 : 24
    slider.scrollTo({
      left: index * (cardWidth + gap),
      behavior: 'auto'
    })
  }
}

const handleScroll = () => {
  const slider = document.querySelector('.product-slider')
  if (slider) {
    const cardWidth = slider.querySelector('.product-card')?.offsetWidth || 260
    const gap = window.innerWidth <= 900 ? 12 : 24
    const scrollLeft = slider.scrollLeft
    const newIndex = Math.round(scrollLeft / (cardWidth + gap))
    if (newIndex !== currentSlide.value && newIndex >= 0 && newIndex < products.length) {
      currentSlide.value = newIndex
    }
  }
}

onMounted(() => {
  const slider = document.querySelector('.product-slider')
  if (slider) {
    slider.addEventListener('scroll', handleScroll)
  }
})
</script>

<style scoped>
.products {
  padding: 80px 0;
  background: var(--bg-white);
  overflow: hidden;
}

.section-header {
  text-align: center;
  margin-bottom: 48px;
}

.section-title {
  font-size: 36px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 12px;
}

.section-subtitle {
  font-size: 18px;
  color: var(--text-secondary);
}

.product-slider-wrapper {
  position: relative;
}

.product-slider {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 24px;
  padding-top: 20px;
}

.product-card {
  background: white;
  border: 1px solid #E5E7EB;
  border-radius: 16px;
  padding: 32px 24px;
  text-align: center;
  position: relative;
  overflow: visible;
  transition: all 0.3s ease;
}

.product-card:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-lg);
}

.product-card.featured {
  border-color: var(--primary-color);
  background: linear-gradient(180deg, #FFFFFF 0%, #F8FAFC 100%);
}

.product-badge {
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--bg-gradient);
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.product-icon {
  width: 64px;
  height: 64px;
  background: linear-gradient(145deg, #F3F4F6 0%, #E5E7EB 100%);
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 20px;
  color: var(--primary-color);
}

.product-card.featured .product-icon {
  background: var(--bg-gradient);
  color: white;
}

.product-name {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 8px;
}

.product-value {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 20px;
}

.product-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, #E5E7EB, transparent);
  margin-bottom: 20px;
}

.product-features {
  list-style: none;
  text-align: left;
  margin-bottom: 24px;
}

.product-features li {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-secondary);
  margin-bottom: 8px;
}

.product-features .el-icon {
  color: var(--primary-color);
  font-weight: 700;
}

.product-btn {
  width: 100%;
}

.slider-dots {
  display: none;
  justify-content: center;
  gap: 10px;
  margin-top: 24px;
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

@media (max-width: 1200px) {
  .product-slider {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 900px) {
  .product-slider-wrapper {
    margin: 0 -20px;
    padding: 0 20px;
  }
  
  .product-slider {
    display: flex;
    overflow-x: auto;
    overflow-y: hidden;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scroll-behavior: smooth;
    padding: 20px 10px;
    gap: 12px;
    scrollbar-width: none;
    -ms-overflow-style: none;
  }

  .product-slider::-webkit-scrollbar {
    display: none;
  }

  .product-card {
    width: calc(100vw - 60px);
    max-width: 340px;
    flex-shrink: 0;
    scroll-snap-align: center;
    overflow: visible;
  }

  .slider-dots {
    display: flex;
  }

  .section-title {
    font-size: 28px;
  }
}

@media (max-width: 768px) {
  .products {
    padding: 48px 0;
  }

  .product-slider {
    padding: 20px 10px;
    gap: 12px;
  }

  .product-card {
    width: calc(100vw - 52px);
    max-width: 320px;
    padding: 24px 20px;
    overflow: visible;
  }

  .product-icon {
    width: 56px;
    height: 56px;
  }

  .product-name {
    font-size: 16px;
  }

  .product-value {
    font-size: 13px;
  }
}
</style>
