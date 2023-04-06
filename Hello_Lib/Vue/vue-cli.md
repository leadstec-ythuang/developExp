[TOC]

# vue-cli

## 基础命令
### 安装
`npm i -g @vue/cli`

### 项目创建预设
#### 选择features
+ Babel Js编译器，前沿js语法编译成更多浏览器能理解的js
+ CSS Pre-processors CSS预处理器，例如将sass转成css
+ Progressive Web App(PWA) 渐进式网页应用，不是特指某一项技术，而是应用了多项技术的 Web App。其核心技术包括 App Manifest、Service Worker、Web Push，等等。可以将 Web 和 App 各自的优势融合在一起：渐进式、可响应、可离线、实现类似 App 的交互、即时更新、安全、可以被搜索引擎检索、可推送、可安装、可链接。
+ Unit Testing 单元测试 [文档指南](https://lmiller1990.github.io/vue-testing-handbook/zh-CN/setting-up-for-tdd.html#%E5%AE%89%E8%A3%85-vue-cli)，测试的小部件, 通常单元是个函数, 也可能是类或复杂的算法
+ E2E Testing 端到端测试（End to Ent test），与单元测试不同 , 不会将应用分解为更小的部分测试, 而是测试整个应用

#### additional lint features
+ lint on save 保存时跑es-lint，快速获知代码是否存在问题
+ lint and fix on commit commit代码时自动跑es-lint，若不通过则不会commit