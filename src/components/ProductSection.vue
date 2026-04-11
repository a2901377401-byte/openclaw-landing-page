<template>
  <section class="products" id="features">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">5 大产品线，对应你的真实场景</h2>
        <p class="section-subtitle">不是功能清单，是岗位能力包</p>
      </div>
      <div class="product-slider-wrapper">
        <div class="product-slider">
          <div class="product-card" v-for="(product, index) in products" :key="index"
            :class="{ 'featured': product.featured }">
            <div class="product-badge" v-if="product.badge">{{ product.badge }}</div>
            <div class="product-icon">
              <el-icon :size="28">
                <component :is="product.icon" />
              </el-icon>
            </div>
            <h3 class="product-name">{{ product.name }}</h3>
            <p class="product-value">{{ product.value }}</p>
            <div class="product-divider"></div>
            <ul class="product-features">
              <li v-for="(feature, fIndex) in product.features" :key="fIndex">
                <el-icon>
                  <Check />
                </el-icon>
                <span>{{ feature }}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="slider-dots">
          <span v-for="(_, index) in products" :key="index" :class="{ active: currentSlide === index }"
            @click="goToSlide(index)"></span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { User, Document, Timer, Shop, OfficeBuilding, Check } from '@element-plus/icons-vue'
import CONTACT_INFO from '../config/contact.js'

const goToWechat = () => {
  window.location.href = `weixin://dl/addfriend/${CONTACT_INFO.wechat}`
}

const products = [
  {
    icon: User,
    name: '职场精英 | 全天候超级助理',
    headline: '7×24 极速响应，您的专属数字智囊',
    value: '开箱即用，免去繁琐指令。从常规复盘到深度竞品分析，直达精准交付。',
    features: ['全场景岗位技能矩阵', '企业级权限与模板管控', '具备自学习进化能力'],
    badge: '1万+ 人在用',
    featured: true
  },
  {
    icon: Document,
    name: '新媒体 | 矩阵增长引擎',
    headline: '以一当十，重构全域分发效能',
    value: '单篇内容耗时缩减90%。集成排期、创作与质检的全自动工作流，让您专注流量破局与变现。',
    features: ['全平台自适应创作模板', '智能周期排期引擎', '爆款因子检测与调优'],
    featured: false
  },
  {
    icon: Timer,
    name: '个人效能 | 智能敏捷副驾',
    headline: '将2小时繁杂，重构为2分钟极简',
    value: '智能接管汇报、纪要与资料梳理等机械劳作，释放您的核心专注力与创造力。',
    features: ['智能汇报一键生成', '核心行动项自动追踪', '多源信息深度结构化'],
    featured: false
  },
  {
    icon: Shop,
    name: '一人企业 | 商业运营中枢',
    headline: '超越时间贩卖，构建自动化商业飞轮',
    value: '沉淀行业标杆SOP，打通报价与交付闭环。全链路进度自动同步，助您从容拓展业务版图。',
    features: ['高净值行业标杆 SOP', '全链路商业交付模板', '履约进度无缝协同'],
    featured: false
  },
  {
    icon: OfficeBuilding,
    name: '集团企业 | 数字生产力引擎',
    headline: '聚焦结果交付，重塑企业级合规劳动力',
    value: '深度集成主流协同生态，构建私有化知识底座。打造安全、可控、可溯源的专属数字团队。',
    features: ['企业办公生态深度融合', '私有化 RAG 知识底座', '全链路风控与日志审计'],
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
  width: 110%;
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
