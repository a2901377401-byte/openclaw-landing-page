<template>
  <section class="pain-point">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">你，是不是也这样？</h2>
        <p class="section-subtitle">别再骗自己"在学 AI"了，这些场景说的就是你</p>
      </div>
      <div class="pain-cards">
        <div class="pain-card" v-for="(pain, index) in painPoints" :key="index">
          <div class="pain-icon">
            <el-icon :size="32"><component :is="pain.icon" /></el-icon>
          </div>
          <h3 class="pain-title">{{ pain.title }}</h3>
          <p class="pain-desc">{{ pain.desc }}</p>
          <div class="pain-tags">
            <el-tag v-for="tag in pain.tags" :key="tag" type="info" effect="plain">
              {{ tag }}
            </el-tag>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Lightning, Clock, Briefcase } from '@element-plus/icons-vue'

const painPoints = [
  {
    icon: Lightning,
    title: '更新焦虑',
    desc: '每天追着 ChatGPT/Claude 跑，越学越焦虑，却拿不出一个能跑的项目。',
    tags: ['学不完', '焦虑', '无产出']
  },
  {
    icon: Clock,
    title: '效率黑洞',
    desc: '职场人被周报、会议纪要、重复搬运榨干，加班只是为了"整理信息"。',
    tags: ['加班常态化', '无价值感', '身心俱疲']
  },
  {
    icon: Briefcase,
    title: '交付瓶颈',
    desc: '副业党靠体力堆时长，接单不可复制，没有可卖的产品化交付件。',
    tags: ['靠体力', '不可复制', '赚辛苦钱']
  }
]
</script>

<style scoped>
.pain-point {
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

.pain-cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.pain-card {
  background: linear-gradient(145deg, #f8f9fc 0%, #fff 100%);
  border: 1px solid #e8eaf0;
  border-radius: 16px;
  padding: 32px;
  transition: var(--transition);
}

.pain-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
  border-color: var(--primary-light);
}

.pain-icon {
  width: 64px;
  height: 64px;
  background: linear-gradient(135deg, #ff6b6b 0%, #ffa500 100%);
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  margin-bottom: 20px;
}

.pain-title {
  font-size: 20px;
  font-weight: 700;
  margin-bottom: 12px;
  color: var(--text-primary);
}

.pain-desc {
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 20px;
}

.pain-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.pain-tags .el-tag {
  background: #f0f2f5;
  border: none;
  border-radius: 6px;
}

@media (max-width: 1024px) {
  .pain-cards {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .pain-point {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }

  .pain-cards {
    grid-template-columns: 1fr;
  }

  .pain-card {
    padding: 24px;
  }
}
</style>
