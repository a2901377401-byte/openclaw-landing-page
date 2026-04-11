<template>
  <section class="faq" id="faq">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">常见问题</h2>
        <p class="section-subtitle">售前 · 售中 · 售后，全流程答疑</p>
      </div>
      <div class="faq-columns">
        <div class="faq-column" v-for="(section, sIndex) in sections" :key="sIndex">
          <div class="column-header">
            <div class="column-icon">
              <el-icon :size="24"><component :is="section.icon" /></el-icon>
            </div>
            <h3 class="column-title">{{ section.title }}</h3>
          </div>
          <el-collapse v-model="section.actives">
            <el-collapse-item
              v-for="(faq, fIndex) in section.faqs"
              :key="fIndex"
              :name="fIndex"
            >
              <template #title>
                <div class="faq-title">
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
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { ShoppingCart, SetUp, Headset, Check } from '@element-plus/icons-vue'

const sections = [
  {
    title: '售前',
    icon: ShoppingCart,
    actives: ref([]),
    faqs: [
      {
        question: '数字员工和 ChatGPT/Claude 有什么区别？',
        answer: 'ChatGPT 是聊天工具，数字员工是岗位能力包。你不需要会写 Prompt，我们帮你把 AI 封装成"能干活的同事"。',
        points: ['开箱即用的岗位模板', '内置专业 Prompt 工程', '直接交付结果，不是对话']
      },
      {
        question: '我不懂技术，能用吗？',
        answer: '完全不需要技术背景。我们提供可视化面板和一键部署，像用手机 App 一样简单。',
        points: ['零代码操作', '可视化拖拽配置', '7×12 小时手把手指导']
      },
      {
        question: '适合什么规模的企业？',
        answer: '从个人到 500 人企业都有对应方案。体验包 29.9 元起步，先试再买。',
        points: ['个人/小团队：体验包起步', '中小企业：基础包覆盖', '大企业：定制包私有化部署']
      },
      {
        question: '数据安全吗？会不会泄露？',
        answer: '支持本地执行与私有化沙盒部署，数据不出域，满足企业合规要求。',
        points: ['100% 本地部署可选', '私有化知识库', '数据不出域，日志可审计']
      },
      {
        question: '可以先试用再决定吗？',
        answer: '所有方案均提供 7 天无理由退款，不满意全额退。体验包 29.9 元即可上手感受。',
        points: ['7 天无理由退款', '体验包低门槛试用', '不满意全额退']
      }
    ]
  },
  {
    title: '售中',
    icon: SetUp,
    actives: ref([]),
    faqs: [
      {
        question: '下单后多久能用上？',
        answer: '标准方案 24 小时内交付。下单后我们会联系你确认需求，当天配置完成。',
        points: ['24 小时内交付', '专人对接确认需求', '远程协助配置上线']
      },
      {
        question: '怎么接入企微/飞书？',
        answer: '我们提供标准化接入流程，5 分钟完成绑定。支持企微、飞书、钉钉等主流办公平台。',
        points: ['5 分钟完成绑定', '企微/飞书/钉钉全覆盖', '无需 IT 部门协助']
      },
      {
        question: '知识库怎么搭建？需要多久？',
        answer: '上传文档即可自动解析建库。PDF、Word、Excel 等格式均支持，通常 1-2 小时完成。',
        points: ['拖拽上传自动解析', '支持 PDF/Word/Excel/网页', '1-2 小时完成基础建库']
      },
      {
        question: '会不会被封号？',
        answer: '内置频控与"人工审批闸门"，自动化不等于无人值守，合规运行有保障。',
        points: ['智能频率控制', '人工审核机制', '合规标准与风控预警']
      },
      {
        question: '能同时部署多个数字员工吗？',
        answer: '可以。每个数字员工独立运行，互不干扰。基础包支持 3 个，定制包不限数量。',
        points: ['多员工独立运行', '基础包支持 3 个', '定制包不限数量']
      }
    ]
  },
  {
    title: '售后',
    icon: Headset,
    actives: ref([]),
    faqs: [
      {
        question: 'AI 回答不准确怎么办？',
        answer: '每个岗位带"验收标准"，对齐确定性结果。不达标可退款，持续优化到满意为止。',
        points: ['明确的交付标准', '量化 KPI 与效果保障', '不达标可退款']
      },
      {
        question: '后续有技术支持吗？',
        answer: '7×12 小时技术支持，响应时间不超过 30 分钟。重大问题 2 小时内解决。',
        points: ['7×12 小时在线', '30 分钟内响应', '重大问题 2 小时解决']
      },
      {
        question: '产品会持续更新吗？',
        answer: '每月迭代更新，新增岗位模板和技能包。老用户免费升级，不额外收费。',
        points: ['每月迭代更新', '新增岗位模板免费', '技能包持续扩充']
      },
      {
        question: '可以退款吗？',
        answer: '7 天无理由退款，不满意全额退。7 天后按比例退款，具体联系客服。',
        points: ['7 天无理由全额退', '7 天后按比例退款', '专属客服对接']
      },
      {
        question: '想增加新功能怎么办？',
        answer: '定制包支持按需开发。提交需求后 3-5 个工作日出方案，确认后 1-2 周交付。',
        points: ['按需定制开发', '3-5 天出方案', '1-2 周快速交付']
      }
    ]
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

.faq-columns {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}

.faq-column {
  background: #FAFAFA;
  border-radius: 16px;
  padding: 28px 24px;
}

.column-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
  padding-bottom: 16px;
  border-bottom: 2px solid #E5E7EB;
}

.column-icon {
  width: 44px;
  height: 44px;
  background: var(--bg-gradient);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
}

.column-title {
  font-size: 20px;
  font-weight: 700;
  color: var(--text-primary);
}

.faq-column :deep(.el-collapse) {
  border: none;
}

.faq-column :deep(.el-collapse-item) {
  margin-bottom: 10px;
  border: 1px solid #e8eaf0;
  border-radius: 10px;
  overflow: hidden;
  background: white;
}

.faq-column :deep(.el-collapse-item.is-active) {
  border-color: var(--primary-light);
}

.faq-column :deep(.el-collapse-item__header) {
  height: auto;
  padding: 14px 16px;
  background: #ffffff;
  border: none;
  font-size: 14px;
  font-weight: 600;
  color: var(--text-primary);
  line-height: 1.5;
}

.faq-column :deep(.el-collapse-item__wrap) {
  border: none;
}

.faq-column :deep(.el-collapse-item__content) {
  padding: 0 16px 14px;
}

.faq-title {
  display: flex;
  align-items: center;
  gap: 12px;
}

.faq-content p {
  font-size: 13px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 10px;
}

.faq-content ul {
  list-style: none;
}

.faq-content li {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
  color: var(--text-primary);
  padding: 4px 0;
}

.faq-content .el-icon {
  color: #A855F7;
  font-weight: 700;
  font-size: 12px;
}

@media (max-width: 1024px) {
  .faq-columns {
    grid-template-columns: 1fr;
    gap: 24px;
  }
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

  .faq-column {
    padding: 20px 16px;
  }

  .column-title {
    font-size: 18px;
  }

  .faq-column :deep(.el-collapse-item__header) {
    padding: 12px 14px;
    font-size: 13px;
  }
}
</style>
