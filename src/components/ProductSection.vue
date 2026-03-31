<template>
  <section class="products" id="features">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">5 大产品线，对应你的真实场景</h2>
        <p class="section-subtitle">不是功能清单，是岗位能力包</p>
      </div>
      <div class="product-grid">
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
    </div>
  </section>
</template>

<script setup>
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
    features: ['小红书/抖音模板', '7 天排期生成器', '批量生产']
  },
  {
    icon: Timer,
    name: '本地提效套件',
    value: '把每周固定加班的 2 小时，压缩成 2 分钟。',
    features: ['周报生成器', '会议行动项闭环', '资料检索']
  },
  {
    icon: Shop,
    name: 'AaaS 接单系统',
    value: '从按小时卖命，升级为按系统卖结果。',
    features: ['3 个行业高频 Demo', '报价单模板', '交付清单']
  },
  {
    icon: OfficeBuilding,
    name: '企业级定制/托管',
    value: '让 AI 交付结果，而不是交付聊天。',
    features: ['企微/飞书接入', '私有化 RAG 知识库', '日志审计']
  }
]
</script>

<style scoped>
.products {
  padding: 80px 0;
  background: #fff;
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

.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.product-card {
  background: white;
  border: 1px solid #e8eaf0;
  border-radius: 20px;
  padding: 32px;
  position: relative;
  transition: var(--transition);
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.product-card.featured {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 2px rgba(0, 102, 255, 0.1);
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
  width: 56px;
  height: 56px;
  background: linear-gradient(135deg, #e8f4ff 0%, #d4ecff 100%);
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  margin-bottom: 20px;
}

.product-name {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 12px;
}

.product-value {
  font-size: 15px;
  color: var(--primary-color);
  font-weight: 500;
  line-height: 1.6;
  margin-bottom: 20px;
}

.product-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, #e8eaf0, transparent);
  margin-bottom: 20px;
}

.product-features {
  list-style: none;
  margin-bottom: 24px;
}

.product-features li {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  color: var(--text-secondary);
  padding: 8px 0;
}

.product-features .el-icon {
  color: #67c23a;
  font-weight: 700;
}

.product-btn {
  width: 100%;
  border-radius: 10px;
  font-weight: 600;
}

.product-card.featured .product-btn {
  background: var(--bg-gradient);
  border: none;
}

@media (max-width: 1024px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .products {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }

  .product-grid {
    grid-template-columns: 1fr;
  }

  .product-card {
    padding: 24px;
  }
}
</style>
