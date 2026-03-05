# 数据与类型边界

## {{primary_record_name}}

{{primary_record_name}}（{{record_source_path}}）：{{record_protocols}}。字段：{{record_fields}}。{{record_storage_note}}。

{{record_status_type}}（{{status_source_path}}）：{{status_enum_definition}}。{{status_compatibility_note}}。

## {{secondary_config_section}}

- {{voice_or_config_type}}（{{config_type_source_path}}）：{{config_type_fields}}。
- 配置：{{config_file_name}} 结构见 {{config_example_path}}；{{settings_manager_name}} 读取并可能被设置页覆盖。

## 协调器输入输出

{{coordinator_name}}：输入 {{coordinator_input}}；进度 {{progress_type}}（{{progress_fields}}）；完成 {{coordinator_output}}。

## 磁盘存储结构

```
{{local_storage_path}}/{id}/
  {{metadata_file}}   -- 轻量摘要，用于列表快速加载
  {{record_file}}      -- 全量字段，但大体积数据字段为空占位
  {{history_file}}     -- {{history_description}}
  {{media_dir_1}}/
    {{media_file_pattern_1}}
    ...
  {{media_file_2}}
  {{thumbnail_file}}
  README.txt
```

加载时由 {{record_manager_name}}.{{load_method}} 从独立文件重组回 {{primary_record_name}} 对象。

## 会话历史（{{history_file}}）

{{history_event_type}}（{{history_source_path}}）：{{history_event_fields}}。

{{history_type}}（{{history_type_source_path}}）：{{history_type_fields}}。{{history_storage_description}}。

{{history_migration_description}}。

## {{config_file_name}} 结构

```json
{{config_json_structure}}
```

{{settings_manager_name}} 优先读 {{runtime_config_path}}，不存在时回退 {{bundle_fallback}}。

## 导出/导入数据结构

```
{{export_dir_pattern}}/
  {{export_manifest_file}}  -- {{export_manifest_description}}
  {{sessions_dir}}/
    {id}/               -- 完整会话目录（同磁盘结构）
  README.txt
```

导入时 ID 重复则跳过。

## {{insecure_storage}} 存储

{{settings_manager_name}} 通过 {{insecure_storage}} 存储非敏感用户配置，键名定义在 {{user_defaults_keys_path}}：
{{user_defaults_keys_list}}

## 边界约定

- UI 层不直接构造/解析 {{external_api_name}} HTTP 请求与响应体。
- 列表只读 {{metadata_file}}；分页通过 {{pagination_method}}；全量 {{full_list_method}} 仅供导出/清空。
- {{legacy_loading_note}}。
