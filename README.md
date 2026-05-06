[![](https://jitpack.io/v/chenjim/ArchMVVM.svg)](https://jitpack.io/#chenjim/ArchMVVM)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)

# MVVM基础框架及常用工具

Android MVVM 基础框架（Kotlin），由 [MVVM_NEWS](https://f.h89.cn:3456/chenjim/XX_MVVM_NEWS) 项目提取。

## 环境要求

- Gradle 使用 **Java 11**，避免高版本编译异常
- Kotlin 1.5.21 / Android Gradle Plugin 7.0.0

## 引入依赖

```gradle
implementation 'com.github.chenjim:ArchMVVM:0.0.5'
```

或本地模块依赖：

```gradle
implementation project(':andlibs')
```

## 核心组件

### Activity

继承 `MvvmActivity<VB, VM>`，实现：

```kotlin
abstract class MainActivity : MvvmActivity<ActivityMainBindingImpl?, MainViewModel?>() {
    override fun createViewModel() = AndroidViewModelFactory(application).create(MainViewModel::class.java)
    override fun getBindingVariable() = BR.viewModel
    override fun getLayoutId() = R.layout.activity_main
    override fun onRetryBtnClick() {} // LoadSir 重试回调
}
```

### ViewModel

实现 `IMvvmBaseViewModel<V>` 接口，自动绑定生命周期：

```kotlin
class MainViewModel : ViewModel(), IMvvmBaseViewModel<IPageView> {
    override fun attachUI(view: IPageView) { ... }
    override fun detachUI() { ... }
    override val isUIAttached: Boolean get() = ...
    override val pageView: IPageView get() = ...
}
```

### 自定义视图

继承 `BaseCustomView<T, S>`，配合 DataBinding：

```kotlin
class TitleView(context: Context?) : BaseCustomView<ViewtitleBinding?, TitleViewModel?>() {
    override val viewLayoutId = R.layout.viewtitle
    override fun bindingViewModel(data: TitleViewModel?) { dataBinding.viewModel = data }
    override fun onRootClick(view: View?) {}
}
```

## 常用工具

| 工具 | 说明 |
|---|---|
| `JsonHelper` | Gson 封装，JSON 序列化/反序列化 |
| `TimeUtils` | 时间格式化与计算 |
| `FaceCropper` | 人脸检测与裁剪 |
| `DataMap` | MMKV 键值存储封装 |
| `Logger` | 日志工具 |

## 主要依赖

| 库 | 版本 |
|---|---|
| Glide | 4.11.0 |
| Lottie | 3.4.0 |
| EventBus | 3.2.0 |
| RxJava | 2.2.19 |
| LoadSir | 1.3.8 |
| MMKV | 1.1.1 |
| Gson | 2.8.6 |

## 完整示例

完整功能演示见 [MVVM_NEWS](https://f.h89.cn:3456/chenjim/XX_MVVM_NEWS)，包含：
- 多模块架构（app / base / common / network / news）
- Retrofit + RxJava 网络层
- 分页、缓存机制
- SmartRefreshLayout 下拉刷新
