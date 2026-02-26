# 景观设计文本排版工具

## 项目目标
帮助景观设计师快速生成、整理设计文案，并以1:1预览InDesign页面版式，最终导出 IDML 文件供 InDesign 2024 直接打开。

## 当前进度
- [x] 项目初始化
- [x] 基础框架（A3横向预览 + 左侧控制面板）
- [x] 第一章：前期分析（区位/基地/建筑，AI提取功能，支持粘贴/上传/截图三种输入）
- [x] AI对话式修改（对话区 + 一键应用到预览）
- [x] 第二章：设计构思（AI生成3版本，选择确认）
- [x] 图片占位符（点击上传本地图片）
- [x] 导出按钮：RTF → 改为 IDML（InDesign 2024 原生格式，可直接打开）
- [x] 对话回复底部新增「一键应用到全部三页」按钮，可反复点击替换
- [x] IDML 导出功能（用自建 ZIP 构建器替代 JSZip，确保 extra field=0，待测试）

## 使用方法
1. 用浏览器（Chrome）直接打开 `index.html`
2. 点击右上角「设置」，填入 DeepSeek API Key
3. 第一章：粘贴原始文本 → 点击「AI一键分析」→ 点「一键应用到全部三页」→ 应用到右侧预览
4. 第二章：填写项目信息 → 「生成三个设计构思版本」→ 选择 → 确认 → 应用到预览
5. 点击图片占位符可上传本地图片
6. 点击「导出 IDML → InDesign」→ 下载 `.idml` 文件 → 在 InDesign 中 File → Open 打开

## 页面规格（已内置）
- 页面尺寸：A3 横向 420×297mm
- 页边距：四边均为 20mm
- 共6页：区位分析、基地分析、建筑分析、灵感来源、设计理念、主题落位

## 文件结构
```
index.html      主工具文件（直接用浏览器打开，单文件包含所有功能）
README.md       本文档
CLAUDE.md       AI助手配置
```

## IDML 调试记录（已修复的问题）
1. `designmap.xml` 含非法属性 `TransparencyAttributeDefaultProperty="..."` → 已删除
2. `DOMVersion="20.0"` 超出 InDesign 2024 支持范围 → 改为 `"19.0"`
3. `<Br/>` 段落符位置错误（应在 CharacterStyleRange 内）→ 已修复
4. 同一 Story 被多个文字框引用 → 已改为每页一个文字框对应一个 Story
5. `designmap.xml` 缺少 `StoryList` 属性和 `<idPkg:MasterSpread>` 引用 → 已补全
6. `Graphic.xml` 为空，颜色引用未定义 → 已添加 CMYK 颜色定义
7. 图片框含非法 `<Image>` 子节点 → 已移除
8. 删除了 MasterSpread（最大化简化结构）→ 改用 `AppliedMaster="n"`
9. `Styles.xml` 根样式缺 `BasedOn` 属性，缺 TableStyle/CellStyle 根组 → 已补全

## 待完成
- [x] 确认 IDML 可在 InDesign 2024 正常打开（2026-02-26 测试通过）
- [ ] 梳理并优化页面逻辑（用户反馈当前逻辑不理想，待用户提供需求）
- [ ] 支持自定义页面模板布局
- [ ] 增加更多分析页面类型（竖向版式、图表页等）
- [ ] 增加项目保存/读取功能（本地存储）
