# 架构与模块边界

## 分层

- 表现层：{{ui_source_dir}}，{{ui_framework}} 视图与 {{secondary_ui_framework}} 封装。主要视图：{{main_views_list}}。
- 业务协调：{{coordinators_dir}}/{{coordinator_name}}，{{coordinator_responsibility}}。
- 能力层：{{handlers_dir}}（{{handler_list}}）、{{managers_dir}}（{{manager_list}}）。{{manager_details}}。
- 系统集成：{{intents_dir}}（{{intent_list}}），{{system_integration_description}}。
- 数据与模型：{{models_dir}}（{{model_list}}）；{{constants_file}}、{{config_file_path}}。
- 预留：{{reserved_dirs}}。

## 模块边界

- UI 不直接调 {{external_api_name}} API，通过 {{coordinator_name}} 调用。
- {{coordinator_name}} 依赖 {{coordinator_dependencies}}。
- {{data_record_name}} 由 {{record_manager_name}} 读写；{{other_managers_responsibilities}}。
- 跨界面状态集中在 {{root_state_class}}，由根视图与各 Tab 共享。
- {{cross_tab_coordination_description}}。
- Tab 导航重置：{{tab_reset_description}}。

## 关键约束

- {{fullscreen_constraint_description}}。
- {{tab_layout_description}}。
- 数据边界：{{data_boundary_description}}。
