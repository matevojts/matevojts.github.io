---
layout: post
title:  "The importance of unit testing"
date:   2021-03-05
excerpt_separator: <!--more-->
---
Are you writing code in TDD? I guess not.

But we all know unit testing can easily catch issues, and they're incredibly cheap in terms of resources, especially time.

I'll show you a case of how.<!--more-->

Please familiarize yourself with the code in *snippet 1* (return types for the calls in comments) and the corresponding test in *snippet 2*.

*Snippet 1.*
```
input.id                                     // PublishSubject<String>
    .flatMapSingle { api.getSomething(it) }  // Single<Value>
    .doFinally { isLoading = false }
    .subscribe(
        { value ->
            render(value)
        },
        { errorEvent.trigger() }
    )
    .addTo(disposable)
```

*Snippet 2.*
```
@Test
fun `Given user has earned something, When screen opened and value loaded, Then loading stopped`() {
    // Given
    every { api.getSomething(any()) } returns Single.just(something)

    // When
    viewModel.onViewResumed()
    viewModel.input.id.accept("")

    // Then
    assertThat(viewModel.isLoading).isFalse
}
```

It turned out that my assertion was not correct. The ViewModel stayed in the loading state somehow, and I didn't understand the reasons.

I assumed that I'm simply making a network call, which is a `Single<T>`, that will signal a Complete event, and the `doFinally` block will catch this and stop the loading.

### Solution

After a quick look, I understood that I'm not facing a `Single`, rather than an `Observable`, which will not complete, and loading will never stop.

I fixed the issue, as shown in *snippet 3*: added stop loading events into more places.

*Snippet 3.*
```
input.id                                     // PublishSubject<String>
    .flatMapSingle { api.getSomething(it) }  // Single<Value>
    .doFinally { isLoading = false }
    .subscribe(
        { value ->
            isLoading = false
            render(value)
        },
        {
            isLoading = false
            errorEvent.trigger() 
        }
    )
    .addTo(disposable)
```

### Conclusion

That's why unit tests can be helpful and are essential: they can easily confirm your assumptions (I'm just doing an API call) or point to issues in your code (loading is not stopped).

#### Hint

Finding out the type of a particular call chain, I often use this approach:
1. Add it to a separate variable - *snippet 4*
2. Press `Ctrl + Q` on Linux or `F1` on Mac to see the type of the variable

*Snippet 4.*

```
val asd =
    input.id
        .flatMapSingle { api.getSomething(it) }
```
