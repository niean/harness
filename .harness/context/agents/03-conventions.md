# 约定与约束（实现细节）

本文件是项目规范约定的权威来源，{{agents_file}} "{{agents_section_name}}"中的各节为摘要引用，以本文件为准。{{agents_file}} 中的通用规范不在本文件重复。

---

# 一、UI交互约定

## 页面布局

- 手势：{{gesture_convention_description}}。

## Tab 重置约定

- {{tab_reset_convention_description}}。
- {{tab_reset_exception}}。

---

# 二、编码约定

## {{domain_data_type}} 尺寸规则

- 入队写入：{{ingest_rule}}。
- 播放/查看：{{display_rule}}。
- 缩略图：{{thumbnail_rule}}。
- {{size_constraint_note}}。

## {{processing_step}} 结果处理规则

- {{reserved_value_rule}}。
- {{failure_handling_rule}}。

## 项目本地化配置

- {{localization_config_description}}。

## 配置与常量

所有编译期常量统一收归 {{constants_file_path}}：

- 已有分类：{{existing_constant_categories}}。
- 新增常量优先归入已有分类；不属于任何分类可新建分组，命名 {{naming_convention}}。
- 不允许在业务文件中硬编码魔法值。
- 运行时可变配置通过 {{config_file}} + {{settings_manager_name}} 管理。

---

# 三、质量约定

## 编译规范

- 项目必须零警告（含 IDE 项目配置警告），每次提交前 {{build_command}} 验证。

## 错误处理约定

- 面向用户：{{user_error_convention}}。
- 面向开发者：{{dev_error_convention}}。
- 异步错误回调切回主线程后再更新 UI。
- 网络超时默认 {{default_timeout}}，大文件可用 {{resource_timeout}}，不允许无超时。

## 日志规范

- 禁止 {{banned_log_api}}，统一 {{standard_log_api}}。
- 日志分类定义在 {{log_category_location}}（{{log_categories}}），subsystem 统一 {{log_subsystem}}。
- 级别：{{log_levels}}。
- 日志文本禁用 emoji/加粗/斜体，禁止输出敏感字段。

## 线程与并发约定

- UI 更新必须在主线程。
- {{async_operations}} 在后台线程，完成后切回主线程。
- {{concurrency_approach_description}}。
- 取消：{{cancellation_approach}}。

## 内存与性能约定

{{domain_data_type}} 加载与缓存：
- {{cache_strategy_description}}。
- {{preload_strategy_description}}。
- {{prohibited_loading_pattern}}。

{{domain_data_type}} 解码（{{platform_constraint}}）：
- {{decode_constraint_description}}。
- {{ingest_size_limit}}。

列表页性能：
- {{list_performance_rule}}。
- {{list_thumbnail_rule}}。

{{record_file}} 存储分离：
- {{storage_separation_rule}}。
- {{load_reassembly_rule}}。

调试日志：
- {{debug_log_strategy}}。

## 单元测试约定

- 测试位于 {{test_dir}}，按被测模块命名。
- 新增/修改核心模块时同步补充测试。
- 外部依赖通过协议注入 Mock。
- {{integration_test_note}}。

---

# 四、安全约定

## 密钥存储

- 敏感凭据只通过 {{secure_storage}}（{{settings_manager_name}} 封装），键名在 {{keychain_keys_path}}。
- {{config_file}} 中密钥仅用于首次导入（写入 {{secure_storage}}），运行时从 {{secure_storage}} 读取。
- {{insecure_storage}} 只存非敏感配置，键名在 {{user_defaults_keys_path}}。

## 网络安全

- 所有外部 API 用 {{secure_protocol}}，不降级 {{insecure_protocol}}，不开安全例外。
- API 响应校验 HTTP 状态码和 Content-Type，解码失败按错误处理。
- {{api_data_sanitization_rule}}。

## 数据隐私

- 用户数据仅存设备本地 {{local_storage_path}}，不主动上传。
- {{data_sent_externally_description}}。
- {{export_privacy_note}}。
- {{secret_config_file}} 已加入 .gitignore。
