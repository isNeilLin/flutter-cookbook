# 处理文本字段的更改

在某些情况下，每当文本字段中的文本发生变化后运行回调函数是很常见的。例如，我们可能希望构建一个具有自动完成功能的搜索界面。在这种情况下，我们希望根据用户的输入来更新搜索结果。

如何在每次文本更改时运行回调函数？对于Flutter，我们有两个选择：

1. 在`TextField`中提供一个`onChanged`回调函数
2. 使用`TextEditingController`

## 1. 在`TextField`中提供一个`onChanged`回调函数

最简单的方法是向[`TextField`](https://docs.flutter.io/flutter/material/TextField-class.html)提供[`onChanged`](https://docs.flutter.io/flutter/material/TextField/onChanged.html)回调。每当文本更改时，将调用回调。这种方法的一个缺点是它不能与`TextFormField`部件一起工作。

在本例中，每次文本更改时，我们都会将文本字段的当前值打印到控制台。

```dart
TextField(
  onChanged: (text) {
    print("First text field: $text");
  },
);
```

## 2. 使用`TextEditingController`

一种更强大但更精细的方法是提供[`TextEditingController`](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html)作为`TextField`或`TextFormField`的[`controller`](https://docs.flutter.io/flutter/material/TextField/controller.html)属性。

要在文本更改时得到通知，我们可以使用其[`addListener`](https://docs.flutter.io/flutter/foundation/ChangeNotifier/addListener.html)方法侦听控制器。

### 步骤

1、 创建一个`TextEditingController`
2、 将`TextEditingController`应用到一个`TextField`中
3、 创建一个方法来打印文本字段的值
4、 侦听控制器(controller)的更改

### 1. 创建一个`TextEditingController`

首先，我们需要创建一个`TextEditingController`。在接下来的步骤中，我们将向`TextField`提供`TextEditingController`。一旦我们将这两个类连接在一起，我们就可以监听文本字段的更改了！

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

> 注意：当不再需要`TextEditingController`时，请记住释放(`dispose`)它。这将确保我们丢弃对象使用的任何资源。

### 2. 将`TextEditingController`应用到一个`TextField`中

我们还必须向`TextField`或`TextFormField`提供`TextEditingController`。一旦连接好，我们就可以开始监听文本字段的更改。

```dart
TextField(
  controller: myController,
);
```

### 3. 创建一个方法来打印文本字段的值

现在，我们需要一个每次文本更改时都应该运行的函数。在本例中，我们将创建一个打印文本字段当前值的方法。

此方法将留在`_MyCustomFormState`类中。

```dart
_printLatestValue() {
  print("Second text field: ${myController.text}");
}
```

### 4. 侦听控制器(controller)的更改

最后，当文本发生变化时，我们需要侦听`TextEditingController`并运行`_printLatestValue`方法。我们将使用`addListener`方法来完成此任务。

在本例中，我们将在`_MyCustomFormState`类初始化时开始侦听文本字段的更改，并在释放`_MyCustomFormState`时停止侦听。

```dart
class _MyCustomFormState extends State<MyCustomForm> {
  @override
  void initState() {
    super.initState();

    // 开始侦听更改
    myController.addListener(_printLatestValue);
  }
}
```

### 完整示例

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
lass MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {
  // 创建文本控制器。我们将使用它来接收TextField的当前值
  final myController = TextEditingController();

  @override
  void initState() {
    super.initState();

    // 开始侦听更改
    myController.addListener(_printLatestValue);
  }

  @override
  void dispose() {
    // 当Widget从Widget树中移除时，清除控制器
    myController.dispose();
    super.dispose();
  }

  _printLatestValue() {
    print("Second text field: ${myController.text}");
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: <Widget>[
            TextField(
              onChanged: (text) {
                print("First text field: $text");
              },
            ),
            TextField(
              controller: myController,
            ),
          ],
        ),
      ),
    );
  }
}
```


