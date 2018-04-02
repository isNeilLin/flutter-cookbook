# 添加Material波纹

在设计一个应该遵循Material设计准则的应用程序时，我们想要在点击时将波纹动画添加到小部件中。

Flutter提供了[`InkWell`](https://docs.flutter.io/flutter/material/InkWell-class.html)Widget来实现这一效果。

## 指南

1、创建想要点击的组件

2、将其包装在`InkWell`中管理`Tap`回调和波纹动画

```dart
new InkWell(
  onTap: (){
    final snackBar = new SnackBar(content: new Text('Tap'));
    Scaffold.of(content).showSnackBar(snackBar);
  },
  child: new Container(
    padding: const EdgeInsets.all(12.0),
    child: new Text('Flat Button'),
  )
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'InkWell Demo';

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
    // InkWell包装我们自定义的FlatButton
    return new InkWell(
      // 当点击按钮，显示snackBar
      onTap: () {
        Scaffold.of(context).showSnackBar(new SnackBar(
          content: new Text('Tap'),
        ));
      },
      child: new Container(
        padding: new EdgeInsets.all(12.0),
        child: new Text('Flat Button'),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/ripples.gif)