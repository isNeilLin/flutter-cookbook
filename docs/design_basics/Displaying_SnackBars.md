# 显示SnackBars

在某些情况下，当某些操作放生时，可以方便地简短地通知用户。例如，当用户删除列表中的消息时，我们可能希望通知他们消息已被删除。我们甚至可以给他们一个选项来撤销这个动作！

在Material Design中，这是[`SnackBar`](https://docs.flutter.io/flutter/material/SnackBar-class.html)的职责。

## 指南

1、 创建`Scaffold`

2、 显示`SnackBar`

3、 提供额外的操作

## 1、 创建`Scaffold`

当创建遵循Material Design设计准则的应用程序时，我们希望给我们的应用程序一致的视觉结构。在本例中，我们需要在屏幕底部显示`SnackBar`，而不覆盖其他重要的小部件，例如`FloatingActionButton`！

[Material库](https://docs.flutter.io/flutter/material/material-library.html)中的[`Scaffold`](https://docs.flutter.io/flutter/material/Scaffold-class.html)部件为我们创建了这个可视化结构，并确保重要的小部件不重叠。

```dart
new Scaffold(
  appBar: new AppBar(
    title: new Text('SnackBar Demo'),
  ),
  body: new SnackBarPage(),
)
```

## 2、 显示`SnackBar`

`Scaffold`创建完成之后，我们可以显示一个`SnackBar`！首先，我们需要创建一个`SnackBar`，然后使用`Scaffold`显示它。

```dart
final snackBar = new SnackBar(content: new Text('Yay! A SnackBar!'));

// 在部件树中找到Scaffold并用它显示SnackBar
Scaffold.of(context).showSnackBar(snackBar);
```

## 3、 提供额外的操作

在某些情况下，我们可能希望在显示`SnackBar`时向用户提供附加操作。例如，如果他们无意中删除了一条消息，我们可以提供一个操作来撤销该操作。

为了实现这一点，我们可以向`SnackBar`部件提供一个`action`。

```dart
final snackBar = new SnackBar(
  content: new Text('Yay! A SnackBar!'),
  action: new SnackBarAction(
    label: 'undo',
    onPressed: (){
      // 撤销的代码
    },
  ),
);
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main()=>runApp(new SnackBarDemo());

class SnackBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'SnackBar Demo',
      home: new Scafflod(
        appBar: new AppBar(
          title: new Text('SnackBar Demo')
        ),
        body: new SnackBarPage(),
      ),
    );
  }
}

class SnackBarPage extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Center(
      child: new RaisedButton(
        onPressed: (){
          final snackBar = new SnackBar(
            content: new Text('Yay! A SnackBar!'),
            action: new SnackBarAction(
               label: 'undo',
               onPressed: (){
                 // 撤销的代码
               }
            ),
          );
          Scaffold.of(context).showSnackBar(snackBar);
        },
        child: new Text('Show SnackBar'),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/snackbar.gif)