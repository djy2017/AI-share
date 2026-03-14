# Figma 设计规范（针对 TalkToFigma MCP 工具集）

## 一、命名规范
- 所有图层必须有语义化名称，禁止使用 Figma 默认名（如 Frame 12、Rectangle 3）。
- 命名格式建议：组件/功能/状态。例如：
    - Button/Primary/Default
    - Card/Order/Header
    - Icon/Search/24px
- 组件变体属性名要清晰，如：Size=Large, State=Hover, Type=Primary。
- 图标图层需标注尺寸：icon-search-16, icon-arrow-24。

## 二、强制使用 Auto Layout
- 所有容器/卡片/列表/按钮必须使用 Auto Layout。
- 明确设置：
    - Direction（Horizontal / Vertical）
    - Gap（元素间距）
    - Padding（上下左右内边距）
    - Align（对齐方式）
- 避免手动拖拽对齐。

## 三、全局 Styles 管理
- 颜色样式（Color Styles）：
    - 品牌色：Primary/500, Primary/400...
    - 功能色：Status/Success, Status/Error, Status/Warning
    - 文字色：Text/Primary, Text/Secondary, Text/Disabled
    - 背景色：Background/Page, Background/Card
    - 边框色：Border/Default, Border/Focus
- 文字样式（Text Styles）：
    - H1 / H2 / H3
    - Body/Large, Body/Medium, Body/Small
    - Caption/Regular, Caption/Bold
    - Label/Form
    - 必须定义：字体、字号、行高、字重、字间距。
- 效果样式（Effect Styles）：
    - Shadow/Card, Shadow/Modal, Shadow/Dropdown

## 四、组件化设计
- 所有可复用 UI 单元必须封装为 Component。
- 使用 Variants 管理多状态。
- 组件内部结构层级不超过 4 层。
- 禁止使用 Detached Instance。

## 五、图片/图标规范
- 图标统一放在专用 Icon 组件库 Frame 内，设置为 Component。
- 图标图层使用 Vector 类型。
- 需导出的图片资源，标记 Export 设置（格式、倍率）。
- 图片容器使用 Frame 包裹，设置 Fill 填充模式。

## 六、层级结构规范
- 页面结构建议：
    ```
    Page
    └── Screen Name (Frame, 固定为设备尺寸)
            ├── Header (Auto Layout Frame)
            ├── Content (Auto Layout Frame)
            │   ├── Section A (Auto Layout Frame)
            │   │   ├── Card (Component Instance)
            │   │   └── Card (Component Instance)
            └── Footer (Auto Layout Frame)
    ```
- 禁止：
    - 多个不相关元素平铺在顶层 Canvas。
    - 使用 Group 替代 Frame。
    - 跨 Frame 的对齐。

## 七、约束与响应式
- 所有元素设置明确的 Constraints（Left/Right/Center/Scale）。
- 撑满宽度的元素设置 Fill container，固定尺寸的用 Fixed。
- 不混用绝对像素值和百分比布局。

## 八、标注与注释规范
- 对交互复杂的组件，使用 Figma Annotation 功能添加说明。
- 状态切换逻辑在 Prototype 面板建立连接。
- 特殊业务逻辑用 Section/Frame 的描述字段说明。

## 九、避免 MCP 无法识别的设计模式
| 设计做法                | 问题                   | 替代方案                |
|-------------------------|------------------------|-------------------------|
| 手动绘制分割线（Line）  | 无布局语义             | 用 Border 或 Auto Layout Gap |
| 直接在 Canvas 放置元素  | 无父容器，无法定位     | 放入 Frame              |
| 使用 Group 堆叠         | 没有 Padding/Gap 属性  | 改为 Auto Layout Frame   |
| 手动调整字间距不存样式  | 只能读到 override 值   | 统一存为 Text Style      |
| 颜色直接填充不关联样式  | 无法归类               | 关联 Color Style        |
| 混用多套字体            | 难以映射到代码变量     | 统一使用 Token 化的 Text Style |

## 十、MCP 读取前的准备工作
- 选中目标 Frame/Section，不要选整个页面。
- 确认 read_my_design 能返回完整的 children 树。
- 用 get_styles 确认所有颜色/文字样式已注册。
- 用 scan_text_nodes 验证所有文本节点名称和内容正确。
- 对图标密集区域用 scan_nodes_by_types 扫描 VECTOR 节点确认数量。

**遵循以上规范，MCP 可提取完整布局语义、样式 Token 和组件映射，极大提升页面还原度。**