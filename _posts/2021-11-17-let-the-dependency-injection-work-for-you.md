---
layout: post
title:  "Let the Dependency Injection work for you"
date:   2021-11-17
excerpt_separator: <!--more-->
---
Recently I received a comment on a code review pointing on this line in one of my ViewModels:

```kotlin
context.getSystemService(Context.DOWNLOAD_SERVICE) as DownloadManager
```

My colleague suggested refactoring it, that the Dependency Injection framework should provide
the proper system service for me.
<!--more-->

## Examples

I'm using [Koin](https://insert-koin.io/) and [Dagger + Hilt](https://dagger.dev/hilt/), so I'll provide a few examples using these frameworks.

#### Dagger + Hilt

Providing

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object AndroidServiceModule {

    @Provides
    @Singleton
    fun provideDownloadManager(context: Context) = context.getSystemService<DownloadManager>()!!
}
```

Usage

```kotlin
@HiltViewModel
class SomeViewModel
@Inject internal constructor(private val downloadManager: DownloadManager) {

    fun downloadRequest(request: DownloadManager.Request) {
        downloadManager.enque(request)
    }
}
```

### Koin

Providing

```kotlin
val androidServiceModule = module {
    single<InputMethodManager> { androidContext().getSystemService()!! }
}
```

Usage

```kotlin
class SomeFragment {

    private val inputMethodManager: InputMethodManager by inject()

    override fun onResume() {
        super.onResume()
        inputMethodManager.toggleSoftInput(InputMethodManager.SHOW_FORCED, 0)
    }
}
```

## Conclusion

This way I could eliminate one more `context` dependency in ViewModels, for example. In addition,
the code is a lot cleaner, you don't have to read long `getWhateverService()` calls.

