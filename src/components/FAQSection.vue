<template>
  <section class="faq" id="faq">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">你的顾虑，我们懂</h2>
        <p class="section-subtitle">4 大常见问题，一一解答</p>
      </div>
      <div class="faq-list">
        <el-collapse v-model="activeNames">
          <el-collapse-item 
            v-for="(faq, index) in faqs" 
            :key="index" 
            :name="index"
          >
            <template #title>
              <div class="faq-title">
                <div class="faq-icon">
                  <el-icon :size="20"><component :is="faq.icon" /></el-icon>
                </div>
                <span>{{ faq.question }}</span>
              </div>
            </template>
            <div class="faq-content">
              <p>{{ faq.answer }}</p>
              <ul v-if="faq.points">
                <li v-for="(point, pIndex) in faq.points" :key="pIndex">
                  <el-icon><Check /></el-icon>
                  {{ point }}
                </li>
              </ul>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { Lock, Warning, Key, CircleCheck, Check } from '@element-plus/icons-vue'

const activeNames = ref([0])

const faqs = [
  {
    icon: Warning,
    question: '怕不会装？',
    answer: '提供可视化面板与一键部署包，小白也能轻松上手。',
    points: ['可视化管理后台', '7×12 小时技术支持']
  },
  {
    icon: Lock,
    question: '怕封号？',
    answer: '内置频控与"人工审批闸门"，自动化不等于无人值守。',
    points: ['智能频率控制', '人工审核机制', ['合规标准', '风控预警']]
  },
  {
    icon: Key,
    question: '怕隐私泄露？',
    answer: '支持本地执行与私有化沙盒部署，数据不出域。',
    points: ['100% 本地部署', '私有化知识库', '数据不出域']
  },
  {
    icon: CircleCheck,
    question: '怕 AI 不靠谱？',
    answer: '每个岗位带"验收标准"，对齐确定性结果。',
    points: ['明确的交付标准', ['量化 KPI', '效果保障'], '不达标可退款']
  }
]
</script>

<style scoped>
.faq {
  padding: 80px 0;
  background: #ffffff;
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

.faq-list {
  max-width: 800px;
  margin: 0 auto;
}

.faq-list :deep(.el-collapse) {
  border: none;
}

.faq-list :deep(.el-collapse-item) {
  margin-bottom: 16px;
  border: 1px solid #e8eaf0;
  border-radius: 12px;
  overflow: hidden;
}

.faq-list :deep(.el-collapse-item.is-active) {
  border-color: var(--primary-light);
}

.faq-list :deep(.el-collapse-item__header) {
  height: auto;
  padding: 20px 24px;
  background: #ffffff;
  border: none;
  font-size: 16px;
  font-weight: 600;
  color: var(--text-primary);
  line-height: 1.5;
}

.faq-list :deep(.el-collapse-item__wrap) {
  border: none;
}

.faq-list :deep(.el-collapse-item__content) {
  padding: 0 24px 24px;
}

.faq-title {
  display: flex;
  align-items: center;
  gap: 16px;
}

.faq-icon {
  width: 40px;
  height: 40px;
  background: white;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
}

.faq-content {
  padding-left: 56px;
}

.faq-content p {
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 16px;
}

.faq-content ul {
  list-style: none;
}

.faq-content li {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  color: var(--text-primary);
  padding: 8px 0;
}

.faq-content .el-icon {
  color: #67c23a;
  font-weight: 700;
}

@media (max-width: 768px) {
  .faq {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .section-subtitle {
    font-size: 16px;
  }

  .faq-list :deep(.el-collapse-item__header) {
    padding: 16px;
    font-size: 15px;
  }

  .faq-title {
    gap: 12px;
  }

  .faq-icon {
    width: 36px;
    height: 36px;
  }

  .faq-content {
    padding-left: 0;
  }
}
</style>
