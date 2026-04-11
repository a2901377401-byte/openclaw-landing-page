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
    desc: '独立环境运行，隐私绝对安全。企业级沙盒部署，满足合规要求。',
    check: '数据不出域，安全更放心'
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
}

.split-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
}

.column-title {
  font-size: 28px;
  font-weight: 700;
  margin-bottom: 28px;
  color: white;
  padding-bottom: 16px;
  border-bottom: 2px solid rgba(255, 255, 255, 0.3);
}

.pain-column {
  position: relative;
  padding: 32px;
  background-image: url('/痛点.png');
  background-size: cover;
  background-position: right;
  border-radius: 20px;
  overflow: hidden;
}

.pain-column::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(60, 30, 30, 0.6) 0%, rgba(40, 15, 15, 0.8) 100%);
  z-index: 0;
}

.result-column {
  position: relative;
  padding: 32px;
  background-image: url('/结果.png');
  background-size: cover;
  background-position: right;
  border-radius: 20px;
  overflow: hidden;
}

.result-column::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(25, 45, 65, 0.6) 0%, rgba(15, 35, 55, 0.8) 100%);
  z-index: 0;
}

.pain-column > *,
.result-column > * {
  position: relative;
  z-index: 1;
}

.pain-list,
.result-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.pain-item {
  display: flex;
  align-items: flex-start;
  gap: 16px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 14px;
  transition: all 0.3s ease;
}

.pain-item:hover {
  background: rgba(255, 255, 255, 0.14);
  transform: translateX(4px);
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
  box-shadow: 0 4px 12px rgba(255, 107, 107, 0.3);
}

.pain-content h3 {
  font-size: 17px;
  font-weight: 700;
  margin-bottom: 6px;
  color: #ffffff;
}

.pain-content p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.7;
}

.result-item {
  display: flex;
  align-items: flex-start;
  gap: 16px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 14px;
  transition: all 0.3s ease;
}

.result-item:hover {
  background: rgba(255, 255, 255, 0.14);
  transform: translateX(4px);
}

.result-number {
  width: 34px;
  height: 34px;
  background: var(--bg-gradient);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 15px;
  flex-shrink: 0;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.4);
}

.result-content h3 {
  font-size: 17px;
  font-weight: 700;
  margin-bottom: 6px;
  color: #ffffff;
}

.result-content p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.7;
  margin-bottom: 10px;
}

.result-check {
  display: inline-block;
  background: linear-gradient(135deg, #A855F7 0%, #C084FC 100%);
  color: white;
  padding: 5px 14px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba(168, 85, 247, 0.3);
}

@media (max-width: 768px) {
  .merged-section {
    padding: 48px 0;
  }

  .split-layout {
    grid-template-columns: 1fr;
    gap: 32px;
  }

  .column-title {
    font-size: 24px;
    margin-bottom: 20px;
  }

  .pain-column,
  .result-column {
    padding: 24px 20px;
  }

  .pain-list,
  .result-list {
    gap: 16px;
  }

  .pain-item,
  .result-item {
    padding: 16px;
  }

  .pain-icon {
    width: 42px;
    height: 42px;
  }

  .pain-content h3,
  .result-content h3 {
    font-size: 15px;
  }

  .pain-content p,
  .result-content p {
    font-size: 12px;
  }
}
</style>
