# 创建水平列表

有时，您可能希望创建一个水平滚动而不是垂直滚动的列表。[`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)Widget支持水平列表。我们将使用标准的`ListView`构造函数，通过`scrollDirection`属性，这将覆盖默认的垂直方向。

```dart
new ListView(
  scrollDirection: Axis.horizontal,
  children: <Widget>[
    new Container(
      width: 160.0,
      color: Colors.red,
    ),
    new Container(
      width: 160.0,
      color: Colors.blue,
    ),
    new Container(
      width: 160.0,
      color: Colors.green,
    ),
    new Container(
      width: 160.0,
      color: Colors.yellow,
    ),
    new Container(
      width: 160.0,
      color: Colors.orange,
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
    final title = 'Horizontal List';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Container(
          margin: new EdgeInsets.symmetric(vertical: 20.0),
          height: 200.0,
          child: new ListView(
            scrollDirection: Axis.horizontal,
            children: <Widget>[
              new Container(
                width: 160.0,
                color: Colors.red,
              ),
              new Container(
                width: 160.0,
                color: Colors.blue,
              ),
              new Container(
                width: 160.0,
                color: Colors.green,
              ),
              new Container(
                width: 160.0,
                color: Colors.yellow,
              ),
              new Container(
                width: 160.0,
                color: Colors.orange,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/horizontal-list.gif)