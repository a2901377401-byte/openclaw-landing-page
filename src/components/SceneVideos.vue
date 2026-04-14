<template>
  <section class="scene-videos" id="cases">
    <div class="scene-header">
      <h2 class="scene-title">主要服务对象的业务输出</h2>
    </div>
    <div class="container">
      <div class="scene-nav">
        <button
          v-for="category in categories"
          :key="category.id"
          class="nav-btn"
          :class="{ active: currentCategory === category.id }"
          @click="switchCategory(category.id)"
        >
          <el-icon :size="24"><component :is="category.icon" /></el-icon>
          <span class="nav-label">{{ category.label }}</span>
        </button>
      </div>
      
      <div class="scene-content">
        <div class="content-section">
          <h3 class="section-label">AI数字帮手</h3>
          <div class="cards-grid">
            <div class="card-item" v-for="(agent, index) in currentAgents" :key="'agent-' + index">
              <div class="card-name">{{ agent.name }}</div>
              <div class="card-desc">{{ agent.desc }}</div>
            </div>
            <div class="card-item more-card" key="agent-more">
              <div class="more-dots">•••</div>
            </div>
          </div>
        </div>
        
        <div class="content-section">
          <h3 class="section-label">魔法棒</h3>
          <div class="cards-grid skills-grid">
            <div class="card-item skill-card" v-for="(skill, index) in currentSkills" :key="'skill-' + index">
              <div class="card-name">{{ skill.name }}</div>
              <div class="card-desc">{{ skill.desc }}</div>
            </div>
            <div class="card-item skill-card more-card" key="skill-more">
              <div class="more-dots">•••</div>
            </div>
          </div>
        </div>
      </div>
      
      <div class="scene-dots">
        <span
          v-for="category in categories"
          :key="category.id"
          :class="{ active: currentCategory === category.id }"
          @click="switchCategory(category.id)"
        ></span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'
import { Reading, Briefcase, OfficeBuilding, Promotion } from '@element-plus/icons-vue'

const categories = [
  {
    id: 'student',
    label: '学生',
    icon: Reading,
    agents: [
      { name: '论文枪手', desc: '论文阅读与综述助理' },
      { name: '课程作业指导师', desc: '作业辅导与知识点讲解' },
      { name: '英语技能教练', desc: '英语写作与口语教练' },
      { name: '简历优化师', desc: '求职简历优化' },
      { name: '实习自动化打卡', desc: '电子书制作' },
      { name: 'AI信息检索师', desc: 'AI信息检索' },
      { name: '考研复习规划师', desc: '定制化学习路线与进度打卡' } // 补充的第 7 个
    ],
    skills: [
      { name: 'Word 文档处理', desc: '报告、备忘录、合同、论文排版' },
      { name: 'PDF 处理工具箱', desc: '论文阅读、报告合并、表单填写' },
      { name: 'Excel 表格处理', desc: '成绩统计、实验数据、课程表' },
      { name: 'PPT 演示文稿', desc: '课堂汇报、毕业答辩、学术演讲' },
      { name: '海报设计', desc: '海报/封面快速设计' },
      { name: '思维导图生成', desc: '知识结构化、复习大纲、概念梳理' },
      { name: 'Notion 笔记与知识库', desc: '课程笔记整理、项目管理、资料库建设' },
      { name: '17搜索引擎聚合', desc: '学术搜索、资料查找、多语言检索' },
      { name: '联网实时搜索', desc: '查资料、搜热点、找新闻' },
      { name: '内容工厂', desc: '多平台内容生成、SEO优化写作、内容日历' },
      { name: '图片生成/创意', desc: 'AI图像生成助手（入门）' },
      { name: '敏感词检测助手', desc: '平台合规与敏感词审核' },
      { name: '时间管理大师', desc: 'AI时间管理助理' },
      { name: '论文查重与降重', desc: '学术规范审查与修改建议' } // 补充的第 14 个
    ]
  },
  {
    id: 'office',
    label: '白领',
    icon: Briefcase,
    agents: [
      { name: '视频剪辑师', desc: '剪辑后顾' },
      { name: '数据分析师', desc: '职场白领' },
      { name: '课程作业指导师', desc: 'Excel/BI数据分析助理' },
      { name: '数字人视频师', desc: '兼职党' },
      { name: '邮件助手', desc: '职场白领' },
      { name: '项目SOP', desc: '职场白领' },
      { name: '绩效考核助手', desc: '职业白领' },
      // { name: '项目投标师', desc: '职场白领' }, // 多余注释
      // { name: '合同助手', desc: '职场白领' } // 多余注释
    ],
    skills: [
      { name: '统一邮件入口', desc: '智能路由到最合适通道' },
      { name: '腾讯文档', desc: '团队协作文档、表格、幻灯片' },
      { name: '腾讯会议', desc: '视频会议全流程管理、AI会议纪要' },
      { name: '腾讯问卷', desc: '员工调查、客户反馈、投票测评' },
      { name: '金融数据搜索', desc: '自然语言查询股票/基金/宏观' },
      { name: 'Word 文档处理', desc: '合同/报告/方案/会议纪要' },
      { name: 'PDF 处理工具箱', desc: '合同审阅、发票归档、表单填写' },
      { name: 'Excel 表格处理', desc: '销售报表、财务模型、数据分析' },
      { name: 'PPT 演示文稿', desc: '商务提案、工作汇报、年度总结' },
      { name: '浏览器自动化', desc: '批量操作企业系统、自动填报数据' },
      { name: 'GitHub 代码仓库', desc: 'Issue/PR管理、Actions CI/CD' },
      { name: '主题工厂', desc: '10套专业配色/字体主题' },
      { name: '云文件上传备份', desc: '腾讯SMH云存储、团队共享' },
      { name: 'Web Artifact 构建器', desc: 'React/Tailwind/shadcn/ui数据仪表盘' }
    ]
  },
  {
    id: 'enterprise',
    label: '企业',
    icon: OfficeBuilding,
    agents: [
      { name: '品牌故事', desc: '企业介绍、创始人IP、文化输出' },
      { name: '产品推广', desc: '功能介绍、案例展示、对比评测' },
      { name: '行业报告', desc: '白皮书、市场研究、趋势预测' },
      { name: '客户案例', desc: '成功故事、解决方案、ROI展示' },
      { name: '流程自动化', desc: '审批流程、数据流转' },
      { name: '知识管理', desc: '企业知识库、文档归档' },
      { name: '客户服务', desc: '智能客服、工单处理' },
      // { name: '数据架构师', desc: 'B端商家/创业者' }, // 多余注释
      // { name: '财务助手', desc: 'B端商家/创业者' } // 多余注释
    ],
    skills: [
      { name: 'GitHub 企业协作', desc: '覆盖企业代码管理全场景' },
      { name: '腾讯文档企业版', desc: '团队协作文档管理、权限控制' },
      { name: '腾讯会议企业版', desc: '视频会议 + AI 会议纪要' },
      { name: '腾讯问卷企业版', desc: '大规模调查与数据分析' },
      { name: '云文件企业备份', desc: '团队文件统一管理' },
      { name: '企业邮箱管理', desc: '商务邮件全流程' },
      { name: '企业邮箱完整收发', desc: 'SMTP/IMAP 全协议' },
      { name: '企业文档处理', desc: '合同/协议/报告专业排版' },
      { name: 'PDF 企业应用', desc: '合同审阅/电子签章/水印加密' },
      { name: 'Excel 企业报表', desc: '财务/销售/运营数据可视化' },
      { name: 'PPT 商业提案', desc: '客户提案/路演/汇报' },
      { name: '前端界面设计', desc: '企业级 Web UI、数据可视化界面' },
      { name: '企业金融数据', desc: '市场行情/投资决策支持' },
      { name: 'Slack GIF 动图', desc: '企业沟通趣味素材、团队表情包' }
    ]
  },
  {
    id: 'social',
    label: '社媒',
    icon: Promotion,
    agents: [
      { name: '短视频工作流', desc: '抖音短视频工作流' },
      { name: '带货视频工作流', desc: '电商平台视频工作流' },
      { name: 'Landing page落地页制作', desc: '营销广告投放' },
      { name: '社媒爆款探测器', desc: '社媒达人' },
      { name: '直播带货短探视', desc: '直播带货工作流' },
      { name: '小红书爆款引擎', desc: '小红书爆款笔记工作流' },
      { name: '人工客服客服服', desc: 'B端商家/创业者' },
      // 以下为超出 7 个的部分，已注释
      // { name: 'UGC广告制作', desc: 'UGC广告工作流' },
      // { name: '私域助手', desc: '职场白领' },
      // { name: '私域搭建工作流', desc: 'B端商家/创业者' },
      // { name: 'AI短剧工作流', desc: '短剧工作流' },
      // { name: 'AI短视频矩阵工作流', desc: 'AI短视频矩阵' },
      // { name: 'IP打造', desc: '品牌IP人设与排期策划' },
      // { name: 'AI教练', desc: 'ai初学者' },
      // { name: '社媒内容转化与分发', desc: 'B端商家/创业者' },
      // { name: '爆款分析', desc: '社媒达人' },
      // { name: '项目评估师', desc: 'B端商家/创业者' },
      // { name: 'prompt优化师', desc: 'ai初学者' }
    ],
    skills: [
      { name: '社媒内容工厂', desc: '公众号/知乎/小红书/Twitter 全平台' },
      { name: '社媒浏览器自动化', desc: '登录态复用操作社交平台' },
      { name: '社媒内容协作', desc: '图文协作/排期管理' },
      { name: '社媒 GIF 动图', desc: '创意内容生产、平台尺寸优化' },
      { name: '社媒热点搜索', desc: '微博热搜/小红书热门/抖音趋势' },
      { name: '社媒实时热点', desc: '腾讯元宝联网搜索、时间范围过滤' },
      { name: '视觉艺术创作', desc: '海报/插画/品牌视觉设计' },
      { name: '商品详情页制作', desc: '详情页卖点结构' },
      { name: 'SOP流程设计师', desc: '社群运营助手' },
      { name: 'AI表格助手', desc: 'AI表格助手（入门）' },
      { name: '知识库AIC工程师', desc: '知识库AIC工程师（轻量）' },
      { name: '财务与经营分析助手', desc: '课程/资料变现助手' },
      { name: 'OKR/KPI拆解教练', desc: '平台/文案写作助手' },
      { name: '品牌合作报价与合同助手', desc: '品牌合作报价与合同助手' }
    ]
  }
];

const currentCategory = ref('student')

const currentAgents = computed(() => {
  const cat = categories.find(c => c.id === currentCategory.value)
  return cat ? cat.agents : []
})

const currentSkills = computed(() => {
  const cat = categories.find(c => c.id === currentCategory.value)
  return cat ? cat.skills : []
})

const switchCategory = (id) => {
  currentCategory.value = id
}
</script>

<style scoped>
.scene-videos {
  background: var(--bg-white);
}

.scene-header {
  background: var(--bg-gradient-light);
  padding: 24px 0;
  text-align: center;
}

.scene-title {
  color: #374151;
  font-size: 24px;
  font-weight: 600;
}

.scene-nav {
  display: flex;
  justify-content: center;
  gap: 32px;
  padding: 32px 0;
}

.nav-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  transition: all 0.3s ease;
}

.nav-btn .el-icon {
  width: 56px;
  height: 56px;
  background: linear-gradient(145deg, #F3F4F6 0%, #E5E7EB 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-secondary);
  transition: all 0.3s ease;
}

.nav-btn.active .el-icon,
.nav-btn:hover .el-icon {
  background: var(--bg-gradient);
  color: white;
}

.nav-label {
  font-size: 14px;
  color: var(--text-secondary);
  font-weight: 500;
}

.nav-btn.active .nav-label {
  color: var(--primary-color);
  font-weight: 600;
}

.scene-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.content-section {
  margin-bottom: 40px;
}

.section-label {
  font-size: 18px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 20px;
  padding-left: 12px;
  border-left: 4px solid var(--primary-color);
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.skills-grid {
  grid-template-columns: repeat(5, 1fr);
}

.card-item {
  background: white;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  border: 1px solid #f0f0f0;
  transition: all 0.3s ease;
}

.card-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  border-color: var(--primary-color);
}

.card-name {
  font-size: 14px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 8px;
}

.card-desc {
  font-size: 12px;
  color: var(--text-secondary);
  line-height: 1.6;
}

.skill-card {
  padding: 14px;
}

.skill-card .card-name {
  font-size: 13px;
}

.skill-card .card-desc {
  font-size: 11px;
}

.more-card {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8fafc;
  border: 1px dashed #d1d5db;
  cursor: default;
}

.more-card:hover {
  transform: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  border-color: #d1d5db;
}

.more-dots {
  font-size: 24px;
  color: #9ca3af;
  letter-spacing: 4px;
  font-weight: bold;
}

.scene-dots {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 40px;
  padding-bottom: 40px;
}

.scene-dots span {
  width: 10px;
  height: 10px;
  background: #E5E7EB;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.scene-dots span.active {
  background: var(--primary-color);
  width: 28px;
  border-radius: 5px;
}

@media (max-width: 1200px) {
  .cards-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .skills-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}

@media (max-width: 900px) {
  .cards-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .skills-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .scene-videos {
    padding: 20px 0;
  }

  .scene-header {
    padding: 20px 0;
  }

  .scene-title {
    font-size: 18px;
    padding: 0 16px;
  }

  .scene-nav {
    gap: 12px;
    padding: 20px 16px;
    overflow-x: auto;
    justify-content: flex-start;
  }

  .nav-btn .el-icon {
    width: 40px;
    height: 40px;
  }

  .nav-label {
    font-size: 11px;
  }

  .scene-content {
    padding: 0 16px;
  }

  .section-label {
    font-size: 16px;
    margin-bottom: 12px;
  }

  .cards-grid,
  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .card-item {
    padding: 10px;
  }

  .card-name {
    font-size: 12px;
  }

  .card-desc {
    font-size: 10px;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .more-card .more-dots {
    font-size: 18px;
  }

  .scene-dots {
    margin-top: 16px;
    padding: 0 0 20px 0;
  }
}

@media (max-width: 480px) {
  .cards-grid,
  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .card-item {
    padding: 8px;
  }

  .card-name {
    font-size: 11px;
    margin-bottom: 4px;
  }

  .card-desc {
    font-size: 9px;
  }

  .more-card .more-dots {
    font-size: 14px;
  }
}
</style>
