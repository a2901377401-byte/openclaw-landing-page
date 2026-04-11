<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-brand">
          <div class="logo">
            <img src="/logo2.png" alt="Openclaw" class="logo-img" />
          </div>
          <p class="footer-desc">让 AI 成为你的数字员工，今晚就拥有能干的 AI 同事</p>
          <div class="footer-tags">
            <span class="tag">AI 数字员工</span>
            <span class="tag">企业级部署</span>
            <span class="tag">7×12 支持</span>
          </div>
        </div>
        <!--   暂时不展示-->
        <div></div>
        <div class="footer-contact">
          <div class="contact-card">
            <h4>联系方式</h4>
            <div class="contact-list">
              <div class="contact-row" @click="copyText(CONTACT_INFO.phone, '电话')">
                <el-icon :size="18">
                  <Phone />
                </el-icon>
                <span>{{ CONTACT_INFO.phone }}</span>
              </div>
              <div class="contact-row" @click="copyText(CONTACT_INFO.email, '邮箱')">
                <el-icon :size="18">
                  <Message />
                </el-icon>
                <span>{{ CONTACT_INFO.email }}</span>
              </div>
            </div>
          </div>
          <div class="qr-card">
            <h4>企业微信</h4>
            <div class="qr-box" @click="showQrPreview = true">
              <img src="/wechat-qr.png" alt="企业微信二维码" class="qr-img" @error="handleQrError" />
            </div>
            <p class="qr-tip">扫码添加专属顾问</p>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>&copy; 2026 清远市浩策工程咨询服务有限公司. All rights reserved.</p>
      </div>
    </div>
    <Teleport to="body">
      <div class="qr-preview-overlay" v-if="showQrPreview" @click.self="showQrPreview = false">
        <div class="qr-preview-content">
          <img src="/wechat-qr.png" alt="企业微信二维码" class="qr-preview-img" />
          <p class="qr-preview-tip">扫码添加企业微信</p>
          <button class="qr-close-btn" @click="showQrPreview = false">&times;</button>
        </div>
      </div>
    </Teleport>
  </footer>
</template>

<script setup>
import { ref } from 'vue'
import { Phone, Message } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import CONTACT_INFO from '../config/contact.js'

const showQrPreview = ref(false)

const handleQrError = (e) => {
  e.target.style.display = 'none'
}

const copyText = (text, label) => {
  navigator.clipboard.writeText(text).then(() => {
    ElMessage.success(`${label}已复制：${text}`)
  }).catch(() => {
    const textarea = document.createElement('textarea')
    textarea.value = text
    document.body.appendChild(textarea)
    textarea.select()
    document.execCommand('copy')
    document.body.removeChild(textarea)
    ElMessage.success(`${label}已复制：${text}`)
  })
}
</script>

<style scoped>
.footer {
  background: linear-gradient(180deg, #f0eef5 0%, #e8e5ef 50%, #ddd9e3 100%);
  padding: 60px 0 24px;
}

.footer-content {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 48px;
  margin-bottom: 40px;
  align-items: start;
}

.footer-brand .logo {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 16px;
}

.logo-img {
  width: 170px;
  height: 44px;
  object-fit: contain;
}

.footer-desc {
  color: #555;
  font-size: 15px;
  line-height: 1.7;
  margin-bottom: 20px;
  max-width: 360px;
}

.footer-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  display: inline-block;
  padding: 4px 12px;
  background: rgba(168, 85, 247, 0.08);
  color: #A855F7;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
  border: 1px solid rgba(168, 85, 247, 0.15);
}

.footer-contact {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 32px;
  align-items: start;
}

.contact-card h4,
.qr-card h4 {
  font-size: 15px;
  font-weight: 700;
  margin-bottom: 14px;
  color: #333;
}

.contact-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.contact-row {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #555;
  font-size: 14px;
  padding: 8px 12px;
  border-radius: 8px;
  transition: all 0.2s ease;
  cursor: pointer;
}

.contact-row:hover {
  background: rgba(255, 255, 255, 0.6);
  color: #A855F7;
}

.contact-row .el-icon {
  color: #A855F7;
  flex-shrink: 0;
}

.qr-card {
  text-align: center;
}

.qr-box {
  width: 110px;
  height: 110px;
  background: white;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 10px;
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  cursor: pointer;
  transition: all 0.2s ease;
}

.qr-box:hover {
  transform: scale(1.05);
  box-shadow: var(--shadow-md);
}

.qr-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.qr-img:not([src]),
.qr-img[src=""] {
  display: none;
}

.qr-box:empty::after,
.qr-box:not(:has(.qr-img))::after,
.qr-box>img[style*='display: none']+ ::after {
  content: '二维码';
  color: #ccc;
  font-size: 12px;
}

.qr-tip {
  color: #888;
  font-size: 12px;
}

.footer-bottom {
  border-top: 1px solid rgba(0, 0, 0, 0.08);
  padding-top: 20px;
  text-align: center;
}

.footer-bottom p {
  color: #888;
  font-size: 13px;
}

@media (max-width: 768px) {
  .footer {
    padding: 40px 0 20px;
  }

  .footer-content {
    grid-template-columns: 1fr;
    gap: 36px;
  }

  .footer-contact {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  .qr-card {
    text-align: left;
  }

  .qr-box {
    margin: 0 0 10px 0;
  }
}

.qr-preview-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  animation: fadeIn 0.2s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

.qr-preview-content {
  position: relative;
  background: white;
  border-radius: 20px;
  padding: 32px;
  text-align: center;
  box-shadow: var(--shadow-lg);
  animation: scaleIn 0.25s ease;
}

@keyframes scaleIn {
  from {
    transform: scale(0.85);
    opacity: 0;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}

.qr-preview-img {
  width: 280px;
  height: 280px;
  object-fit: contain;
  border-radius: 12px;
}

.qr-preview-tip {
  margin-top: 16px;
  color: #555;
  font-size: 14px;
}

.qr-close-btn {
  position: absolute;
  top: -12px;
  right: -12px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: #333;
  color: white;
  border: none;
  font-size: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  box-shadow: var(--shadow-md);
}

.qr-close-btn:hover {
  background: #A855F7;
  transform: scale(1.1);
}
</style>
