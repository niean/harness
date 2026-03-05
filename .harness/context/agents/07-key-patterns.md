# 关键代码模式

项目中反复出现但不易从单个文件推断的模式，供新功能实现时参照。

## 模式一：{{cross_module_coordination_name}}

场景：{{cross_coordination_scenario}}。

要点：
1. {{source_view}} 写入 {{root_state_class}} 标志（{{flag_names}}），再设 {{tab_switch_property}}。
2. {{target_view}} 在 .onAppear 和 .onChange(of: {{tab_switch_property}}) 中同时检测并消费（立即置 false）。
3. 用户取消且本次从 {{source_view}} 发起时，自动回 {{source_tab}}。

陷阱：{{cross_coordination_pitfall}}。

## 模式二：{{play_view_name}} 两种打开路径

两种互斥入参：
- {{param_saved}}：已保存记录，onAppear 后台加载，按需加载，禁止全量加载。
- {{param_unsaved}}：未保存的制作中记录，数据已在内存。

均通过 {{presentation_method}} 打开。关闭时调用 {{dismiss_callback}}；{{history_manager}} 在正常播放结束时记录。

播放互斥：{{root_state_class}}.{{play_active_flag}} 全局标志，任意时刻只允许一个记录播放。{{play_trigger_points}} 在打开播放前检查该标志，为 true 时拒绝并记录日志；触发时设 true，{{dismiss_callback}} 中设 false。新增播放触发点必须遵守此防御。

双击手势：{{fullscreen_content_class}} 支持可选 {{double_tap_callback}} 回调。{{play_view_name}} 传入 {{toggle_method}}，双击切换暂停/播放。{{conditional_gesture_extension}}。

滑动控制：{{fullscreen_content_class}} 支持 {{swipe_disabled_param}} 参数。播放时禁止手动左右滑动翻页，暂停时允许。播放自动翻页不受影响。

翻页动画：{{page_transition_animation_description}}。

## 模式三：{{data_type}} 按需加载与缓存

用于 {{play_view_name}} 全屏播放翻页，避免全量加载大数据。

调用链：
- {{fullscreen_content_class}}（{{on_demand_path}}）-> {{on_demand_page_class}}。
- {{on_demand_page_class}}.onAppear：先查 {{cache_class}}（countLimit={{cache_limit}}），命中同步显示；未命中则后台调 {{load_method}}（{{max_dimension}}），完成后主线程动画更新。
- 切页时 {{preload_method}} 预加载前后 {{preload_count}} 张。
- {{load_method}} 内部用 {{efficient_decode_api}} 直接生成目标尺寸。

新增全屏数据浏览应复用 {{fullscreen_content_class}} + {{on_demand_page_class}}。

## 模式四：{{processing_step}} 并发分批处理

{{coordinator_name}}.{{concurrent_method}}：
1. 按 {{concurrency_config}}（{{config_source}}）分批。
2. 每批内用 {{concurrency_api}} 并发执行 {{service_method}}。
3. 失败返回 {{failure_default}}，保持索引对应。
4. 每批完成更新进度（{{progress_allocation}}）。
5. 拼接时剔除 {{reserved_value}}，检查总长度不超 {{max_length_config}}。

扩展 {{processing_step}} 能力应在此函数上修改，不在 UI 层自行实现并发。

## 模式五：{{voice_assistant_name}} 语音触发播放与控制

### 触发播放

流程：
1. {{register_shortcuts_method}} 仅在 {{activation_condition}} 时调用，注册语音短语。
2. {{play_intent_name}} 将 sessionId 写入 {{insecure_storage}}（{{pending_key}}），设 openAppWhenRun=true。
3. {{app_struct_name}} 监听 {{lifecycle_event}} 时调 {{load_pending_method}}。
4. {{load_pending_method}} 读取并清除 {{insecure_storage}} key，后台加载数据，主线程赋值 {{root_state_class}}.{{pending_play_property}}。
5. 根视图的 {{presentation_method}}(item: {{pending_play_property}}) 触发 {{play_view_name}}。
6. 冷启动时启动页未结束则延迟触发。

模糊匹配规则：
{{fuzzy_match_rules}}

陷阱：{{voice_assistant_pitfall}}。

### 播放中控制（暂停/继续/音量）

基于 {{remote_command_center}}，无需自定义意图：

1. {{play_view_name}}.{{start_method}} 调用 {{setup_remote_method}}，注册远程命令。
2. 远程命令处理器通过 {{notification_mechanism}}（{{notification_name}}）发送 action。
3. {{play_view_name}} 通过 {{receive_method}} 监听通知，分发到对应处理方法。
4. {{stop_method}} / {{finish_method}} 调 {{clear_remote_method}}，移除命令处理器。

注意：{{voice_control_limitations}}。

## 模式六：全屏覆盖层（{{fullscreen_property}}）

{{root_state_class}}.{{fullscreen_property}} 控制全屏覆盖，{{root_layout_class}} 根层渲染：

```
{{root_layout_class}} {
    if {{fullscreen_property}} != .loading { {{main_tab_view}} }
    if let kind = {{fullscreen_property}} { {{fullscreen_container}}(kind:) }
}
```

新增全屏场景应新增 {{fullscreen_kind_enum}} case 并在 {{fullscreen_container}} 的 switch 中处理，不要在局部视图绕过。{{play_view_name}} 例外，它通过 {{presentation_method}} 在各 Tab 内弹出。

## 模式七：后台处理（Background {{processing_name}}）

场景：用户发起 {{processing_pipeline}} 后可切换 Tab，处理在后台继续运行，完成后更新记录。

要点：
1. {{source_view}}.{{process_method}} 调用 {{background_manager}}.shared.{{start_method}}({{start_params}})，返回 {{task_id_type}}。
2. {{start_method}} 先创建草稿记录（{{draft_method}}），再创建 {{task_type}}（持有独立 {{coordinator_name}} 实例）启动处理。
3. {{source_view}} 通过 {{observation_mechanism}} 跟踪当前任务，同步进度/结果到本地状态。
4. 任务完成后 {{background_manager}} 在后台调 {{update_method}} 更新记录、保存结果文件、设 {{status_completed}}。
5. 失败时删除草稿记录。
6. 重连：切回对应 Tab 时通过 {{reconnect_mechanism}} 恢复 UI 状态。
7. 列表展示：对 {{in_progress_status}} 记录显示"处理中"标签，禁用操作，仅允许删除。

约束：{{concurrency_constraint}}。
