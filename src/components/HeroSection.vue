<template>
  <section class="hero">
    <div class="hero-bg">
      <div class="hero-circle circle-1"></div>
      <div class="hero-circle circle-2"></div>
      <div class="hero-circle circle-3"></div>
    </div>
    <div class="container">
      <div class="hero-content">
        <div class="hero-badge">
          <el-icon><Star /></el-icon>
          <span>已服务 500+ 企业客户</span>
        </div>
        <h1 class="hero-title">
          把 Openclaw 卖成<span class="highlight">「数字员工岗位」</span><br>
          而不是 AI 工具
        </h1>
        <p class="hero-subtitle">
          不讲概念：今天下单，今晚就在企微/飞书里拥有你的 AI 数字员工同事。
        </p>
        <p class="hero-desc">
          别再追着 AI 更新跑了。你需要的是把每周 2 小时的搬运工作，<br class="pc-only">
          压缩成 2 分钟的自动化流程。
        </p>
        <div class="hero-actions">
          <el-button type="primary" size="large" class="cta-button" @click="goToWechat">
            <el-icon><Promotion /></el-icon>
            立即部署你的第一个数字员工
          </el-button>
          <!-- <el-button size="large" class="secondary-button">
            <el-icon><VideoPlay /></el-icon>
            观看演示
          </el-button> -->
        </div>
        <div class="hero-stats">
          <div class="stat-item">
            <span class="stat-number">500+</span>
            <span class="stat-label">企业客户</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">10K+</span>
            <span class="stat-label">数字员工</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-number">99.9%</span>
            <span class="stat-label">可用率</span>
          </div>
        </div>
      </div>
      <div class="hero-visual">
        <div class="chat-preview">
          <div class="chat-header">
            <div class="chat-avatar">
              <el-icon><User /></el-icon>
            </div>
            <div class="chat-info">
              <span class="chat-name">小 O 助手</span>
              <span class="chat-status">企微数字员工</span>
            </div>
          </div>
          <div class="chat-messages">
            <div class="message bot">
              <p>早上好！今天的早报已生成：</p>
              <div class="message-card">
                <div class="card-header">
                  <el-icon><Document /></el-icon>
                  <span>今日工作摘要</span>
                </div>
                <ul>
                  <li>📊 昨日数据：新增客户 23 家</li>
                  <li>📋 待办事项：3 项</li>
                  <li>⏰ 会议提醒：14:00 周例会</li>
                </ul>
              </div>
            </div>
            <div class="message user">
              <p>帮我整理上周的周报</p>
            </div>
            <div class="message bot">
              <p>已整理完成！这是上周周报要点：</p>
              <div class="message-card">
                <ul>
                  <li>✅ 业绩目标达成 112%</li>
                  <li>✅ 客户满意度 98%</li>
                  <li>📈 环比增长 15%</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
        <div class="floating-card card-1">
          <el-icon><SuccessFilled /></el-icon>
          <span>周报生成完成</span>
        </div>
        <div class="floating-card card-2">
          <el-icon><Clock /></el-icon>
          <span>耗时 2 分钟</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Star, Promotion, VideoPlay, User, Document, SuccessFilled, Clock } from '@element-plus/icons-vue'

const goToWechat = () => {
  window.open('https://work.weixin.qq.com/kfid/your-value', '_blank')
}
</script>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 120px 0 80px;
  position: relative;
  overflow: hidden;
  background: linear-gradient(180deg, #f0f7ff 0%, #fff 100%);
}

.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.hero-circle {
  position: absolute;
  border-radius: 50%;
  opacity: 0.4;
}

.circle-1 {
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, rgba(0, 102, 255, 0.15) 0%, transparent 70%);
  top: -200px;
  right: -100px;
}

.circle-2 {
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(0, 212, 255, 0.12) 0%, transparent 70%);
  bottom: -100px;
  left: -100px;
}

.circle-3 {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(0, 102, 255, 0.1) 0%, transparent 70%);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.hero .container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  position: relative;
  z-index: 1;
}

.hero-content {
  max-width: 600px;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(0, 102, 255, 0.1);
  color: var(--primary-color);
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 24px;
}

.hero-title {
  font-size: 52px;
  font-weight: 800;
  line-height: 1.2;
  margin-bottom: 20px;
  color: var(--text-primary);
}

.hero-title .highlight {
  background: var(--bg-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 22px;
  font-weight: 600;
  color: var(--primary-color);
  margin-bottom: 16px;
  line-height: 1.5;
}

.hero-desc {
  font-size: 16px;
  color: var(--text-secondary);
  line-height: 1.8;
  margin-bottom: 32px;
}

.pc-only {
  display: inline;
}

.hero-actions {
  display: flex;
  gap: 16px;
  margin-bottom: 48px;
}

.cta-button {
  background: var(--bg-gradient) !important;
  border: none !important;
  padding: 16px 32px !important;
  font-size: 16px !important;
  font-weight: 600 !important;
  height: auto !important;
  border-radius: 12px !important;
}

.cta-button .el-icon {
  margin-right: 8px;
}

.secondary-button {
  padding: 16px 32px !important;
  font-size: 16px !important;
  font-weight: 600 !important;
  height: auto !important;
  border-radius: 12px !important;
  border: 2px solid #e0e6ed !important;
}

.hero-stats {
  display: flex;
  align-items: center;
  gap: 32px;
}

.stat-item {
  display: flex;
  flex-direction: column;
}

.stat-number {
  font-size: 32px;
  font-weight: 700;
  color: var(--text-primary);
}

.stat-label {
  font-size: 14px;
  color: var(--text-secondary);
}

.stat-divider {
  width: 1px;
  height: 40px;
  background: #e0e6ed;
}

.hero-visual {
  position: relative;
}

.chat-preview {
  background: white;
  border-radius: 16px;
  box-shadow: var(--shadow-lg);
  overflow: hidden;
}

.chat-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 20px;
  background: linear-gradient(135deg, #0066ff 0%, #00d4ff 100%);
  color: white;
}

.chat-avatar {
  width: 44px;
  height: 44px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
}

.chat-info {
  display: flex;
  flex-direction: column;
}

.chat-name {
  font-weight: 600;
  font-size: 15px;
}

.chat-status {
  font-size: 12px;
  opacity: 0.8;
}

.chat-messages {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.message {
  max-width: 85%;
  padding: 12px 16px;
  border-radius: 12px;
  font-size: 14px;
  line-height: 1.6;
}

.message.bot {
  align-self: flex-start;
  background: #f5f7fa;
  border-bottom-left-radius: 4px;
}

.message.user {
  align-self: flex-end;
  background: linear-gradient(135deg, #0066ff 0%, #00d4ff 100%);
  color: white;
  border-bottom-right-radius: 4px;
}

.message-card {
  background: white;
  border-radius: 8px;
  padding: 12px;
  margin-top: 8px;
  box-shadow: var(--shadow-sm);
}

.message-card .card-header {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 600;
  margin-bottom: 8px;
  color: var(--primary-color);
}

.message-card ul {
  list-style: none;
}

.message-card li {
  padding: 4px 0;
  font-size: 13px;
}

.floating-card {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 8px;
  background: white;
  padding: 12px 20px;
  border-radius: 12px;
  box-shadow: var(--shadow-md);
  font-size: 14px;
  font-weight: 500;
  animation: float 3s ease-in-out infinite;
}

.floating-card .el-icon {
  font-size: 18px;
}

.card-1 {
  top: 20%;
  right: -20px;
  color: #67c23a;
}

.card-2 {
  bottom: 20%;
  left: -20px;
  color: var(--primary-color);
  animation-delay: 1.5s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

@media (max-width: 1024px) {
  .hero .container {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .hero-content {
    max-width: 100%;
    text-align: center;
  }

  .hero-title {
    font-size: 36px;
  }

  .hero-subtitle {
    font-size: 18px;
  }

  .hero-desc {
    font-size: 15px;
  }

  .pc-only {
    display: none;
  }

  .hero-desc br {
    display: none;
  }

  .hero-actions {
    justify-content: center;
  }

  .hero-stats {
    justify-content: center;
  }

  .hero-visual {
    max-width: 500px;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .hero {
    padding: 100px 0 60px;
    min-height: auto;
  }

  .hero-title {
    font-size: 28px;
  }

  .hero-subtitle {
    font-size: 16px;
  }

  .hero-actions {
    flex-direction: column;
  }

  .cta-button,
  .secondary-button {
    width: 100%;
  }

  .hero-stats {
    flex-wrap: wrap;
    gap: 20px;
  }

  .stat-divider {
    display: none;
  }

  .floating-card {
    display: none;
  }
}
</style>
