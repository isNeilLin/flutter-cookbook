# 淡入淡出一个部件

作为UI开发人员，我们通常需要在屏幕上显示和隐藏元素。然而，在屏幕上快速显示和隐藏元素会让用户感到不舒服。相反，我们可以淡出元素以及改变元素透明度，以创造一个平滑的体验。

在Flutter中，我们可以使用[`AnimatedOpacity`](https://docs.flutter.io/flutter/widgets/AnimatedOpacity-class.html)Widget来完成这个任务

## 指南

1、显示一个淡入淡出的盒子

2、定义一个`StatefulWidget`

3、显示一个按钮，以切换可见性

4、添加淡入淡出动画

## 1、显示一个淡入淡出的盒子

首先，我们需要一些东西来淡入淡出！在本例中，我们将在屏幕上绘制一个绿色背景的盒子。

```dart
new Container(
  color: Colors.green,
  width: 200.0,
  height: 200.0
)
```

## 2、定义一个`StatefulWidget`

现在我们有了一个绿色的盒子来实现动画，我们需要一种方法来知道这个盒子应该是可见的还是不可见的。要实现这一点，我们可以使用[`StatefulWidget`](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)。

`StatefulWidget`是一个创建`State`对象的类。`State`对象保存了一些有关我们应用程序的数据，并提供了更新改数据的方法。当我们更新数据时，我们也可以要求Flutter用这些更改重新构建我们的UI。

在本例中，我们将有一个数据：一个布尔值，用来表示按钮是可见的还是不可见的。

构建一个`StatefulWidget`类，我们需要创建两个类：一个`StatefulWidget`和相应的`State`类。小贴士：Android Studio和VSCode的Flutter插件包括快速生成此代码的`stful`片段！

```dart
//StatefulWidget的工作是接收一些数据并创建一个State类。
//在本例中，我们的Widget接受一个标题，并创建一个_MyHomePageState。
class MyHomePage extends StatefulWidget {
  final String title;
  MyHomePage({Key key, this.title}) : super(key:key);
  
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

//这个State类负责两件事： 保存一些数据，我们可以使用该数据更新和构建UI
class _MyHomePageState extends State<MyHomePage> {
  // 绿色盒子是否可见
  bool _visible = true;
  
  @override
  Widget build(BuildContext context){
    // 绿色的盒子将与其他一些小部件一起放在这里！
  }
}
```

## 3、显示一个按钮，以切换可见性

现在我们有了一些数据来确定我们的绿色盒子应该是可见的还是不可见的，我们需要一种更新数据的方法。在我们的例子中，如果这个盒子是可见的，我们想要隐藏它。如果盒子是隐藏的，我们要显示它！

为此，我们将显示一个按钮。当用户按下按钮时，我们会将布尔值从true转换为false，或false转换为true。我们需要使用[`setState`](https://docs.flutter.io/flutter/widgets/State/setState.html)进行此更改，它是`State`类上的一种方法。这将让Flutter知道它需要重新构建Widget。

```dart
new FloatingActionButton(
  onPressed: (){
    //一定要调用setState，这将告诉Flutter根据我们的更改重建UI
    setState((){
      _visible = !_visible;
    })
  },
  tooltip: 'Toggle Opacity',
  child: new Icon(Icons.flip)
)
```

## 4、添加淡入淡出动画

屏幕上有个绿色盒子。我们有一个按钮来切换可见性为真或假。那我们怎么才能让盒子淡入淡出？使用[`AnimatedOpacity`](https://docs.flutter.io/flutter/widgets/AnimatedOpacity-class.html)Widget!

`AnimatedOpacity`Widget要求三个参数：

- `opacity`：从0.0(不可见)到1.0(完全可见)的值

- `duration`：动画应该持续多久

- `child`：应用动画的部件，在我们的例子中，绿色盒子。

```dart
new AnimatedOpacity(
  //如果Widget应该是可见的，则动画到1.0(完全可见)。如果Widget应该隐藏，动画到0.0(不可见)。
  opacity: _visible ? 1.0 : 0.0,
  duration: new Duration(milliseconds: 500),
  child: new Container(
    color: Colors.green,
    width: 200.0,
    height: 200.0
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
    final appTitle = 'Opacity Demo';
    return new MaterialApp(
      title: appTitle,
      home: new MyHomePage(title: appTitle),
    );
  }
}

//StatefulWidget的工作是接收一些数据并创建一个State类。
//在本例中，我们的Widget接受一个标题，并创建一个_MyHomePageState。
class MyHomePage extends StatefulWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

//这个State类负责两件事： 保存一些数据，我们可以使用该数据更新和构建UI
class _MyHomePageState extends State<MyHomePage> {
  // 绿色盒子是否可见
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: new AnimatedOpacity(
          //如果Widget应该是可见的，则动画到1.0(完全可见)。如果Widget应该隐藏，动画到0.0(不可见)。
          opacity: _visible ? 1.0 : 0.0,
          duration: new Duration(milliseconds: 500),
          child: new Container(
            width: 200.0,
            height: 200.0,
            color: Colors.green,
          ),
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: () {
          //一定要调用setState，这将告诉Flutter根据我们的更改重建UI
          setState(() {
            _visible = !_visible;
          });
        },
        tooltip: 'Toggle Opacity',
        child: new Icon(Icons.flip),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/fade-in-out.gif)