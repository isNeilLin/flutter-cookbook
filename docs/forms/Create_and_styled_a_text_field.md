# 创建文本字段并添加样式

文本字段允许用户在我们的应用程序中键入文本。文本字段可以用来构建表单、消息输入框、搜索框等等！在这个章节中，我们将探讨如何创建文本字段并添加样式。Flutter提供了两个文本字段：[TextField](https://docs.flutter.io/flutter/material/TextField-class.html)和[TextFormField](https://docs.flutter.io/flutter/material/TextFormField-class.html)。

## `TextField`

`TextField`是最常用的文本输入部件。默认情况下，`TextField`使用下划线修饰。通过提供[`InputDecoration`](https://docs.flutter.io/flutter/material/InputDecoration-class.html)作为`TextField`的[`decoration`](https://docs.flutter.io/flutter/material/TextField/decoration.html)属性，我们可以添加标签(`label`)、图标(`icon`)、内联提示文本(`hintText`)和错误文本(`errorText`)。若要完全删除装饰(包括下划线和为标签保留的空间)，请显式地将`decoration`设置为空。

```dart
TextField(
  decoration: InputDecoration(
    border: InputBorder.none,
    hintText: 'Please enter a search term'
  ),
);
```

## `TextFormField`

`TextFormField`封装一个`TextField`并将其与封闭表单集成。这提供了其他功能，例如验证和与其他[`FormField`](https://docs.flutter.io/flutter/widgets/FormField-class.html)部件的集成。

```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Enter your username'
  ),
);
```
