<template>
  <section class="merged-section">
    <div class="container">
      <div class="split-layout">
        <div class="pain-column">
          <h2 class="column-title">「效能黑洞」：AI 落地的五重隐形阻力</h2>
          <div class="pain-list">
            <div class="pain-item" v-for="(pain, index) in painPoints" :key="index">
              <div class="pain-icon">
                <el-icon :size="24">
                  <component :is="pain.icon" />
                </el-icon>
              </div>
              <div class="pain-content">
                <h3>{{ pain.title }}</h3>
                <p>{{ pain.desc }}</p>
              </div>
            </div>
          </div>
        </div>
        <div class="result-column">
          <h2 class="column-title">「范式跃迁」：新一代数字生产力引擎</h2>
          <div class="result-list">
            <div class="result-item" v-for="(result, index) in results" :key="index">
              <div class="result-number">{{ index + 1 }}</div>
              <div class="result-content">
                <h3 style="display: inline-block; margin-right: 4ch; margin-top: 10px;">{{ result.title }}</h3>
                <span class="result-check" style="display: inline-block;">{{ result.check }}</span>
                <p>{{ result.desc }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Lightning, Clock, Briefcase, Timer, Lock, Notebook, Tools, Files } from '@element-plus/icons-vue'

const painPoints = [
  { icon: Lightning, title: ' 追新内耗 ', desc: ' 每天追着新工具新趋势内耗不断 ' },
  { icon: Clock, title: ' 时间被撕碎 ', desc: ' 职场人被各种琐事撕碎宝贵时间 ' },
  { icon: Briefcase, title: ' 体力换钱死循环 ', desc: ' 接单靠堆时长陷入体力换钱死循环 ' },
  { icon: Tools, title: ' 落地断层 ', desc: ' 买了 AI 工具会员却无法真正落地 ' },
  { icon: Files, title: ' 能力孤岛 ', desc: ' 每次项目从零开始经验难以积累 ' }
];

const results = [
  { icon: Timer, title: '24 小时内部署', desc: ' 拥有能在企微专属快速落地的能力 ', check: ' 今晚下单，明天就能用 ' },
  { icon: Lightning, title: ' 端到端自动化 ', desc: ' 不只是聊天能实现端到端自动化 ', check: ' 从想法到交付，真正省人 ' },
  { icon: Notebook, title: ' 一次搭建，N 次复用 ', desc: ' 市场运营等场景可反复多次复用 ', check: ' 一次投入，长期回报 ' },
  { icon: Lock, title: ' 本地独立运行 ', desc: ' 企业级沙盒保障数据安全不出域 ', check: ' 数据不出域，安全更放心 ' },
  { icon: Lock, title: ' 组织能力升级 ', desc: ' 每个数字员工沉淀组织核心能力 ', check: ' 经验留在公司，而非个人 ' }
];
</script>

<style scoped>
.merged-section {
  padding: 80px 0;
}

.split-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  min-height: 600px;
}

.column-title {
  font-size: 23px;
  font-weight: 700;
  margin-bottom: 20px;
  color: white;
  padding-bottom: 12px;
  padding-top: 16px;
  border-bottom: 2px solid rgba(255, 255, 255, 0.3);
  letter-spacing: 0.5px;
  text-align: center;
  width: 100%;
}

.pain-column {
  position: relative;
  padding: 10px;
  background-image: url('/痛点.png');
  background-size: cover;
  background-position: right;
  border-radius: 24px;
  overflow: hidden;
  min-height: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pain-column::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(60, 30, 30, 0.6) 0%, rgba(40, 15, 15, 0.8) 100%);
  z-index: 0;
}

.result-column {
  position: relative;
  padding: 10px;
  background-image: url('/结果.png');
  background-size: cover;
  background-position: right;
  border-radius: 24px;
  overflow: hidden;
  min-height: 600px; /* 修改：改为min-height确保内容完全显示 */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.result-column::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(25, 45, 65, 0.6) 0%, rgba(15, 35, 55, 0.8) 100%);
  z-index: 0;
}

.pain-column > *,
.result-column > * {
  position: relative;
  z-index: 1;
}

.pain-list,
.result-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 100%;
}

.pain-item {
  display: flex;
  gap: 16px;
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 16px;
  transition: all 0.3s ease;
  padding: 15px;
}

.pain-item:hover {
  background: rgba(255, 255, 255, 0.14);
  transform: translateX(6px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

.pain-icon {
  width: 56px;
  height: 56px;
  margin: 0;
  background: linear-gradient(135deg, #ff6b6b 0%, #ffa500 100%);
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
  box-shadow: 0 6px 16px rgba(255, 107, 107, 0.4);
}

.pain-icon .el-icon {
  font-size: 28px;
}

.pain-content h3 {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
  color: #ffffff;
  letter-spacing: 0.3px;
}

.pain-content p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.8;
}

.result-item {
  display: flex;
  gap: 20px;
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 16px;
  transition: all 0.3s ease;
  padding: 15px;
}

.result-item:hover {
  background: rgba(255, 255, 255, 0.14);
  transform: translateX(6px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

.result-number {
  width: 42px;
  height: 42px;
  margin: 0;
  background: linear-gradient(135deg, #A855F7 0%, #C084FC 100%);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 18px;
  flex-shrink: 0;
  box-shadow: 0 6px 16px rgba(168, 85, 247, 0.5);
}

.result-content h3 {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
  color: #ffffff;
  letter-spacing: 0.3px;
  display: block; /* 移动端显示调整 */
  margin-right: 0;
}

.result-content p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.8;
  margin-bottom: 12px;
}

.result-check {
  display: inline-block;
  background: linear-gradient(135deg, #A855F7 0%, #C084FC 100%);
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  box-shadow: 0 3px 10px rgba(168, 85, 247, 0.4);
  letter-spacing: 0.3px;
  margin-top: 8px;
}

@media (max-width: 768px) {
  .merged-section {
    padding: 40px 0;
  }

  .split-layout {
    grid-template-columns: 1fr;
    gap: 20px;
    min-height: auto;
  }

  .column-title {
    font-size: 20px;
    margin-bottom: 16px;
    padding-bottom: 10px;
    line-height: 1.3;
  }

  .pain-column,
  .result-column {
    padding: 24px 20px;
    min-height: auto;
    max-height: none; /* 移除最大高度限制 */
  }

  .pain-list,
  .result-list {
    gap: 12px;
  }

  .pain-item,
  .result-item {
    gap: 14px;
    padding: 18px;
    flex-direction: row; /* 保持水平布局 */
  }

  .pain-icon,
  .result-number {
    width: 48px;
    height: 48px;
    flex-shrink: 0;
  }

  .pain-icon .el-icon {
    font-size: 26px;
  }

  .result-number {
    font-size: 17px;
  }

  .pain-content h3,
  .result-content h3 {
    font-size: 16px;
    margin-bottom: 6px;
    line-height: 1.4;
  }

  .pain-content p,
  .result-content p {
    font-size: 13px;
    line-height: 1.6;
    margin-bottom: 8px;
  }
  
  .result-check {
    font-size: 12px;
    padding: 5px 12px;
    margin-top: 8px;
  }
  
  /* 针对移动设备优化 */
  .result-content h3 {
    display: block;
    margin-right: 0;
  }
}
</style>
