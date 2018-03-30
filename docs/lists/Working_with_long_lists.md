# 处理长列表

标准的[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)构造函数对于小列表效果很好。但为了处理包含大量子项的列表，最好使用[`ListView.Builder`](https://docs.flutter.io/flutter/widgets/ListView/ListView.builder.html)构造函数。

默认的`ListView`构造函数要求我们一次创建所有子项，`ListView.Builder`构造函数将在滚动到屏幕上时创建子项。

## 1、创建数据源

首先，我们需要一个可以使用的数据源。例如，数据源可能是存储中的消息、搜索结果或产品的列表。大多数情况下，这些数据将来自网络或数据库。

对于本例，我们将使用[`List.generate`](https://docs.flutter.io/flutter/dart-core/List/List.generate.html)构造函数生成一个包含10000个字符串的列表。

```dart
final items = new List<String>.generate(10000, (i)=>"Item ${i}");
```

## 2、将数据源转化为Widgets

为了显示字符串列表，我们需要将每个字符串呈现为Widget！

这就是`ListView.builder`发挥作用的地方。在我们的例子中，我们将在每行上显示一个字符串。

```dart
new ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index){
    return new ListTile(
      title: new Text('${items[index]}')
    )
  }
)
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp(
    items: new List<String>.generate(10000, (i)=>"Item ${i}")
  ));
}

class MyApp extends StatelessWidget {
  final List<String> items;
  
  MyApp({Key: key, this.items}) : super(key: key);
  
  @override
  Widget build(BuildContext context){
    final title = 'Long List';
    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title)
        ),
        body: new ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index){
            return new ListTile(
              title: new Text("${items[index]}")
            );
          }
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/long-lists.gif)