# 创建网格列表

在某些情况下，您可能希望将子项显示为一个网格，而不是一个一个接一个的正常列表。对于这个需求，我们将使用[`GridView `](https://docs.flutter.io/flutter/widgets/GridView-class.html)Widget。

开始使用网格的最简单方法是使用[`GridView.count`](https://docs.flutter.io/flutter/widgets/GridView/GridView.count.html)构造函数，因为它允许我们指定我们想要的行或列数。

在本例中，我们将生成一个由100个小部件组成的列表，在列表中显示它们的索引。这将帮助我们可视化的认识`GridView`的工作方式。

```dart
new GridView.count(
   //创建具有2个列的网格
   //如果将scrollDirection更改为horizontal,这将产生2行
   crossAxisCount: 2,
   //生成在列表中显示索引的100个小部件
   children: new List.generate(100,(index){
     return new Center(
        child: new Text(
          'Item ${index}',
          style: Theme.of(context).textTheme.headline
        )
     );
   })
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Grid List';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new GridView.count(
          //创建具有2个列的网格
          //如果将scrollDirection更改为horizontal,这将产生2行
          crossAxisCount: 2,
          //生成在列表中显示索引的100个小部件
          children: new List.generate(100, (index) {
            return new Center(
              child: new Text(
                'Item $index',
                style: Theme.of(context).textTheme.headline,
              ),
            );
          }),
        ),
      ),
    );
  }
}
```
