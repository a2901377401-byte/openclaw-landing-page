<template>
  <section class="merged-section">
    <div class="container">
      <div class="split-layout">
        <div class="pain-column">
          <h2 class="column-title">痛点</h2>
          <div class="pain-list">
            <div class="pain-item" v-for="(pain, index) in painPoints" :key="index">
              <div class="pain-icon">
                <el-icon :size="24"><component :is="pain.icon" /></el-icon>
              </div>
              <div class="pain-content">
                <h3>{{ pain.title }}</h3>
                <p>{{ pain.desc }}</p>
              </div>
            </div>
          </div>
        </div>
        <div class="result-column">
          <h2 class="column-title">结果</h2>
          <div class="result-list">
            <div class="result-item" v-for="(result, index) in results" :key="index">
              <div class="result-number">{{ index + 1 }}</div>
              <div class="result-content">
                <h3>{{ result.title }}</h3>
                <p>{{ result.desc }}</p>
                <span class="result-check">{{ result.check }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Lightning, Clock, Briefcase, Timer, Lock, Notebook } from '@element-plus/icons-vue'

const painPoints = [
  {
    icon: Lightning,
    title: '更新焦虑',
    desc: '每天追着 ChatGPT/Claude 跑，越学越焦虑，却拿不出一个能跑的项目。'
  },
  {
    icon: Clock,
    title: '效率黑洞',
    desc: '职场人被周报、会议纪要、重复搬运榨干，加班只是为了"整理信息"。'
  },
  {
    icon: Briefcase,
    title: '交付瓶颈',
    desc: '副业党靠体力堆时长，接单不可复制，没有可卖的产品化交付件。'
  }
]

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
.merged-section {
  padding: 80px 0;
  background: linear-gradient(180deg, #ffffff 0%, #ffffff 100%);
}

.split-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
}

.column-title {
  font-size: 28px;
  font-weight: 700;
  margin-bottom: 32px;
  color: var(--text-primary);
}

.pain-column {
  border-right: 1px solid #e8eaf0;
  padding-right: 60px;
}

.result-column {
  padding-left: 0;
}

.pain-list,
.result-list {
  display: flex;
  flex-direction: column;
  gap: 32px;
}

.pain-item {
  display: flex;
  gap: 16px;
}

.pain-icon {
  width: 48px;
  height: 48px;
  background: linear-gradient(135deg, #ff6b6b 0%, #ffa500 100%);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
}

.pain-content h3 {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
  color: var(--text-primary);
}

.pain-content p {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
}

.result-item {
  display: flex;
  gap: 16px;
}

.result-number {
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
  flex-shrink: 0;
}

.result-content h3 {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
  color: var(--text-primary);
}

.result-content p {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 12px;
}

.result-check {
  display: inline-block;
  background: linear-gradient(135deg, #e8f8e8 0%, #d4f5d4 100%);
  color: #67c23a;
  padding: 6px 16px;
  border-radius: 16px;
  font-size: 12px;
  font-weight: 500;
}

@media (max-width: 768px) {
  .merged-section {
    padding: 48px 0;
  }

  .split-layout {
    grid-template-columns: 1fr;
    gap: 48px;
  }

  .pain-column {
    border-right: none;
    padding-right: 0;
    border-bottom: 1px solid #e8eaf0;
    padding-bottom: 48px;
  }

  .column-title {
    font-size: 24px;
    margin-bottom: 24px;
  }

  .pain-list,
  .result-list {
    gap: 24px;
  }
}
</style>
