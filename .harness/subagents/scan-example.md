# Subagent: {{scan_dimension_name}} 扫描

## 任务

扫描项目源代码{{and_config_if_needed}}，检查是否符合 {{scan_dimension_name}} 规范。逐文件读取并检查，输出违规清单。

## 扫描范围

{{scan_scope_description}}

示例：
- {{project_root}}/{{source_dir}}/ 目录下全部 {{source_file_extension}} 文件
- （可选）{{project_root}}/.gitignore
- （可选）{{project_root}}/{{config_dir}}/{{config_file}}

## 检查规则

{{check_rules_list}}

下面按扫描维度给出示例规则，实际使用时替换为项目自身规则。

---

### 维度一：架构边界扫描

检查项目是否遵守架构分层和模块边界约束。

示例规则：
1. {{ui_layer_name}} 层不直接调用 {{external_api_name}} API -- {{ui_layer_name}} 层不直接调用 {{external_api_name}} API，必须通过 {{coordinator_name}} 或 Manager 间接调用。检查 {{ui_dir}} 目录下的文件是否直接引用 {{api_service_name}}。
2. 全屏页通过 {{app_state_name}} 控制 -- 全屏页面通过 {{app_state_name}}.{{fullscreen_property}} 控制，不在局部视图绕过（{{exception_view}} 例外）。
3. {{special_tab_name}} 不参与 Tab 重置 -- 新增 Tab 重置逻辑时，{{special_tab_name}} 不参与重置。
4. 网络请求设超时 -- 网络请求必须设置超时（默认见 {{timeout_constant_path}}），不允许无限等待。
5. 异步操作非主线程 -- 异步操作（{{async_operation_types}}）必须在非主线程执行，回调结果切回主线程更新 UI。
6. 大数据集合设上限 -- 大数据集合（{{large_collection_examples}}）加载到内存时必须设置条数上限，不允许全量驻留。

---

### 维度二：编码约定扫描

检查项目是否符合团队编码约定。

示例规则：
1. 常量集中管理 -- 新增常量优先写入 {{constants_file_path}}，不要散落在各文件。检查各文件中是否有硬编码的魔法数字或字符串常量。
2. 保留字处理 -- {{reserved_string_value}} 是系统保留字，表示 {{reserved_string_meaning}}，拼接后剔除。检查涉及文本拼接的代码是否正确处理。
3. {{processing_step}} 失败返回空串 -- {{processing_step}} 失败返回 ""，保持索引与 {{data_item}} 一一对应，不压缩数组。
4. 手势识别 -- 新增页面如果有返回按钮，必须同时实现 {{gesture_type}} 手势识别，参数从 {{gesture_constants_path}} 读取。
5. 文档禁止润色 -- 文档内容禁用 emoji、加粗、斜体等润色，使用普通文字。

---

### 维度三：{{domain_specific_dimension}} 规范扫描

检查项目中 {{domain_specific_dimension}} 相关逻辑是否符合规范。

示例规则（以图片/大文件处理为例）：
1. 入队前降采样 -- {{data_item}} 入队前必须降采样到 {{max_size}}（使用 {{downsample_method}}）。不得把原始数据直接存入 {{storage_field}}。
2. 展示时按需加载 -- 展示时按需加载，最大 {{display_max_size}}（使用 {{load_method}}）。
3. 禁止全尺寸解码再缩放 -- 解码必须使用 {{efficient_decode_api}} 直接生成目标尺寸，禁止先解码全尺寸再缩放。
4. 按需加载 + 缓存 -- 禁止一次性全量加载到内存；必须按需加载当前帧，并通过有限缓存（{{cache_class}}）预加载相邻帧。
5. 列表页不加载原数据 -- 列表页只读 {{metadata_file}}，禁止加载 {{full_record_file}} 或原始数据。
6. {{record_file}} 不存二进制 -- {{record_file}} 不存储二进制数据，大文件必须独立存储。
7. 缩略图预生成 -- 缩略图必须在保存时预生成，列表展示时从磁盘加载，不得现场从原始数据生成。
8. 发送到外部 API 的数据降采样 -- 发送到外部 API 的数据必须经过降采样（{{api_max_size}}），不发送原始数据，减少泄露面。

---

### 维度四：日志规范扫描

检查是否符合日志规范。

示例规则：
1. 禁止 {{banned_log_api}} -- 日志输出禁止使用 {{banned_log_api}}，必须统一使用 {{standard_log_api}}。
2. 日志内容禁止润色 -- 日志内容禁用 emoji、加粗、斜体等润色，使用普通文字。
3. 错误信息分两层 -- 面向用户的提示使用自然语言、不含技术细节；面向开发者的日志使用 {{standard_log_api}}，可包含错误码和上下文。
4. 日志禁止输出敏感信息 -- 日志中禁止输出 {{sensitive_field_examples}} 等敏感字段；如需标识，仅输出末四位（如 key=***abcd）。

---

### 维度五：安全规范扫描

检查是否符合安全规范。

示例规则：
1. 密钥只存 {{secure_storage}} -- {{sensitive_credential_types}} 只允许存储在 {{secure_storage}}（通过 {{settings_manager}}），不得硬编码在源码中、不得写入 {{insecure_storage}}、不得写入日志。
2. 密钥日志脱敏 -- 日志中禁止输出 {{sensitive_credential_types}} 等敏感字段；如需标识，仅输出末四位。
3. 配置文件不入库 -- {{secret_config_file}} 已加入 .gitignore，包含密钥的配置文件禁止提交到版本库。
4. 强制 {{secure_protocol}} -- 所有外部 API 调用必须使用 {{secure_protocol}}；不得降级为 {{insecure_protocol}}，不得开启安全例外。
5. API 响应校验 -- API 响应必须校验 HTTP 状态码和数据完整性，不信任未经校验的外部输入。
6. 数据本地存储 -- 用户数据仅存储在设备本地（{{local_storage_path}}），不主动上传到任何服务器（{{exception_apis}} 请求除外）。

---

## 输出格式

```
## {{scan_dimension_name}} 扫描结果

违规数量：N

| # | 文件 | 行号 | 违规项 | 建议修复 |
|---|------|------|--------|---------|
| 1 | ... | ... | ... | ... |

未发现违规的检查项：...
```

---

## 使用说明

1. 复制本文件，将 {{scan_dimension_name}} 替换为具体扫描维度名称（如"架构边界""编码约定""安全规范"等）。
2. 从上方维度示例中选择适用的规则，或编写项目专属规则，填入"检查规则"部分。
3. 替换所有 {{占位符}} 为项目实际值。
4. 每个扫描维度建议独立一个 subagent 文件（如 scan-architecture.md、scan-security.md），也可合并。
5. 删除不需要的维度示例和本使用说明。
