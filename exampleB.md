## 演示案例2：html2canvas 实现账单导出&打印（功能方案类，模板B）

````markdown
# Vue3 + html2canvas 实现账单DOM转图片、预览、打印功能

## 1. 业务使用场景

四云协同后台管理系统，账单模块需要实现：DOM账单转图片、在线预览、图片下载、浏览器打印、嵌入iframe在线签合同。

## 2. 技术选型 & 设计思路

1. 技术：Vue3 + Vite + html2canvas + 原生浏览器API
2. 设计思路：
   - 统一处理跨域、分辨率问题；
   - 封装通用转换方法，页面直接调用，提升复用性；
   - 增加加载状态，优化用户体验。

## 3. 完整实现步骤

1. 安装依赖：npm install html2canvas
2. 封装通用转换工具函数
3. 页面引入工具，绑定点击事件触发功能

## 4. 核心代码

```javascript
// utils/html2canvas.js 工具封装
import html2canvas from "html2canvas";

/**
 * DOM转图片base64
 * @param {HTMLElement} dom 目标DOM节点
 * @returns {Promise<string>} base64图片地址
 */
export async function domToImage(dom) {
  const canvas = await html2canvas(dom, {
    useCORS: true, // 解决跨域图片
    scale: window.devicePixelRatio, // 提升清晰度
    backgroundColor: "#ffffff", // 固定背景色，防止透明
  });
  return canvas.toDataURL("image/png");
}
```
````

## 5. 调用示例（页面中使用）

```vue
<template>
  <div ref="billDom">账单内容区域</div>
  <button @click="handlePrint">打印账单</button>
</template>

<script setup>
import { ref } from "vue";
import { domToImage } from "@/utils/html2canvas";

const billDom = ref(null);

// 打印功能
const handlePrint = async () => {
  const imgUrl = await domToImage(billDom.value);
  window.print();
};
</script>
```
