<template>
  <section class="pricing" id="pricing">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">选择你的起步方式</h2>
        <p class="section-subtitle">总有一款适合你</p>
      </div>
      <div class="pricing-cards">
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
          <div class="pricing-price">
            <span class="price-currency">¥</span>
            <span class="price-amount">{{ plan.price }}</span>
            <span class="price-period" v-if="plan.period">/{{ plan.period }}</span>
          </div>
          <ul class="pricing-features">
            <li v-for="(feature, fIndex) in plan.features" :key="fIndex">
              <el-icon><Check /></el-icon>
              <span>{{ feature }}</span>
            </li>
          </ul>
          <el-button 
            :type="plan.featured ? 'primary' : 'default'" 
            size="large"
            class="pricing-btn"
            @click="goToWechat"
          >
            {{ plan.cta }}
          </el-button>
        </div>
      </div>
      <div class="pricing-footer">
        <p>所有方案均提供 <strong>7 天无理由退款</strong>，不满意全额退</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Check } from '@element-plus/icons-vue'

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/kfid/your-value', '_blank')
}

const plans = [
  {
    name: '岗位模板包',
    desc: '快速体验成果',
    price: '999',
    period: '起',
    features: [
      '3 个岗位模板',
      '基础配置指导',
      '社群答疑支持',
      '持续更新'
    ],
    cta: '立即购买',
    featured: false
  },
  {
    name: '数字员工库',
    desc: '个人/小团队首选',
    price: '3999',
    period: '年',
    features: [
      '10 个岗位包',
      '企微/飞书接入',
      '优先技术支持',
      '定制化配置',
      '岗位模板持续更新'
    ],
    cta: '立即部署',
    badge: '热销第一',
    featured: true
  },
  {
    name: 'AaaS 系统',
    desc: '老板和工作室',
    price: '9999',
    period: '起',
    features: [
      '3 个行业高频 Demo',
      '报价单/交付清单模板',
      '专属客服',
      '一对一培训',
      '优先新功能体验'
    ],
    cta: '联系商务',
    featured: false
  }
]
</script>

<style scoped>
.pricing {
  padding: 80px 0;
  background: linear-gradient(180deg, #fff 0%, #f0f7ff 100%);
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

.pricing-cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 1000px;
  margin: 0 auto;
}

.pricing-card {
  background: white;
  border: 1px solid #e8eaf0;
  border-radius: 20px;
  padding: 40px 32px;
  text-align: center;
  position: relative;
  transition: var(--transition);
}

.pricing-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.pricing-card.featured {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 2px rgba(0, 102, 255, 0.1);
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
  color: #67c23a;
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
  color: #67c23a;
}

@media (max-width: 1024px) {
  .pricing-cards {
    grid-template-columns: 1fr;
    max-width: 400px;
  }

  .pricing-card.featured {
    transform: none;
  }

  .pricing-card.featured:hover {
    transform: translateY(-4px);
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
    padding: 32px 24px;
  }

  .price-amount {
    font-size: 40px;
  }
}
</style>
