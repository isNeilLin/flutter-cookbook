# 导航到命名路由

在[导航到新页面并返回](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/navigation/Navigate_to_a_new_screen_and_back.md)章节中，我们学习了如何通过创建一个新的路由导航到新的页面并将它添加到[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)。

但是，如果我们需要在应用程序的许多部分导航到相同的屏幕，这可能会导致代码重复。在这些情况下，定义“命名路由”并使用命名路由进行导航是非常方便的。

要使用指定的路由，我们可以使用[`Navigator.pushNamed`](https://docs.flutter.io/flutter/widgets/Navigator/pushNamed.html)函数。此示例将演示如何使用命名路由。

## 步骤

1、 创建两个页面
2、 定义路由
3、 使用`Navigator.pushNamed`导航到第二个页面
4、 使用`Navigator.pop`返回到第一个页面

## 1. 创建两个页面

首先，我们需要使用两个页面。第一个页面将包含导航到第二个页面的按钮。第二个页面将包含一个导航回到第一个页面的按钮。

```dart
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Screen'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Launch screen'),
          onPressed: () {
            // 点击导航到第二个页面
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Screen"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            // 点击返回到第一个页面
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. 定义路由

接下来，我们需要通过向[`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html)构造函数提供额外的属性来定义我们的路由`initialRoute`和`routes`本身。

`initialRoute`属性定义了我们的应用程序应该从哪个路由开始。`routes`属性定义了可用的命名路由和在导航到这些路由时应该构建的部件。

```dart
MaterialApp(
  // 从"/"路由这里开始，在本例中是第一个页面
  initialRoute: '/',
  routes: {
    // 当导航到"/"时，构建第一个页面
    '/': (context) => FirstScreen(),
    // 当导航到"/second"时，构建第二个页面
    '/second': (context) => SecondScreen(),
  },
);
```

> 注意：当使用`initialRoute`时，请确保没有定义`home`属性。

## 3. 使用`Navigator.pushNamed`导航到第二个页面

在部件和路由完成之后，我们就可以开始导航了！在本例中，我们将使用[`Navigator.pushNamed`](https://docs.flutter.io/flutter/widgets/Navigator/pushNamed.html)函数。这告诉Fluter构建在我们的路由表中定义的Widget并启动屏幕。

在`FirstScreen`部件的`build`方法中，我们需要更新`onPressed`回调函数：

```dart
// in FirstScreen Widget
onPressed: () {
  Navigator.pushNamed(context, '/second');
}
```

## 4. 使用`Navigator.pop`返回到第一个页面

为了回到第一个页面，我们可以使用[`Navigator.pop`](https://docs.flutter.io/flutter/widgets/Navigator/pop.html)函数。

```dart
// in SecondScreen Widget
onPressed: () {
  Navigator.pop(context);
}
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: 'Named Routes Demo',
     // 从"/"路由这里开始，在本例中是第一个页面
    initialRoute: '/',
    routes: {
      // 当导航到"/"时，构建第一个页面
      '/': (context) => FirstScreen(),
      // 当导航到"/second"时，构建第二个页面
      '/second': (context) => SecondScreen(),
    },
  ));
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Screen'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Launch screen'),
          onPressed: () {
            // 使用命名路由导航到第二个页面
            Navigator.pushNamed(context, '/second');
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Screen"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            // 通过弹出当前路由从堆栈返回到第一个页面
            Navigator.pop(context);
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/navigation-basics.gif)

