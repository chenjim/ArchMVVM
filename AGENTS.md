# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

Android MVVM 基础框架及常用工具库 (`andlibs`) 配合演示应用 (`demo`)。

## 开发环境

- **Gradle**: 使用 Java 11，避免高版本编译异常
- **Kotlin**: 1.5.21
- **Android Gradle Plugin**: 7.0.0
- **compileSdk**: 30 / **targetSdk**: 28 / **minSdk**: 16

## 构建命令

```bash
./gradlew assembleDebug    # 构建 demo debug APK
./gradlew assembleRelease  # 构建 demo release APK
./gradlew clean             # 清理构建产物
```

## 架构设计

### 核心模块: `andlibs`

提供 MVVM 基础组件，位于 `andlibs/src/main/java/com/chenjim/andlibs/`

**Activity 层**:
- `MvvmActivity<V, VM>` - 基类 Activity，使用 DataBinding + LoadSir 状态管理
  - 抽象方法: `createViewModel()`, `getBindingVariable()`, `getLayoutId()`, `onRetryBtnClick()`
  - 自动绑定 ViewModel，生命周期自动 detachUI
- `IBaseView` - 视图状态接口: `showContent()`, `showLoading()`, `onRefreshEmpty()`, `onRefreshFailure()`

**ViewModel 层**:
- `IMvvmBaseViewModel<V>` - ViewModel 基接口，包含 `attachUI()`, `detachUI()`, `isUIAttached`, `pageView`, `onBack()`

**自定义视图**:
- `BaseCustomView<T, S>` - DataBinding 封装的自定义视图，抽象方法: `viewLayoutId`, `bindingViewModel()`, `onRootClick()`
- `BaseCustomViewModel` - 继承 `BaseObservable` + `Serializable`，供 BaseCustomView 使用

**Fragment**:
- `IBasePagingView` - 分页视图接口，继承 IBaseView

### 常用工具: `andlibs/utils` & `andlibs/face`

- `JsonHelper` - Gson 封装，提供 `parseArray()`, `jsonToMap()`, `mapToJson()` 等静态方法
- `TimeUtils` - 时间处理工具
- `FaceCropper` / `FaceCropperUtils` - 人脸裁剪工具
- `DataMap` - MMKV 数据存储封装
- `Logger` - 日志工具

## 依赖库

| 库 | 用途 |
|---|---|
| Glide 4.11.0 | 图片加载 |
| Lottie 3.4.0 | JSON 动画 |
| EventBus 3.2.0 | 事件总线 |
| RxJava 2.2.19 / RxAndroid 2.1.1 | 响应式编程 |
| LoadSir 1.3.8 | 加载状态页管理 |
| MMKV 1.1.1 | 键值存储 |
| Gson 2.8.6 | JSON 解析 |

## Demo 模块

`demo/src/main/java/com/chenjim/andlibs/demo/` 展示框架用法:
- `MainActivity` - 入口 Activity，继承 `MvvmActivity`
- `MainViewModel` / `MainModel` - 示例 ViewModel 和数据模型
- `ContainerFragment` - Fragment 切换示例

## 发布

`andlibs` 通过 JitPack 发布 (groupId: `com.github.chenjim`, artifactId: `andlibs`, version: `0.0.5`)
