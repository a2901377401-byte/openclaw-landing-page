<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-brand">
          <div class="logo">
            <div class="logo-icon">
              <el-icon :size="24"><Promotion /></el-icon>
            </div>
            <span class="logo-text">Openclaw</span>
          </div>
          <p class="footer-desc">
            让 AI 成为你的数字员工<br>
            今晚就拥有能干的 AI 同事
          </p>
        </div>
        <div class="footer-links">
          <div class="link-group">
            <h4>产品服务</h4>
            <a href="#">数字员工库</a>
            <a href="#">内容矩阵</a>
            <a href="#">本地提效</a>
            <a href="#">企业定制</a>
          </div>
          <div class="link-group">
            <h4>资源</h4>
            <a href="#">使用文档</a>
            <a href="#">API 文档</a>
            <a href="#">案例中心</a>
          </div>
          <div class="link-group">
            <h4>公司</h4>
            <a href="#">关于我们</a>
            <a href="#">联系方式</a>
            <a href="#">隐私政策</a>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>&copy; 2026 Openclaw. All rights reserved.</p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { Promotion } from '@element-plus/icons-vue'
</script>

<style scoped>
.footer {
  background: #1a1a2e;
  color: #fff;
  padding: 60px 0 24px;
}

.footer-content {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 60px;
  margin-bottom: 40px;
}

.footer-brand .logo {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 16px;
}

.logo-icon {
  width: 40px;
  height: 40px;
  background: var(--bg-gradient);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}

.logo-text {
  font-size: 22px;
  font-weight: 700;
}

.footer-desc {
  color: #999;
  font-size: 14px;
  line-height: 1.8;
}

.footer-links {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 40px;
}

.link-group h4 {
  font-size: 15px;
  font-weight: 600;
  margin-bottom: 16px;
  color: #fff;
}

.link-group a {
  display: block;
  color: #999;
  text-decoration: none;
  font-size: 14px;
  margin-bottom: 12px;
  transition: var(--transition);
}

.link-group a:hover {
  color: var(--primary-light);
}

.footer-bottom {
  border-top: 1px solid #333;
  padding-top: 24px;
  text-align: center;
}

.footer-bottom p {
  color: #666;
  font-size: 13px;
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .footer-links {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

@media (max-width: 480px) {
  .footer-links {
    grid-template-columns: 1fr;
  }
}
</style>
