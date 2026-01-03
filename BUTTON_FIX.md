# 🔧 按钮可见性修复

## 问题
操作按钮（预览和生成PDF）的文字不够清晰可见。

## 解决方案

### 1. 增强按钮样式
- ✅ 增加按钮内边距（py-3 → py-3.5）
- ✅ 加深背景色（bg-gray-600 → bg-gray-700）
- ✅ 添加阴影效果（shadow-md）
- ✅ 增强字体粗细（font-medium → font-semibold）
- ✅ 增大图标尺寸（w-4 h-4 → w-5 h-5）
- ✅ 添加悬停阴影（hover:shadow-lg）

### 2. 按钮文字包装
使用 `<span>` 标签包装按钮文字，确保样式正确应用：
```vue
<span>{{ showPreview ? '隐藏预览' : '预览销售单' }}</span>
```

### 3. CSS 增强
添加了额外的 CSS 规则确保按钮文字可见：
```css
button span {
  color: inherit;
  font-weight: 600;
}

button:not(:disabled) {
  cursor: pointer;
}

button:focus-visible {
  outline: 2px solid #14b8a6;
  outline-offset: 2px;
}
```

### 4. "添加产品"按钮优化
- ✅ 添加边框（border-2 border-primary-300）
- ✅ 增加阴影（shadow-sm）
- ✅ 增强悬停效果

## 效果

现在所有按钮都具有：
- 🎨 更深的背景色，提高对比度
- 📏 更大的尺寸，更容易点击
- ✨ 阴影效果，增强立体感
- 💪 加粗字体，更清晰可读
- 🎯 明确的悬停和点击反馈

## 测试
刷新浏览器页面 `http://localhost:3000`，你应该能清楚地看到：
1. 灰色的"预览销售单"按钮
2. 翡翠绿色的"生成 PDF"按钮
3. 浅绿色带边框的"添加产品"按钮

所有按钮的文字和图标都应该清晰可见！
