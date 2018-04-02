# 处理点击事件

我们不仅要向用户展示信息，还要让用户与我们的应用程序交互！那么，我们如何应对诸如点击和拖拽等事件？我们要用[`GestureDetector`](https://docs.flutter.io/flutter/widgets/GestureDetector-class.html)Widget！

假设我们想做一个自定义按钮，当点击时显示一个`snackBar`。我们怎么处理这个？

## 指南

1、创建按钮

2、将其包装在`GestureDetector`中，并使用`onTap`回调

```dart
new GestureDetector(
  child: new Container(
    padding: new EdgeInsets.all(12.0),
    decoration: new BoxDecoration(
      color: Theme.of(context).buttonColor,
      borderRadius: new BorderRadius.cicular(8.0)
    ),
    child: new Text('myButton')
  ),
  // 当child被点击, 显示snackbar 
  onTap: (){
    final snackBar = new SnackBar(content: new Text('Tap'));
    Scaffold.of(context).showSnackBar(snackBar);
  }
)
```

## 注意

1、如果你想在你的按钮上添加Material波纹效果，请查看[添加Material波纹](https://github.com/isNeilLin/flutter-cookbook/blob/master/docs/handling_gestures/Adding_Material_Touch_ripples.md)。

2、虽然我们已经创建了一个自定义按钮来演示这些概念，但Flutter已经包含了一些按钮，其中包括：[`RaisedButton`](https://docs.flutter.io/flutter/material/RaisedButton-class.html), [`FlatButton`](https://docs.flutter.io/flutter/material/FlatButton-class.html), 和 [`CupertinoButton`](https://docs.flutter.io/flutter/cupertino/CupertinoButton-class.html)

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Gesture Demo';

    return new MaterialApp(
      title: title,
      home: new MyHomePage(title: title),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(title),
      ),
      body: new Center(child: new MyButton()),
    );
  }
}

class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Our GestureDetector wraps our button
    return new GestureDetector(
      // When the child is tapped, show a snackbar
      onTap: () {
        final snackBar = new SnackBar(content: new Text("Tap"));

        Scaffold.of(context).showSnackBar(snackBar);
      },
      // Our Custom Button!
      child: new Container(
        padding: new EdgeInsets.all(12.0),
        decoration: new BoxDecoration(
          color: Theme.of(context).buttonColor,
          borderRadius: new BorderRadius.circular(8.0),
        ),
        child: new Text('My Button'),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/handling-taps.gif)
