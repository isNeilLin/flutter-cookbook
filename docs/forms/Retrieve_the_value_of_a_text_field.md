# 获取文本字段的值

在本章中，我们将看到如何获取用户在文本字段中键入的文本。

## 步骤

1、 创建一个`TextEditingController`
2、 将`TextEditingController`应用到一个`TextField`中
3、 显示文本字段的当前值

## 1. 创建一个`TextEditingController`

为了获取用户输入文本字段的文本，我们需要创建一个[`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html)。然后，我们将在接下来的步骤中将`TextEditingControlle`提供给`TextField`。

一旦向`TextField`或`TextFormField`提供了`TextEditingController`，我们就可以使用它获取用户在该文本字段中键入的文本。

> 注意：当不再需要`TextEditingController`时，请记住释放(`dispose`)它。这将确保我们丢弃对象使用的任何资源。

```dart
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {
  // 创建文本控制器。我们将使用它来接收TextField的当前值
  final myController = TextEditingController();

  @override
  void dispose() {
    // 当Widget从Widget树中移除时，清除控制器
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // 下一步中完成
  }
}
```

## 2. 将`TextEditingController`应用到一个`TextField`中

现在我们有了要使用的`TextEditingController`，我们需要将其连接到特定的文本字段。为此，我们将`TextEditingController`提供给`TextField`或`TextFormField`部件作为`controller`属性。

```dart
TextField(
  controller: myController,
);
```

## 3. 显示文本字段的当前值

在向文本字段提供了`TextEditingController`之后，我们就可以开始读取值了！我们将使用`TextEditingController`提供的[`text`](https://docs.flutter.io/flutter/widgets/TextEditingController/text.html)方法来获取用户在文本字段中键入的文本字符串。

在本例中，当用户点击操作按钮时，我们将使用文本字段的当前值显示一个alert对话框。

```dart
FloatingActionButton(
  // 当用户点击操作按钮时，使用文本字段的当前值显示一个alert对话框。
  onPressed: () {
    return showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          // 使用`TextEditingController.text`获取用户输入的值
          content: Text(myController.text),
        );
      },
    );
  },
  tooltip: 'Show me the value!',
  child: Icon(Icons.text_fields),
);
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Retrieve Text Input',
      home: MyCustomForm(),
    );
  }
}

class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {
  // 创建文本控制器。我们将使用它来接收TextField的当前值
  final myController = TextEditingController();

  @override
  void dispose() {
    // 当Widget从Widget树中移除时，清除控制器
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: myController,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // 当用户点击操作按钮时，使用文本字段的当前值显示一个alert对话框。
        onPressed: () {
          return showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // 使用`TextEditingController.text`获取用户输入的值
                content: Text(myController.text),
              );
            },
          );
        },
        tooltip: 'Show me the value!',
        child: Icon(Icons.text_fields),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/retrieve-input.gif)
