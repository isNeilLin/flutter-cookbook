# 创建基础列表

显示数据列表是移动应用的基本模式。Flutter包括[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)Widget，使得生成列表易如反掌！

## 创建ListView

使用标准`ListView`构造函数对于仅包含几个项目的列表来说是完美的。我们还将使用内置的`ListTile`Widget为我们的项目提供视觉结构。

```dart
new ListView(
  children: <Widget>[
    new ListTile(
      leading: new Icon(Icons.map),
      title: new Text('Map')
    ),
    new ListTile(
      leading: new Icon(Icons.photo_album),
      title: new Text('Album')
    ),
    new ListTile(
      leading: new Icon(Icons.phone),
      title: new Text('Phone')
    ),
  ]
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Basic List';
    
    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new ListView(
          children: <Widget>[
            new ListTile(
              leading: new Icon(Icons.map),
              title: new Text('Map'),
            ),
            new ListTile(
              leading: new Icon(Icons.photo_album),
              title: new Text('Album'),
            ),
            new ListTile(
              leading: new Icon(Icons.phone),
              title: new Text('Phone'),
            ),
          ],
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/basic-list.png)