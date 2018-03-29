# 使用Tabs

使用选项卡是应用程序中遵循Material Design设计准则的常见模式。Flutter包括一种方便的方法来创建tab布局作为库的[Material库](https://docs.flutter.io/flutter/material/material-library.html)的一部分。

## 指南

1、 创建一个`TabController`

2、 创建Tabs

3、 为每个tab创建内容

## 1、 创建一个`TabController`

为了使选项卡工作，我们需要保持选定的选项卡和内容部分保持同步。这是[`TabController`](https://docs.flutter.io/flutter/material/TabController-class.html)的职责。

我们可以手动创建`TabController`或使用[`DefaultTabController`](https://docs.flutter.io/flutter/material/DefaultTabController-class.html)。使用`DefaultTabController`是最简单的选择。它将为我们创建一个`TabController`，并将其提供给所有的后代小部件。

```dart
new DefaultTabController(
  length: 3, // 需要显示的选项卡数量
  child: // 看下一步
);
```

## 2、 创建Tabs

现在我们有了一个`TabController`，我们可以使用[`TabBar`](https://docs.flutter.io/flutter/material/TabController-class.html)创建我们的选项卡了。在本例中，我们将创建一个带有3个[`Tab`](https://docs.flutter.io/flutter/material/Tab-class.html)小部件的`TabBar`，并将其放置在[`AppBar`](https://docs.flutter.io/flutter/material/AppBar-class.html)中。

```dart
new DefaultTabController(
  length: 3,
  child: new Scaffold(
    appBar: new AppBar(
      bottom: new TabBar(
        tabs: [
          new Tab(icon: new Icon(Icons.directions_car)),
          new Tab(icon: new Icon(Icons.directions_transit)),,
          new Tab(icon: new Icon(Icons.directions_bike)),
        ]
      ),
    )
  ),
);
```

默认情况下，`TabBar`查找最近的`DefaultTabController`的Widget树。如果要手动创建`TabController`，则需要将其传递给`TabBar`。

## 3、 为每个tab创建内容

现在我们有了选项卡，我们希望在选择选项卡时显示内容。为了这个目的，我们将使用[`TabBarView`](https://docs.flutter.io/flutter/material/TabBarView-class.html)部件。

**注意**：顺序很重要，必须与`TabBar`中选项卡的顺序相对应！

```dart
new TabBarView(
  children: [
    new Icon(Icons.directions_car),
    new Icon(Icons.directions_transit),
    new Icon(Icons.directions_bike),
  ]
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new TabBarDemo());

class TabBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'TabBar Demo',
      home: new DefaultTabBarController(
        length: 3,
        child: new Scaffold(
          appBar: new AppBar(
            title: new Text('Tabs Demo'),
            bottom: new TabBar(
              tabs: [
                new Tab(icon: new Icon(Icons.directions_car)),
                new Tab(icon: new Icon(Icons.directions_transit)),
                new Tab(icon: new Icon(Icons.directions_bike)),
              ],
            )
          ),
          body: new TabBarView(
            children: [
              new Icon(Icons.directions_car),
              new Icon(Icons.directions_transit),
              new Icon(Icons.directions_bike),
            ]
          )
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/tabs.gif)