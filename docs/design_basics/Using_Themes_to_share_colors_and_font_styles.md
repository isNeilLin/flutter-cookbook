# 使用Themes共享颜色和字体样式

为了在整个应用程序中共享颜色和字体样式，我们可以利用主题。有两种方式来定义主题：使用`Theme`部件来定义应用程序特定部分的颜色和字体样式
或者应用程序范围内的颜色和字体样式。实际上，应用程序范围内的主题只是由`MaterialApp`在我们应用程序根部创建的`Theme`小部件。

在定义了主题之后，我们可以在自己的小部件里使用它。此外，由Flutter提供的Material部件将使用我们的主题来设置AppBars、Buttons、Checkboxes等等。

## 创建应用程序主题

为了在整个应用程序中共享包含颜色和字体样式的主题，我们可以向`MaterialApp`构造函数提供[`ThemeData`](https://docs.flutter.io/flutter/material/ThemeData-class.html)。
如果没有提供`theme`，Flutter将会创建一个fallback theme。

```dart
new MaterialApp(
  title: title,
  theme: new ThemeData(
    brightness: Brightness.dark,
    primaryColor: Colors.lightBlue[800],
    accentColor: Colors.cyan[600],
  )
)
```

```go
import "fmt"
func main(){
  fmt.Println("test")
}
```


请参阅[ThemeData](https://docs.flutter.io/flutter/material/ThemeData-class.html)文档，查看所有可供定义的颜色和字体样式。

## 应用程序特定部分的主题

如果想在应用的某部分重写应用程序范围的主题，我们可以将这部分封装为`Theme`部件的子部件。

有两种方法可以解决这个问题：创建唯一的`ThemeData`，或者扩展父主题。

### 创建唯一的`ThemeData`

如果我们不想继承应用程序里现有的颜色和字体样式，可以创建一个`new ThemeData()`实例并把它传递给`Theme`部件。

```dart
new Theme(
  // 使用"new ThemeData"创建唯一的theme
  data: new ThemeData(
    accentColor: Colors.yellow,
  ),
  child: new FloatingActionButton(
    onPressed: () {},
    child: new Icon(Icons.add),
  ),
);
```

### 扩展父主题

相比覆盖所有内容，扩展父主题是我们更为推荐的方式。可以通过使用[`copyWith`](https://docs.flutter.io/flutter/material/ThemeData/copyWith.html)方法来实现。

```dart
new Theme(
  data: Theme.of(context).copyWith(accentColor: Colors.yellow),
  child: new FloatingActionButton(
    onPressed: () {},
    child: new Icon(Icons.add),
  ),
)
```

## 使用主题

现在我们已经定义了主题，就可以在Widget的`build`方法中通过`Theme.of(context)`函数使用它了。

`Theme.of(context)`将查找Widget树并返回树中最近的`Theme`。如果我们在Widget上定义了一个独立主题，它将返回该主题。如果没有，则返回App主题。

实际上，`FloatingActionButton`使用exact技术查找`accentColor`。

```dart
new Container(
  color: Theme.of(context).accentColor,
  child: new Text(
    'Text with a background color',
    style: Theme.of(context).textTheme.title,
  )
)
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main(){
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    final appName = 'Custom Theme';
    return new MaterialApp(
      title: appName,
      theme: new ThemeData(
        brightness: Brightness.dark,
        primaryColor: Colors.lightBlue[800],
        accentColor: Colors.cyan[600],
      ),
      home: new MyHomePage(
        title: appName,
      ),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;
  
  MyHomePage({Key key,@required this.title}) : super(key:key);
  
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
         title: new Text(title),
      ),
      body: new Center(
        child: new Container(
          color: Theme.of(context).accentColor,
          child: new Text(
            'Text with a background color',
            style: Theme.of(context).textTheme.title,
          ),
        ),
      ),
      floatingActionButton: new Theme(
        data: Theme.of(context).copyWith(accentColor: Colors.yellow),
        child: new FloatingActionButton(
          onPressed: null,
          child: new Icon(Icons.add),
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/themes.png)