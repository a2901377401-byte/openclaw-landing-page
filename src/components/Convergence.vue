<template>
  <section class="convergence">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">我们要的结果，就 3 个</h2>
        <p class="section-subtitle">不搞虚的，给你实实在在的确定性</p>
      </div>
      <div class="result-cards">
        <div class="result-card" v-for="(result, index) in results" :key="index">
          <div class="result-number">{{ index + 1 }}</div>
          <div class="result-icon">
            <el-icon :size="36"><component :is="result.icon" /></el-icon>
          </div>
          <h3 class="result-title">{{ result.title }}</h3>
          <p class="result-desc">{{ result.desc }}</p>
          <div class="result-check">
            <el-icon><Check /></el-icon>
            <span>{{ result.check }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Timer, Lock, Notebook } from '@element-plus/icons-vue'

const results = [
  {
    icon: Timer,
    title: '24 小时内上手',
    desc: '拥有能在企微/飞书里干活的 AI 同事，不需要等待漫长的部署周期。',
    check: '今晚下单，明天就能用'
  },
  {
    icon: Lock,
    title: '本地执行，隐私安全',
    desc: '数据不出网，隐私绝对安全。企业级沙盒部署，满足合规要求。',
    check: '数据100%不出本地'
  },
  {
    icon: Notebook,
    title: '可复用的岗位模板',
    desc: '一次搭建，持续产生价值。岗位模板库让你复制 N 个数字员工。',
    check: '一次投入，长期回报'
  }
]
</script>

<style scoped>
.convergence {
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

.result-cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.result-card {
  background: white;
  border-radius: 20px;
  padding: 40px 32px;
  text-align: center;
  position: relative;
  box-shadow: var(--shadow-md);
  transition: var(--transition);
}

.result-card:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-lg);
}

.result-number {
  position: absolute;
  top: -16px;
  left: 50%;
  transform: translateX(-50%);
  width: 32px;
  height: 32px;
  background: var(--bg-gradient);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 14px;
}

.result-icon {
  width: 80px;
  height: 80px;
  background: linear-gradient(135deg, #e8f4ff 0%, #d4ecff 100%);
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  margin: 0 auto 24px;
}

.result-title {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 16px;
}

.result-desc {
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 24px;
}

.result-check {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: linear-gradient(135deg, #e8f8e8 0%, #d4f5d4 100%);
  color: #67c23a;
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 500;
}

.result-check .el-icon {
  font-weight: 700;
}

@media (max-width: 1024px) {
  .result-cards {
    grid-template-columns: repeat(2, 1fr);
  }

  .result-card:last-child {
    grid-column: span 2;
    max-width: 400px;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .convergence {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }

  .result-cards {
    grid-template-columns: 1fr;
  }

  .result-card:last-child {
    grid-column: span 1;
    max-width: 100%;
  }

  .result-card {
    padding: 32px 24px;
  }
}
</style>
