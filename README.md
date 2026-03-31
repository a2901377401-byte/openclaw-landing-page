# Openclaw 落地页

基于 Vue3 + Element Plus 构建的响应式落地页。

## 功能特点

- 完整的 7 大落地页区块
- PC 端和移动端响应式布局
- 科技感设计风格
- Element Plus 组件库

## 安装运行

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build
```

## 项目结构

```
├── src/
│   ├── components/
│   │   ├── Header.vue          # 顶部导航
│   │   ├── Footer.vue          # 页脚
│   │   ├── HeroSection.vue     # 黄金置顶区
│   │   ├── PainPoint.vue       # 痛点放大区
│   │   ├── Convergence.vue     # 需求收敛区
│   │   ├── ProductSection.vue  # 产品对应区
│   │   ├── SocialProof.vue     # 证据锚点区
│   │   ├── FAQSection.vue      # 反对理由消除区
│   │   └── PricingSection.vue  # 价格阶梯
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── index.html
├── vite.config.js
└── package.json
```

## 页面区块

1. **Hero Section** - 黄金置顶区，确定性价值主张
2. **Pain Point** - 痛点放大区，共鸣与现实打击
3. **Convergence** - 需求收敛区，定义确定的结果
4. **Products** - 产品对应区，5大产品线卡片展示
5. **Social Proof** - 证据锚点区，硬数据背书
6. **FAQ** - 反对理由消除区，降低购买门槛
7. **Pricing** - 价格阶梯与最后召唤
