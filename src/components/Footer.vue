<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-brand">
          <div class="logo">
            <img src="/logo2.png" alt="Openclaw" class="logo-img" />
          </div>
          <p class="footer-desc">
            让 AI 成为你的数字员工<br>
            今晚就拥有能干的 AI 同事
          </p>
          <div class="company-info">
            <p class="company-name">广东仨藏</p>
          </div>
        </div>
        <div class="footer-contact">
          <div class="contact-item">
            <h4>联系我们</h4>
            <div class="contact-row">
              <span class="contact-label">电话：</span>
              <a href="tel:your-phone" class="contact-link">13800000000</a>
            </div>
            <div class="contact-row">
              <span class="contact-label">邮箱：</span>
              <a href="mailto:your-email" class="contact-link">123@qq.com</a>
            </div>
          </div>
          <div class="wechat-qr">
            <h4>企业微信</h4>
            <div class="qr-placeholder">
              <img src="/wechat-qr.png" alt="企业微信二维码" class="qr-img" @error="handleQrError" />
            </div>
            <p class="qr-desc">扫码添加企业微信</p>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>&copy; 2026 广东仨藏有限公司. All rights reserved.</p>
      </div>
    </div>
  </footer>
</template>

<script setup>
const handleQrError = (e) => {
  e.target.style.display = 'none'
  e.target.parentElement.innerHTML = '<span class="qr-placeholder-text">请添加二维码图片</span>'
}
</script>

<style scoped>
.footer {
  background: rgb(230, 230, 230);
  color: #333;
  padding: 60px 0 24px;
}

.footer-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  margin-bottom: 40px;
}

.footer-brand .logo {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 16px;
}

.logo-img {
  width: 190px;
  height: 50px;
  object-fit: contain;
}

.logo-text {
  font-size: 22px;
  font-weight: 700;
}

.footer-desc {
  color: #333;
  font-size: 14px;
  line-height: 1.8;
  margin-bottom: 20px;
}

.company-info {
  margin-top: 24px;
}

.company-name {
  color: #333;
  font-size: 14px;
}

.footer-contact {
  display: flex;
  gap: 40px;
  justify-content: flex-start;
}

.contact-item h4,
.wechat-qr h4 {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 16px;
  color: #333;
}

.contact-row {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.contact-label {
  color: #333;
  font-size: 14px;
}

.contact-link {
  color: #333;
  text-decoration: none;
  font-size: 14px;
  transition: opacity 0.3s;
}

.contact-link:hover {
  opacity: 0.6;
  text-decoration: underline;
}

.wechat-qr {
  text-align: center;
}

.qr-placeholder {
  width: 120px;
  height: 120px;
  background: #fff;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 12px;
  overflow: hidden;
}

.qr-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.qr-placeholder-text {
  color: #999;
  font-size: 12px;
}

.qr-desc {
  color: #333;
  font-size: 13px;
}

.footer-bottom {
  border-top: 1px solid #ccc;
  padding-top: 24px;
  text-align: center;
}

.footer-bottom p {
  color: #333;
  font-size: 13px;
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .footer-contact {
    flex-direction: column;
    gap: 32px;
  }

  .wechat-qr {
    text-align: left;
  }

  .qr-placeholder {
    margin: 0;
  }
}
</style>
