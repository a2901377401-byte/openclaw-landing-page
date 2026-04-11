<template>
  <section class="knowledge-base" id="knowledge">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">企业知识库</h2>
        <p class="section-subtitle">定制 AI 数据源，精准回答每一个问题</p>
      </div>
      <div class="knowledge-content">
        <div class="knowledge-left">
          <div class="feature-list">
            <div class="feature-item" v-for="(feature, index) in features" :key="index">
              <div class="feature-icon">
                <el-icon :size="24">
                  <component :is="feature.icon" />
                </el-icon>
              </div>
              <div class="feature-info">
                <h4>{{ feature.title }}</h4>
                <p>{{ feature.desc }}</p>
              </div>
            </div>
          </div>
        </div>
        <div class="knowledge-right">
          <div class="demo-card">
            <div class="demo-header">
              <span class="demo-title">超级个体数字分身</span>
            </div>
            <div class="demo-chat">
              <div class="chat-message user">
                <div class="message-content">调取上个月关于『AI 创作者经济』的调研结论，并帮我拟定一份给新客户的咨询方案大纲。</div>
              </div>
              <div class="chat-message ai">
                <div class="message-content">
                  <div class="ai-source">
                    <el-icon>
                      <Document />
                    </el-icon>
                    <span>来源：Obsidian 灵感笔记 / 行业洞察.md</span>
                  </div>
                  <p>已为您提取核心洞察，咨询方案大纲已生成：</p>
                  <ol>
                    <li><strong>破局切入：</strong>避开底层模型内卷，聚焦解决垂直场景的“最后一公里”痛点。</li>
                    <li><strong>产品飞轮：</strong>利用“微服务+自动化API”构建轻量化护城河，实现睡后收入。</li>
                    <li><strong>高定体验：</strong>在 UI/UX 层面拉开差距，用极简主义设计提升客单价与品牌溢价。</li>
                    <li><strong>交付物规划：</strong>包含趋势研报、SaaS 架构蓝图及 MVP 上线时间表。</li>
                  </ol>
                </div>
              </div>
            </div>
            <div class="demo-input">
              <el-input placeholder="与您的第二大脑对话..." disabled>
                <template #suffix>
                  <el-icon class="input-icon">
                    <Promotion />
                  </el-icon>
                </template>
              </el-input>
            </div>
          </div>
        </div>
      </div>
      <div class="knowledge-sources">
        <h3 class="sources-title">支持多种数据源接入</h3>
        <div class="sources-grid">
          <div class="source-item" v-for="(source, index) in sources" :key="index">
            <el-icon :size="32">
              <component :is="source.icon" />
            </el-icon>
            <span>{{ source.name }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Document, Folder, Connection, Search, Promotion, Files, Link, ChatDotRound, DataAnalysis } from '@element-plus/icons-vue'

const features = [
  {
    icon: Document,
    title: '认知资产萃取',
    desc: '无视排版与格式壁垒，将碎片化的图文、PDF、研报瞬间转化为你的「第二大脑」养料。'
  },
  {
    icon: Search,
    title: '直觉式语义检索',
    desc: '超越传统关键字。基于深度神经网络的向量引擎，让稍纵即逝的灵感与记忆在毫秒间精准回溯。'
  },
  {
    icon: Connection,
    title: '全栖信息接管',
    desc: '挂载网页、API 与各类生产力工具，为你构建极简且高效的单人商业智能中枢。'
  },
  {
    icon: Folder,
    title: '绝对数据主权',
    desc: '私有沙箱隔离，以最高标准捍卫独立创作者与超级个体的核心商业机密。'
  }
]

const sources = [
  { icon: Files, name: '本地数字资产' },
  { icon: Link, name: '全网信息流' },
  { icon: ChatDotRound, name: '生产力平台 (Notion等)' },
  { icon: DataAnalysis, name: '私有云数据库' }
]
</script>

<style scoped>
.knowledge-base {
  padding: 80px 0;
  background: linear-gradient(180deg, #f8fafc 0%, #ffffff 100%);
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

.knowledge-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  margin-bottom: 60px;
}

.feature-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.feature-item {
  display: flex;
  gap: 16px;
  padding: 20px;
  background: white;
  border-radius: 16px;
  box-shadow: var(--shadow-sm);
  transition: all 0.3s ease;
}

.feature-item:hover {
  transform: translateX(8px);
  box-shadow: var(--shadow-md);
}

.feature-icon {
  width: 48px;
  height: 48px;
  background: linear-gradient(135deg, #F3E8FF 0%, #E9D5FF 100%);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  flex-shrink: 0;
}

.feature-info h4 {
  font-size: 16px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 6px;
}

.feature-info p {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
}

.demo-card {
  background: white;
  border-radius: 20px;
  box-shadow: var(--shadow-lg);
  overflow: hidden;
}

.demo-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  background: linear-gradient(135deg, #FEF3C7 0%, #DBEAFE 100%);
}

.demo-title {
  font-size: 14px;
  font-weight: 600;
  color: #374151;
}

.demo-badge {
  font-size: 12px;
  color: #A855F7;
  background: rgba(168, 85, 247, 0.1);
  padding: 4px 10px;
  border-radius: 12px;
}

.demo-chat {
  padding: 20px;
  overflow-y: auto;
}

.chat-message {
  margin-bottom: 16px;
}

.chat-message.user {
  display: flex;
  justify-content: flex-end;
}

.chat-message.user .message-content {
  background: var(--primary-color);
  color: white;
  border-radius: 16px 16px 4px 16px;
  padding: 12px 16px;
  max-width: 80%;
}

.chat-message.ai .message-content {
  background: #f5f7fa;
  border-radius: 16px 16px 16px 4px;
  padding: 16px;
  max-width: 100%;
}

.ai-source {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 12px;
  color: var(--primary-color);
  margin-bottom: 10px;
  padding: 6px 10px;
  background: rgba(168, 85, 247, 0.1);
  border-radius: 6px;
  width: fit-content;
}

.chat-message.ai p {
  font-size: 14px;
  color: var(--text-primary);
  margin-bottom: 8px;
}

.chat-message.ai ol {
  font-size: 13px;
  color: var(--text-secondary);
  padding-left: 16px;
  margin: 0;
}

.chat-message.ai ol li {
  margin-bottom: 4px;
}

.demo-input {
  padding: 16px 20px;
  border-top: 1px solid #eee;
}

.input-icon {
  color: var(--primary-color);
  cursor: pointer;
}

.knowledge-sources {
  text-align: center;
}

.sources-title {
  font-size: 20px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 24px;
}

.sources-grid {
  display: flex;
  justify-content: center;
  gap: 40px;
  flex-wrap: wrap;
}

.source-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 20px 32px;
  background: white;
  border-radius: 12px;
  box-shadow: var(--shadow-sm);
  transition: all 0.3s ease;
  color: var(--text-secondary);
}

.source-item:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
  color: var(--primary-color);
}

.source-item span {
  font-size: 14px;
  font-weight: 500;
}

@media (max-width: 900px) {
  .knowledge-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .demo-card {
    max-width: 500px;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .knowledge-base {
    padding: 48px 0;
  }

  .section-title {
    font-size: 28px;
  }

  .sources-grid {
    gap: 16px;
  }

  .source-item {
    padding: 16px 24px;
  }
}
</style>
