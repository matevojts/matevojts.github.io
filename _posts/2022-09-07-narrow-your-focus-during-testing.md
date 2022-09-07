---
layout: post
title:  "Narrow your focus during testing"
date:   2022-09-07
excerpt_separator: <!--more-->
---

Recently, I wrote the first UI test for an Android app. The first one is often way longer than the others, as you need to set up many things which can be reused later. The infinite amount of time required is especially true if you overcomplicate the test case and want to test too many things in the first simple run, as I did. Let me show you how.<!--more-->

## Purpose

The app's primary purpose is to manage phone calls, with a bit more functionality than the built-in caller apps.

## Settings and permissions

The app needs two critical settings enabled:

1. It should have permission to read the contacts,
2. It should be the default dialer app on the device.

I'm checking both of these settings during the startup of the application. More precisely, in `onResume` in `MainActivity`. Any of them missing will automatically show a dialog to give the permission or the default dialer role, like this:

<img src="https://matevojts.github.io//assets/images/contact_permission_dialog.jpg" style="display: block; margin: auto;" />

<img src="https://matevojts.github.io//assets/images/dialer_request_role_dialog.jpg" style="display: block; margin: auto;" />

## Base test

The first version of the test file was like this:

*Snippet 1*

```kotlin
@RunWith(AndroidJUnit4::class)
class RecentScreenTest {

    @get:Rule
    val activityRule = ActivityScenarioRule(MainActivity::class.java)

    @Test
    fun asd() {
        onView(withId(R.id.mainDialerButton)).check(matches(isDisplayed()))
    }
}
```

Having a fresh app for every single test case is a crucial part of these tests means that each test will start with the two dialogs described above to gain the necessary settings and permissions.

However, I just wanted to understand if the first screen is displayed containing my dialer button.

Let's hide the dialogs!

## Contact permission dialog

Hiding the contact permission panel is relatively straightforward. You need to add a rule to your test class to provide the permission you want for the test cases.

*Snippet 2*

```kotlin
@get:Rule
val grantPermissionRule: GrantPermissionRule =
        GrantPermissionRule.grant(android.Manifest.permission.READ_CONTACTS)
```

It's important to mention that this rule changes the app's behavior only for the UI tests; it does not modify the app's usual way of working.

## Dialer role request dialog

In the beginning, I started to look for some rule, or built-in solution, like the above, for the permission. Unfortunately, I didn't find anything where you don't need manual interaction to have the dialer role.

Manual interaction??? That's not what I want to see in an automated testing solution. Then, during ideation, I updated my test case like this:

*Snippet 3*

```kotlin
@Test
fun givenOnRecentScreen_WhenScreenIsLoaded_ThenDialerButtonDisplayed() {
    onView(withId(R.id.mainDialerButton)).check(matches(isDisplayed()))
}
```

I hope you realize the difference: I named the test method appropriately. Using the `given-when-then` keywords will guide your focus on what you exactly want to test and what not.

In my case, I want to test that after the screen buildup flow, I can see the dialer button. And guess what not: I don't want to test the dialer role request dialog.

This revelation helped to change my mind and focus on mocking the dialer role request data, as I don't want to play with that right now.

## Source of the data

The end of the dialer request data is this file for me:

*Snippet 4*

```kotlin
package com.myapp.repository

class DefaultDialerRepository(private val roleManager: RoleManager) {

    private var isDefaultDialer: MutableStateFlow<Boolean> =
            MutableStateFlow(roleManager.isRoleHeld(RoleManager.ROLE_DIALER))

    suspend fun refresh() {
        this.isDefaultDialer.emit(roleManager.isRoleHeld(RoleManager.ROLE_DIALER))
    }

    fun isDefaultDialer(): Flow<Boolean> = isDefaultDialer
}
```

I wanted to replace this behavior only for the UI tests. Fortunately, it's easily achievable by creating a copy of the file in the appropriate package with the desired content, like this:

*Snippet 5*

```kotlin
package com.myapp.repository

class DefaultDialerRepository(private val roleManager: RoleManager) {

    suspend fun refresh() {
        // not needed
    }

    fun isDefaultDialer(): Flow<Boolean> = MutableStateFlow(isDefaultDialer)

    companion object {
        private var isDefaultDialer: Boolean = true

        fun setIsDefaultDialer(isDefaultDialer: Boolean) {
            this.isDefaultDialer = isDefaultDialer
        }
    }
}
```

Please note that both files have the same package: `package com.myapp.repository`

On the other hand, if you compare the path of the two Repository files, you will see the difference:

*Snippet 6*

```kotlin
app / src / main / kotlin / com / myapp / repository / DefaultDialerRepository.kt
app / src / androidTest / kotlin / com / myapp / repository / DefaultDialerRepository.kt
```

As the file names and the packages are the same for the files, during compilation for the UI tests, the original file in the `main` package will be replaced by the other one (in *snippet 5*).

This way, during UI testing, the app will behave like it has the dialer role (as I set it to true by default in *snippet 5*), so it will not show the request dialog for us.

## Conclusion

Knowing that you can replace files during testing is just pure experience, in my opinion. You'll learn these kinds of tricks daily if you are curious about your work.

However, narrowing your focus is what really helps in these situations: a well-named method or variable will help you test the crucial parts and forget about the others (e.g., testing if the dialer request role appears). At least for this time.
