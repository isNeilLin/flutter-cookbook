# 导航到新页面并返回

大多数应用程序都包含几个显示不同类型信息的Screen。例如，我们可能有一个显示产品的Screen。我们的用户可以点击一个产品，在一个新的Screen获得更多的信息。

从Android的角度来说，我们的Screen将是新的Activities。在IOS术语中，新的ViewController。在Flutter中，Screen只是Widget(部件)。

那么，我们如何导航到新的Screen呢？用[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)！

## 指南

1、创建两个Screen

2、导航到第二个Screen使用`Navigator.push`

3、回到第一个Screen使用`Navigator.pop`

## 1、创建两个Screen

首先，我们需要使用两个Screen。因为这是一个基本的例子，我们将创建两个Screen，每个Screen包含一个按钮。点击第一个Screen上的按钮将导航到第二个Screen。点击第二个Screen上的按钮将我们的用户返回到第一个！

首先，我们将建立视觉结构。

```dart
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('First Screen'),
      ),
      body: new Center(
        child: new RaisedButton(
          child: new Text('Launch new screen'),
          onPressed: (){
            //导航到第二个Screen
          }
        )
      )
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Second Screen'),
      ),
      body: new Center(
        child: new RaisedButton(
          child: new Text('Go Back'),
          onPressed: (){
            //导航到第一个Screen
          }
        )
      )
    );
  }
}
```

## 2、导航到第二个Screen使用`Navigator.push`

为了导航到一个新的Screen，我们需要使用[`Navigator.push`](https://docs.flutter.io/flutter/widgets/Navigator/push.html)方法,`push`方法将向由Navigator管理的路由堆栈中添加一个`Route`！

`push`方法需要一个`Route`，但是`Route`从何而来？我们可以创建自己的，也可以直接使用[`MaterialPageRoute`](https://docs.flutter.io/flutter/material/MaterialPageRoute-class.html)。`MaterialPageRoute`非常方便，因为它使用特定于平台的动画转换到新Screen。

在`FirstScreen`Widget的`build`方法中，我们将更新`onPressed`回调：

```dart
// FirstScreen部件
onPressed: (){
  Navigator.push(
    context,
    new MaterialPageRoute(
      build: (context)=>new SecondScreen()
    )
  )
}
```

## 3、回到第一个Screen使用`Navigator.pop`

现在我们已经进入了第二个Screen，我们如何关闭它并返回到第一个Screen呢？使用[`Navigator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)方法！`pop`方法将从导航器管理的路由堆栈中删除当前`Route`。

对于此部分，我们需要更新在我们的`SecondScreen`部件中的`onPressed`回调:

```dart
// SecondScreen部件
onPressed: (){
  Navigator.pop(context);
}
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: 'Navigation Basics',
    home: new FirstScreen(),
  ));
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('First Screen'),
      ),
      body: new Center(
        child: new RaisedButton(
          child: new Text('Launch new screen'),
          onPressed: () {
            Navigator.push(
              context,
              new MaterialPageRoute(builder: (context) => new SecondScreen()),
            );
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("Second Screen"),
      ),
      body: new Center(
        child: new RaisedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: new Text('Go back!'),
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/navigation-basics.gif)