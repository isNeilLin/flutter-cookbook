# 一个屏幕动画到另一个屏幕

在用户从一个屏幕导航到另一个屏幕时，引导用户浏览我们的应用程序通常是有帮助的。引导用户通过应用程序的一种常见技术是将Widget从一个屏幕动画到另一个屏幕。这将创建一个连接这两个屏幕的可视锚点。

我们如何用Flutter将一个Widget从一个屏幕动画到另一个屏幕？用[`Hero`](https://docs.flutter.io/flutter/widgets/Hero-class.html)部件！

## 指南

1、创建显示相同图片的两个Screen

2、添加`Hero`部件到第一个Screen

3、添加`Hero`部件到第二个Screen

## 1、创建显示相同图片的两个Screen

在本例中，我们将在两个Screen上显示相同的图像。当用户点击图像时，我们希望将图像从第一个屏幕动画到第二个屏幕。现在，我们将创建可视化结构，并在接下来的步骤中处理动画！

```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Main Screen'),
      ),
      body: new GestureDetector(
        onTap: (){
          Navigator.push(context,new MaterialPageRoute(
            builder: (_) => new DetailScreen()
          ));
        },
        child: new Image.network(
          'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
        )
      )
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      body: new GestureDetector(
        onTap: (){
          Navigator.pop(context);
        },
        child: new Center(
          child: new Image.network(
            'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
          ),
        ),
      )
    );
  }
}
```

## 2、添加`Hero`部件到第一个Screen

为了将这两个Screen用动画连接起来，我们需要将`Image`Widget封装在`Her`oWidget中。`Hero`Widget需要两个参数：

1、`tag`：识别`Hero`的对象。两个Screen上都是一样的

2、 `child`：我们想在Screen上做动画的Widget。

```dart
new Hero(
  tag: 'imageHero',
  child: new Image.network(
    'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
  ),
)
```

## 3、添加`Hero`部件到第二个Screen

要完成与第一个Screen的连接，我们还需要在第二个Screen上使用`Hero`Widget包装`Image`！它必须使用与第一个Screen相同的`tag`。

在将`Hero`Widget应用到第二个Screen后，Screen之间的动画将工作！

```dart
new Hero(
  tag: 'imageHero',
  child: new Image.network(
    'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
  ),
)
```

> 注意：此代码与我们在第一个屏幕上的代码相同！通常，您可以创建一个可重用的Widget，而不是重复代码，但是对于这个示例，我们将复制代码以进行演示。

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new HeroApp());

class HeroApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'Transition Demo',
      home: new MainScreen()
    );
  }
}

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Main Screen')
      ),
      body: new GestureDetector(
        onTap: (){
          Navigator.push(context,new MaterialPageRoute(
            builder: (_) => new DetailScreen()
          ));
        },
        child: new Hero(
          tag: 'imageHero',
          child: new Image.network(
            'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
          )
        )
      )
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      body: new GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: new Center(
          child: new Hero(
            tag: 'imageHero',
            child: new Image.network('https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg')
          )
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/hero.gif)
