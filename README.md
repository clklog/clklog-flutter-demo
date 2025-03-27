# clklog-flutter-demo

# Flutter 神策分析 SDK 插件集成指南

## 引入插件

在 Flutter 项目的 `pubspec.yaml` 文件中的 dependencies 部分添加以下依赖：

```yaml
dependencies:
  # 添加神策 flutter plugin
  sensors_analytics_flutter_plugin: ^3.0.1
```

执行以下命令安装插件：

```bash
flutter pub get
```

## 版本依赖关系

Flutter 插件与 Android 和 iOS 对应的 SDK 版本关系如下：

| Flutter 插件版本 | Android SDK 版本范围 | iOS SDK 版本范围 |
|----------------|-------------------|----------------|
| v2.3.0 及以上 | v6.6.0 及以上 | v4.5.0 及以上 |
| v2.2.0 ~ v2.2.2 | v6.5.1 ~ v6.5.5 | v4.4.6 ~ v4.4.8 |
| v2.1.0 ~ v2.1.1 | v6.0.0 ~ v6.5.0 | v4.0.0 ~ v4.4.5 |
| v1.0.5 ~ v2.0.4 | v4.4.0 ~ v5.4.7 | v2.1.17 ~ v3.1.9 |
| v1.0.0 ~ v1.0.4 | SDK 版本 <= v4.3.7 | SDK 版本 <= v2.1.15 |

## 初始化 SDK

从 Flutter 插件 v2.1.0 版本开始支持在 Flutter 端进行初始化。示例代码如下：

```dart
SensorsAnalyticsFlutterPlugin.init(
    serverUrl: "<#数据接收地址#>",
    enableLog: true,
    visualized: VisualizedConfig(autoTrack: true, properties: true),
    android: AndroidConfig(maxCacheSize: 32 * 1024 * 1024, jellybean: true, subProcessFlush: true),
    ios: IOSConfig(maxCacheSize: 10000));
```

> 注意：通常原生端和 Flutter 端只要初始化一次即可。若选择在 Flutter 端初始化，就不需要再在原生端初始化了。

# ClkLog Flutter 数据采集说明

## 使用须知

在使用 ClkLog 作为 Flutter 应用的数据采集服务时，需要注意以下内容：

ClkLog 的统计数据基于会话ID（$event_session_id）。由于 Flutter SDK 的会话未实现，所以需要自己实现会话ID并配置为全局属性。

这样的实现方式可以确保在 Flutter 应用中准确追踪用户的页面访问行为和会话数据。

# ClkLog Flutter Demo 功能说明

### 1. 会话管理
```dart
void initSensors() async {
    var uuid = Uuid();
    var map = {
      "\$event_session_id": uuid.v4()
    };
    SensorsAnalyticsFlutterPlugin.registerSuperProperties(map);
}
```
- 生成会话ID
- 注册为全局属性

### 2. 用户标识管理
- 获取访客ID（distinctId）
- 用户登录标识（login）
- 用户登出（logout）
- 匿名ID管理（anonymousId）

### 3. 事件追踪
- 激活事件（trackInstallation）
- 自定义事件（track）
- 计时事件（trackTimer相关方法）
- 页面浏览事件（自动采集）

### 4. 用户属性管理
- 设置用户属性（profileSet）
- 设置推送ID（profilePushId）
- 删除推送ID（profileUnsetPushId）

### 5. 数据发送控制
- 设置发送策略（setFlushNetworkPolicy）
- 设置发送间隔（setFlushInterval）
- 设置发送条数（setFlushBulkSize）
- 手动发送数据（flush）

### 6. 调试功能
- 日志开关（enableLog）
- 网络请求开关（enableNetworkRequest）
- 数据清除（deleteAll）

这个 Demo 展示了 ClkLog Flutter SDK 的主要功能和使用方法，可以作为集成参考。