# UniversalStringResources-jetPackCompose

- Whenever possible, you should be using string resources to make sure your app can easily be translated to other languages. However to read a string resource, you need access to the context which you don't have in ViewModels. In this Repo I show you how you can solve this problem.

Book            |
:-------------------------:|
<img src="https://user-images.githubusercontent.com/51374446/152574995-48c5555f-42c8-44d6-8522-36e49fdb3a58.gif" width="200" height="400" /> |

## Create sealed class :
```kotlin
sealed class UiText {
    data class DynamicString(val value: String) : UiText()
    class StringResource(
        @StringRes val resId: Int,
        vararg val args: Any
    ) : UiText()

    @Composable
    fun asString(): String {
        return when (this) {
            is DynamicString -> value
            is StringResource -> stringResource(resId, *args)
        }
    }


    fun asString(context: Context): String {
        return when (this) {
            is DynamicString -> value
            is StringResource -> context.getString(resId, *args)
        }
    }

}
```
```
@StringRes
Denotes that an integer parameter, field or method return value is expected to be a String resource reference (e.g. android.R.string.ok).
```

## In Validate Function :
```kotlin
  UiText.StringResource(resId = R.string.min_name_length_error, MIN_NAME_LENGTH)
```

## In String Res :
```xml
<string name="min_name_length_error">The name must be at least %d characters long</string>
```
