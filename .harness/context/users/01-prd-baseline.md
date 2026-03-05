# 产品需求 - 稳定固化(不频繁变更)

## 1. 极简摘要

- 产品：{{product_name}} -- {{core_flow_summary: 一句话描述核心功能链路}}。
- 用户：{{target_users: 目标用户群体与核心使用场景}}。
- 结构：{{app_structure_summary: 概述导航结构、主要Tab/页面组成及其职责}}。
- 核心流程：{{core_flow_steps: 按顺序列出主流程步骤，如"输入 -> 处理 -> 输出 -> 保存 -> 复用"}}。

产品定位、体验原则与判断准则见 context/users/01-prd-sense.md。

---

## 2. 页面结构

{{page_structure_overview: 简述从启动到主界面的层级关系}}

- 全屏容器
    - {{fullscreen_layer_1: 如启动页}}
    - {{fullscreen_layer_2: 如核心内容展示页}}
    - {{fullscreen_layer_3: 如输入采集页}}
- 顶状态栏
- 底导Tab
    - {{tab_1: 如首页}}
    - {{tab_2: 如制作/编辑}}
    - {{tab_3: 如消息/通知}}
    - {{tab_4: 如我的/设置}}
        - {{tab_page_layout: 描述Tab内部页面通用布局，如顶导+手势区+内容}}

---

## 3. 功能需求

### 3.1 全屏
- {{fullscreen_behavior: 描述哪些页面/场景使用全屏展示，以及全屏时对导航栏的影响}}

### 3.2 底导
- {{bottom_nav_behavior: 列出底导Tab项，说明全屏场景下底导的显隐规则}}

### 3.3 {{tab_1_name: 如首页}}
- 顶导：{{tab1_top_nav: 标题及导航元素}}。
- {{tab1_entry_area: 核心操作入口区域描述}}。
- {{tab1_content_area: 主内容区域描述，如记录列表、信息流等}}。
- {{tab1_interaction: 关键交互说明，如点击记录后的行为}}。

### 3.4 {{tab_2_name: 如制作/编辑}}
- 顶导：{{tab2_top_nav: 标题及操作按钮}}。
- {{tab2_core_function: 核心功能描述，包括输入方式、处理流程、输出方式}}。
- {{tab2_preview: 预览/播放功能描述}}。
- {{tab2_background: 后台处理能力描述及约束}}。

### 3.5 {{tab_3_name: 如消息/历史}}
- {{tab3_sections: 分组入口列表}}。
- {{tab3_section_detail_1: 第一个分组的详细规则，包括展示、排序、过滤、存储方式}}。
- {{tab3_section_detail_2: 第二个分组的详细规则}}。

### 3.6 {{tab_4_name: 如我的}}
- {{tab4_profile: 顶部用户信息区域描述}}。
- {{tab4_menu: 菜单项列表及各项功能说明}}。

### 3.7 其它
- 配置：{{config_description: 配置文件位置及设置页说明}}。
- {{extra_feature: 其它特色功能描述，如语音控制、快捷指令等}}。

---

## 4. 非功能与约束

- 平台：{{platform: 如iOS/Android/Web，设备类型，屏幕方向}}。
- 体验：{{ux_constraints: 核心体验约束，如流畅性、状态分离等}}。
- 数据：{{data_constraints: 数据导入导出、去重、存储等规则}}。
- {{other_constraint: 其它约束，如互斥规则、并发限制等}}。

---

## 5. 版本与需求池

- 备选需求池 context/users/01-prd-specs.md
- 版本历史见 {{changelog_path: 版本变更日志文件路径}}
