# 滑动移除

在许多移动应用程序中，“滑动移除”模式是很常见的。例如，如果我们正在编写一个电子邮件应用程序，我们可能希望允许我们的用户在列表中滑动电子邮件信息。当他们这么做的时候，我们会想要把收件箱里的邮件移到垃圾桶里。

通过[`Dismissible`](https://docs.flutter.io/flutter/widgets/Dismissible-class.html)Widget，Flutter使这项任务变得简单。

## 指南

1、创建列表

2、使用`Dismissible`包装列表中的每一项

3、提供"Leave Behind"指示

## 1、创建列表

第一步将是创建一个可以滑动的列表。有关如何创建列表的更详细说明，请查看[处理长列表](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/lists/Working_with_long_lists.md)。

**创建数据源**

在我们的示例中，我们需要使用20个示例项。为了保持简单，我们将生成一个String列表。

```dart
final items = new List<String>.generate(20,(i)=>"Item ${i+1}");
```

**将数据源转换为列表**

首先，我们将简单地显示屏幕上列表中的每个项目。用户将无法滑动这些项目，只是暂时！

```dart
new ListView.builder(
  itemCount: items.length,
  itemBuilder: (cotext, index){
    return new ListTile(title: new Text('${items[index]}'));
  }
)
```

## 2、使用`Dismissible`包装列表中的每一项

现在，我们显示了一个项目列表，我们希望我们的用户能够滑动列表中的每一项！

在用户删除列表中子项后，我们需要运行一些代码来从列表中删除该项并显示一个Snackbar。在真正的应用程序中，您可能需要执行更复杂的逻辑，例如从Web服务或数据库中删除项。

这就是`Dismissible`Widget发挥作用的地方！在我们的示例中，我们将更新`itemBuilder`函数以返回一个`Dismissible`Widget。

```dart
new Dismissible(
  // 每个可Dismissible的必须包含一个key
  key: new Key(item),
  // 我们还需要提供一个功能来告诉我们的应用程序
  // 在子项被Dismiss后该怎么办。
  onDismissed: (direction){
    // 从数据源移除子项
    items.removeAt(index);
    // 显示snackBar，也可以包含撤销操作
    Scaffold.of(context).showSnackBar(
            new SnackBar(content: new Text("$item dismissed"))); 
  },
  child: new ListTile(title: new Text('${items[index]}'))
)
```

## 3、提供"Leave Behind"指示

就目前情况而言，我们的应用程序将允许用户从列表中滑动项目，但这可能不会给他们提供一个可视化的指示，当他们这样做时会发生什么。为了提供一个提示，我们将显示一个“Leave Behind”的指示。

为此目的，我们将向`Dismissible`提供一个`background`参数。

```dart
new Dismissible(
  // 显示红色背景
  background: new Container(color: Colors.red),
  key: new Key(item),
  onDismissed: (direction) {
    items.removeAt(index);

    Scaffold.of(context).showSnackBar(
        new SnackBar(content: new Text("$item dismissed")));
  },
  child: new ListTile(title: new Text('$item')),
);
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp(
    items: new List<String>.generate(20, (i) => "Item ${i + 1}"),
  ));
}

class MyApp extends StatelessWidget {
  final List<String> items;

  MyApp({Key key, @required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final title = 'Dismissing Items';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            final item = items[index];

            return new Dismissible(
              // 每个可Dismissible的必须包含一个key
              key: new Key(item),
              // 我们还需要提供一个功能来告诉我们的应用程序
              // 在子项被Dismiss后该怎么办。
              onDismissed: (direction) {
                items.removeAt(index);

                Scaffold.of(context).showSnackBar(
                    new SnackBar(content: new Text("$item dismissed")));
              },
              // 显示红色背景
              background: new Container(color: Colors.red),
              child: new ListTile(title: new Text('$item')),
            );
          },
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/dismissible.gif)