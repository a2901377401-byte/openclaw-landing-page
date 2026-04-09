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
                <el-icon :size="24"><component :is="feature.icon" /></el-icon>
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
              <span class="demo-title">AI 知识库问答</span>
              <span class="demo-badge">实时演示</span>
            </div>
            <div class="demo-chat">
              <div class="chat-message user">
                <div class="message-content">公司的请假流程是什么？</div>
              </div>
              <div class="chat-message ai">
                <div class="message-content">
                  <div class="ai-source">
                    <el-icon><Document /></el-icon>
                    <span>来源：员工手册 v3.2</span>
                  </div>
                  <p>根据公司规定，请假流程如下：</p>
                  <ol>
                    <li>提前 3 天在 OA 系统提交请假申请</li>
                    <li>直属领导审批（1-3 天）</li>
                    <li>部门负责人审批（3 天以上）</li>
                    <li>HR 备案确认</li>
                  </ol>
                </div>
              </div>
            </div>
            <div class="demo-input">
              <el-input placeholder="输入您的问题..." disabled>
                <template #suffix>
                  <el-icon class="input-icon"><Promotion /></el-icon>
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
            <el-icon :size="32"><component :is="source.icon" /></el-icon>
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
    title: '智能文档解析',
    desc: '自动解析 PDF、Word、Excel 等格式，提取关键信息'
  },
  {
    icon: Search,
    title: '语义检索',
    desc: '基于向量检索，精准匹配用户问题与知识内容'
  },
  {
    icon: Connection,
    title: '多源整合',
    desc: '支持网页、数据库、API 等多种数据源接入'
  },
  {
    icon: Folder,
    title: '权限管理',
    desc: '细粒度权限控制，保障企业数据安全'
  }
]

const sources = [
  { icon: Files, name: '本地文档' },
  { icon: Link, name: '网页链接' },
  { icon: ChatDotRound, name: '企微/飞书' },
  { icon: DataAnalysis, name: '数据库' }
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
  background: linear-gradient(135deg, #e8f4fd 0%, #d4e9fc 100%);
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
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

.demo-title {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
}

.demo-badge {
  font-size: 12px;
  color: #67c23a;
  background: rgba(103, 194, 58, 0.1);
  padding: 4px 10px;
  border-radius: 12px;
}

.demo-chat {
  padding: 20px;
  max-height: 300px;
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
  background: rgba(0, 102, 255, 0.1);
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
