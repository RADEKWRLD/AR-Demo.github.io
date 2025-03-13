# AR_Over-the-Web-Browser

AR_Over-the-Web-Browser 是一个创新的增强现实（AR）应用，旨在通过 Web 浏览器为用户带来沉浸式的 AR 体验。该应用利用了流行的 AR.js 和 A-Frame 框架，使得开发者能够轻松创建和部署 AR 内容，而无需复杂的原生应用开发流程。我们还在其中创建了手势识别组件，状态提示栏。通过这个项目，我们希望能够展示 Web AR 的潜力，并为开发者提供一个易于上手的起点。


---

![image](https://github.com/user-attachments/assets/38e9926b-3abf-44d5-8f0d-e3af0371ad35)


---

## ✨ 核心特性

- **多模式追踪支持**
  - 📍 基于标记的模式 (Pattern Marker)
  - 🖼️ 基于图像的 NFT 模式
- ✋ 自然手势交互
  - 单指拖动旋转模型
  - 双指捏合缩放模型
- 📱 跨平台支持
  - 兼容移动端/PC 浏览器
  - 自动识别设备类型

## 🛠 技术栈

| 框架/库          | 版本   | 用途                  |
|------------------|--------|-----------------------|
| A-Frame          | 1.2.0  | WebXR 场景构建        |
| AR.js            | 3.3.0  | AR 追踪核心           |
| Gesture.js       | Custom | 手势识别逻辑          |

## 🎯 追踪模式对比

### 1. 标记追踪 (Marker Tracking)
```html
<a-marker type="pattern" url="path/to/pattern.patt">
  <a-entity gltf-model="#model"></a-entity>
</a-marker>
```
- **优势**：快速识别、低性能消耗
- **最佳实践**：打印标记尺寸 ≥ 8cm
- **定制图片链接**: https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html

### 2. 图像追踪 (NFT)
```html
<a-nft type="nft" url="path/to/image-set">
  <a-entity gltf-model="#model"></a-entity>
</a-nft>
```
- **优势**：无需物理标记、支持复杂图像
- **准备流程**：需使用 `nft-maker` 生成特征文件
- **定制图片链接**:https://carnaux.github.io/NFT-Marker-Creator/#/
  
## 🤲 手势交互实现

### 核心组件架构
```javascript
AFRAME.registerComponent("gesture-handler", {
  handleRotation(event) {
    // 单指移动计算旋转量
    const rotationFactor = this.data.rotationFactor;
    this.el.object3D.rotation.y += event.detail.positionChange.x * rotationFactor;
  },
  
  handleScale(event) {
    // 双指间距计算缩放比
    const scaleFactor = Math.clamp(
      this.initialScale * (1 + event.detail.spreadChange / startSpread),
      minScale, 
      maxScale
    );
  }
});
```

### 事件处理流程
1. **触摸开始**：建立手势基准点
2. **持续移动**：实时计算位置变化
3. **触摸结束**：重置状态
4. **阈值检测**：过滤微小移动

## 🚀 性能优化

- **深度缓冲配置**
  ```html
  <a-scene renderer="logarithmicDepthBuffer: true;">
  ```
- **模型加载优化**
  - GLB 格式优先
  - 多边形面数 ≤ 50k
  - 纹理尺寸 ≤ 2048px

## 🌍 运行示例

```bash
# 使用本地服务器运行
npx http-server -p 8080
```
访问 `http://localhost:8080/marker.html` 或 `http://localhost:8080/nft.html`

## 📚 开发资源

- [AR.js 官方文档](https://ar-js-org.github.io/AR.js-Docs/)
- [A-Frame 组件开发指南](https://aframe.io/docs/1.2.0/introduction/writing-a-component.html)
- [NFT 目标生成工具](https://carnaux.github.io/NFT-Marker-Creator/)

## 📂 项目文件结构

项目中的 `assets` 文件夹已经内置了所需的模型和 NFT 标记图像，方便开发者直接使用。以下是 `assets` 文件夹中的文件列表：

- earth.fset
- earth.fset3
- earth.glb
- earth.iset
- earth.jpg
- iphone.glb
- pattern-marker A.patt
- pattern-marker A.png
- pattern-marker B.patt
- pattern-marker B.png
- pinball.fset
- pinball.fset3
- pinball.iset
- pinball.jpg
- yellow_duck.glb

这些文件可以直接在项目中引用，无需额外下载或配置。

## 🤝 贡献指南

欢迎通过 Issue 提交：
- 手势交互改进方案
- 新追踪模式适配
- 性能优化建议
