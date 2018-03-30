# 处理缓存的图片

在某些情况下，可以方便地缓存从网络下载的图像，以便它们可以离线使用。为此，我们将使用[cached_network_image](https://pub.dartlang.org/packages/cached_network_image)包。

除了缓存之外，`cached_network_image`包在加载时还支持占位符和淡入图像！

```dart
new CachedNetworkImage(
  imageUrl: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
)
```

## 加入占位符

`cached_network_image`包允许我们使用任何Widget作为占位符！在本例中，我们将在图像加载时显示一个loading。

```dart
new CachedNetworkImage(
  placeholder: new CircularProgressIndicator(),
  imageUrl: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';
import 'package:cached_network_image/cached_network_image.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final title = 'Cached Images';

    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Center(
          child: new CachedNetworkImage(
            placeholder: new CircularProgressIndicator(),
            imageUrl:
                'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
          ),
        ),
      ),
    );
  }
}
```