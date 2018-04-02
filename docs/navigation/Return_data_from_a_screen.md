# 从页面返回数据

在某些情况下，我们可能希望从新Screen返回数据。例如，我们推送一个新Screen，向用户提供两个选项。当用户点击某个选项时，我们将希望将用户的选择通知我们的第一个Screen，这样它就可以对该信息采取行动了！

我们如何才能做到这一点？使用[`Navigator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)！

## 指南

1、定义主屏幕

2、添加按钮打开SelectionScreen

3、显示SelectionScreen的两个按钮

4、当点击按钮，关闭SelectionScreen

5、在主屏幕显示带有selection的SnackBar

## 1、定义主屏幕

主屏幕将会显示一个按钮，当点击时会打开SelectionScreen。

```dart
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Returning Data Demo')
      ),
      // 我们将在下一步创建SelectionButton部件
      body: new Center(child: new SelectionButton())
    );
  }
}
```

## 2、添加按钮打开SelectionScreen

现在我们将要创建SelectionButton，我们的SelectionButton将会：

1、 当被点击的时候打开SelectionScreen

2、 等待SelectionScreen返回结果

```dart
class SelectionButton extends StatelessWidget {
  @override
  Widget build(BuildCotext context){
    return new RaisedButton(
      child: new Text("Pick an option, any option!"),
      onPressed: _navigateAndDisplaySelection(context),
    );
  }
  
  // 启动SelectionScreen并等待Navigator.pop返回结果
  void _navigateAndDisplaySelection(BuildContext context) async{
    // Navigator.push在我们调用Navigator.pop之后返回结果
    final result = await Navigator.push(
      context,
      // 我们将在下一步创建SelectionScreen
      new MaterialPageRoute(
        builder: (context) => new SelectionScreen()
      )
    );
    
  }
}
```

## 3、显示SelectionScreen的两个按钮

现在我们需要构建SelectionScreen。它将包含两个按钮，当用户点击按钮时，它应该关闭选择SelectionScreen并让主屏幕知道点击了哪个按钮！

现在，我们将定义UI，并找出如何在下一步中返回数据。

```dart
class SelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Pick an option'),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                },
                child: new Text('Yep!'),
              ),
            ),
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                },
                child: new Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

## 4、当点击按钮，关闭SelectionScreen

现在，我们要更新两个按钮的`onPressed`回调！为了将数据返回到第一个屏幕，我们需要使用`Navigator.pop`方法。

`Navigator.pop`接受一个可选的第二个参数，称为`result`。如果我们提供一个`result`，它将返回给`Future`！

**Yep Button**

```dart
new RaisedButton(
  onPressed: () {
    // 返回"Yep!"作为结果
    Navigator.pop(context, 'Yep!');
  },
  child: new Text('Yep!'),
);
```

**Nope Button**

```dart
new RaisedButton(
  onPressed: () {
    // 返回"Nope!"作为结果
    Navigator.pop(context, 'Nope!');
  },
  child: new Text('Nope!'),
);
```

## 5、在主屏幕显示带有selection的SnackBar

现在我们启动了一个SelectionScreen并等待结果，我们将用返回的信息做一些事情！

在这种情况下，我们将展示一个SnackBar显示结果。我们会更新`SelectionButton`的`_navigateAndDisplaySelection`方法。

```dart
void _navigateAndDisplaySelection(BuildContext context) async {
  final result = await Navigator.push(
     context,
     new MaterialPageRoute(
        builder: (context) => new SelectionScreen()
     )
  );
  
  // SelectionScreen返回结果之后，以SnackBar的形式展示
  Scaffold.of(context).showSnackBar(
    new SnackBar(content: new Text('${result}'))
  );
}
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: 'Returning Data',
    home: new HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Returning Data Demo'),
      ),
      body: new Center(child: new SelectionButton()),
    );
  }
}

class SelectionButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new RaisedButton(
      onPressed: () {
        _navigateAndDisplaySelection(context);
      },
      child: new Text('Pick an option, any option!'),
    );
  }

  // 启动SelectionScreen并等待Navigator.pop返回结果
  _navigateAndDisplaySelection(BuildContext context) async {
    // Navigator.push在我们调用Navigator.pop之后返回结果
    final result = await Navigator.push(
      context,
      new MaterialPageRoute(builder: (context) => new SelectionScreen()),
    );

    // SelectionScreen返回结果之后，以SnackBar的形式展示
    Scaffold
        .of(context)
        .showSnackBar(new SnackBar(content: new Text("$result")));
  }
}

class SelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Pick an option'),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // 返回"Yep!"作为结果
                  Navigator.pop(context, 'Yep!');
                },
                child: new Text('Yep!'),
              ),
            ),
            new Padding(
              padding: const EdgeInsets.all(8.0),
              child: new RaisedButton(
                onPressed: () {
                  // 返回"Nope!"作为结果
                  Navigator.pop(context, 'Nope.');
                },
                child: new Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/returning-data.gif)