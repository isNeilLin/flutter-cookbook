# 使TextField获得焦点

当选择文本字段并接受输入时，通常会获得焦点(“focus”)，用户可以通过点击它们来聚焦文本字段，开发人员可以使用本章中描述的工具来聚焦文本字段。

管理焦点是创建具有简便操作流的表单的基本工具。例如，我们有一个带有文本字段的搜索屏幕。当用户导航到搜索屏幕时，我们可以将搜索项文本字段聚焦。这允许用户在屏幕可见时立即开始键入，而无需手动点击文本字段！

在本章中，我们将学习如何在文本字段可见时就获得焦点，以及如何在点击按钮时聚焦文本字段。

## 在文本字段可见时就获得焦点

为了使文本字段在可见时立即对焦，我们可以使用“`autofocus`”属性。

```dart
TextField(
  autofocus: true,
);
```

## 在点击按钮时聚焦文本字段

我们可能需要在特定的时间点聚焦特定的文本字段，而不是立即聚焦特定的文本字段。在本例中，我们将看到用户按下按钮后如何聚焦文本字段。在现实世界中，您还可能需要根据API调用或验证错误来聚焦特定的文本字段。

### 步骤

1、 创建`FocusNode`

2、 将`FocusNode`传递给一个`TextField`

3、 当点击被按钮时聚焦`TextField`

### 1、 创建`FocusNode`

首先，我们需要创建一个[FocusNode](https://docs.flutter.io/flutter/widgets/FocusNode-class.html)。我们将使用`FocusNode`来识别Flutter的“焦点树”中的特定`TextField`。这将使我们能够在接下来的步骤中聚焦TextField。

由于FocusNode是长期存在的对象，我们需要使用`State`类来管理生命周期。为此，在`State`类的`initState`方法中创建`FocusNode`实例，并在`dispose`方法中清除它们。

```dart
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {
  // 定义FocusNode。为了管理生命周期，在initState中创建FocusNode，在dispose中清除FocusNode
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // 当Form dispose的时候清除focusNode
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // 下一步中完成
  }
}
```

### 2. 将`FocusNode`传递给一个`TextField`

现在我们有了`FocusNode`，我们可以将它传递给`build`方法中特定的`TextField`。

```dart
class _MyCustomFormState extends State<MyCustomForm> {

  @override
  Widget build(BuildContext context) {
    return TextField(
      focusNode: myFocusNode,
    );
  }
}
```

### 3. 当点击被按钮时聚焦`TextField`

最后，当用户点击按钮时，我们想要聚焦文本字段！我们将使用[`requestFocus`](https://docs.flutter.io/flutter/widgets/FocusScopeNode/requestFocus.html)方法来完成此任务。

```dart
FloatingActionButton(
  // 当按下按钮时，flutter使用myFocusNode将文本字段聚焦。
  onPressed: () => FocusScope.of(context).requestFocus(myFocusNode),
);
```

### 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Text Field Focus',
      home: MyCustomForm(),
    );
  }
}

class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {
  // 定义FocusNode。为了管理生命周期，在initState中创建FocusNode，在dispose中清除FocusNode
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // 当Form dispose的时候清除focusNode
    myFocusNode.dispose();

    super.dispose();
  }

   @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Text Field Focus'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // The first text field will be focused as soon as the app starts
            TextField(
              autofocus: true,
            ),
            // The second text field will be focused when a user taps on the
            // FloatingActionButton
            TextField(
              focusNode: myFocusNode,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => FocusScope.of(context).requestFocus(myFocusNode),
        tooltip: 'Focus Second Text Field',
        child: Icon(Icons.edit),
      ), 
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/focus.gif)

