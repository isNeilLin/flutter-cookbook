# 添加侧边导航

在采用Material Design设计的应用程序中，关于导航有两个主要的选择：`tab`和`drawer`。当没有足够的空间来支持`tab`，`drawer`提供了一个方便的选择。

在Flutter中，我们可以使用[`Drawer`](https://docs.flutter.io/flutter/material/Drawer-class.html)Widget与[`Scaffold`](https://docs.flutter.io/flutter/material/Scaffold-class.html)相结合，以创建一个Material Design的Drawer布局。

## 指南

1、创建`Scaffold`

2、添加一个drawer

3、向drawer中添加items

4、关闭drawer

## 1、创建`Scaffold`

为了在我们的应用程序中添加`Drawer`，我们需要将它包装在[`Scaffold`](https://docs.flutter.io/flutter/material/Scaffold-class.html)Widget中。Scaffold Widget为应用程序提供了一致的Material Design视觉结构。它还支持特殊的Material Design设计组件，如`Drawer`，`AppBar`，`SnackBar`。

在本例中，我们将创建一个带`drawer`的`Scaffold`：

```dart
new Scaffold(
  drawer: // 我们将在下一步添加Drawer
)
```

## 2、添加一个drawer

我们现在可以在`Scaffold`上加一个drawer，drawer可以是任意的Widget。但通常最好使用[material库](https://docs.flutter.io/flutter/material/material-library.html)中的`Drawer`部件，这符合Material Design设计规范。

```dart
new Scaffold(
  drawer: new Drawer(
    child: // 我们将在下一步向drawer中添加item
  )
)
```

## 3、向drawer中添加items

既然我们有了一个`Drawer`，我们就可以给它添加内容了！在本例中，我们将使用[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)。虽然我们可以使用`Column`Widget，但是`ListView`更为合适，因为如果内容占用的空间超过屏幕支持的空间，它将允许用户滚动`Drawer`。

我们将使用[`DrawerHeader`](https://docs.flutter.io/flutter/material/DrawerHeader-class.html)和两个[`ListTile`](https://docs.flutter.io/flutter/material/ListTile-class.html)小部件填充`ListView`。有关使用列表的更多信息，请参见[list recipes](https://flutter.io/cookbook/#lists)。

```dart
new Scaffold(
  drawer: new Drawer(
    // 添加ListView，确保了如果没有足够的垂直方向上的空间用户可以滚动
    child: new ListView(
      // 很重要，移除ListView的padding
      padding: EdgeInsets.zero,
      children: <Widget>[
        new DrawerHeader(
          child: new Text('Drawer Header'),
          decoration: new BoxDecoration(
            color: Colors.blue    
          ),
        ),
        new ListTile(
          title: new Text('Item 1'),
          onTap: (){
            // 更新state
            // ...
          }
        ),
        new ListTile(
          title: new Text('Item 2'),
          onTap: (){
            // 更新state
            // ...
          }
        ),
      ]
    )
  )
)
```

## 4、关闭drawer

在用户点击某个Item后，我们通常希望关闭`drawer`。我们如何才能做到这一点？使用[`Navigator`](https://docs.flutter.io/flutter/widgets/Navigator-class.html)！

当用户打开Drawer时，Flutter会将添加Drawer到navigation堆栈中。因此，要关闭Drawer，我们可以调用`navigator.pop(context)`。

```dart
new ListTile(
  title: new Text('Item 1'),
  onTap: (){
    // 更新state
    // 关闭drawer
    Navigator.pop(context);
  }
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  final appTitle = 'Drawer Demo';
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: appTitle,
      home: new MyHomePage(title: appTitle),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;
  
  MyHomePage({Key: key, this.title}) : super(key: key);
  
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(title),
      ),
      body: new Center(
        child: new Text('My Page!')
      ),
      drawer: new Drawer(
        // 添加ListView，确保了如果没有足够的垂直方向上的空间用户可以滚动
        child: new ListView(
           // 很重要，移除ListView的padding 
           padding: EdgeInsets.zero,
           children: <Widget>[
             new DrawerHeader(
                child: new Text('Drawer Header'),
                decoration: new BoxDecoration(
                  color: Colors.blue
                )
             ),
             new ListTile(
                title: new Text('Item 1'),
                onTap: (){
                  // 更新state
                  // 关闭drawer
                  Navigator.pop(context);               
                }
             ),
             new ListTile(
                title: new Text('Item 2'),
                onTap: (){
                  // 更新state
                  // 关闭drawer
                  Navigator.pop(context);               
                }             
             )
           ]
        ),
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/drawer.png)