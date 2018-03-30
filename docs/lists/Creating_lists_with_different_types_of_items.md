# 创建具有不同类型项的列表

我们通常需要创建显示不同类型内容的列表。例如，我们可能正在处理一个列表，该列表显示一个标题，后面跟着几个与标题相关的项，然后是另一个标题，依此类推。

我们怎样才能创造出这样一种Flutter的结构呢？

## 指南

1、使用不同类型的项创建数据源

2、将数据源转换为小部件列表

## 1、使用不同类型的项创建数据源

为了在列表中表示不同类型的项，我们需要为每种类型的项定义一个类。

在本例中，我们将开发一个应用程序，它显示一个标题，后面跟着五个消息。因此，我们将创建三个类：`ListItem`、`HeadingItem`和`MessageItem`。

```dart
//可以包含的不同类型项的基类
abstract class ListItem {}

//包含显示标题的数据的ListItem
class HeadingItem implements ListItem {
  final String heading;
  
  HeadingItem(this.heading);
}

// 包含显示消息的数据的ListItem
class MessageItem implements ListItem {
  final String sender;
  final String body;
  
  MessageItem({this.sender,this.body});
}
```

大多数情况下，我们会从互联网或本地数据库中获取数据，并将这些数据转换为项目列表。

对于本例，我们将生成要使用的列表。该列表将包含一个标题，后面是五条消息。然后重复。

```dart
final items = new List<ListItem>.generate(
  1200,
  (i) => i % 6 == 0
      ? HeadingItem("Heading ${i}")
      : MessageItem("Sender ${i}", "Message body ${i}")
);
```

## 2、将数据源转换为小部件列表

为了将每个项转换为Widget，我们将使用[`ListView.Builder`](https://docs.flutter.io/flutter/widgets/ListView/ListView.builder.html)构造函数。

通常，我们希望提供一个`builder`函数来检查我们所处理的项目的类型，并为该类型的项返回适当的Widget。

在本例中，使用`is`关键字检查我们正在处理的项的类型是很方便的。它很快，并将自动将每个项目转换为适当的类型。但是，如果您喜欢另一种模式，则有不同的方法来处理这个问题！

```dart
new ListView.builder(
  itemCoount: items.length,
  itemBuilder: (context, index){
    final item = items[index];
    if(item is HeadingItem){
      return new ListTile(
        titll: new Text(
          item.heading,
          style: Theme.of(context).textTheme.headline
        )
      );
    }else if(item is MessageItem){
      return new ListTile(
        title: new Text(item.sender),
        subtitle: new Text(item.body)
      );
    }
  }
)
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main(){
  runApp(new MyApp(
    items: new List<ListItem>.generate(
     1200,
     (i) => i % 6 == 0
         ? HeadingItem("Heading ${i}")
         : MessageItem("Sender ${i}", "Message body ${i}")
   )
}

class MyApp extends StatelessWidget {
  
  final List<ListItem> items;
  
  MyApp({Key: key, @required this.items}) : super(key:key);
  
  @override
  Widget build(BuildContext context){
    final title = 'Mixed List';
    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index){
            final item = items[index];
            if(item is HeadingItem){
              return new ListTile(
                titll: new Text(
                  item.heading,
                  style: Theme.of(context).textTheme.headline
                )
              );
            }else if(item is MessageItem){
              return new ListTile(
                title: new Text(item.sender),
                subtitle: new Text(item.body)
              );
            }
          }
        )
      )
    );
  }
}

//可以包含的不同类型项的基类
abstract class ListItem {}

//包含显示标题的数据的ListItem
class HeadingItem implements ListItem {
  final String heading;
  
  HeadingItem(this.heading);
}

// 包含显示消息的数据的ListItem
class MessageItem implements ListItem {
  final String sender;
  final String body;
  
  MessageItem({this.sender,this.body});
}
```

![效果图](https://flutter.io/images/cookbook/mixed-list.png)