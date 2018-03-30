# 展示网络图片

显示图像是大多数移动应用程序的基础。Flutter提供了[`Image`](https://docs.flutter.io/flutter/widgets/Image-class.html)Widget来显示不同类型的图像。

为了处理来自URL的图像，请使用[`Image.network`](https://docs.flutter.io/flutter/widgets/Image/Image.network.html)构造函数。

```dart
new Image.network(
  'https://raw.githubusercontent.com/flutter/website/master/_includes/code/layout/lakes/images/lake.jpg',
);
```

## 动画GIF

令人赞叹的是`Image`Widget也支持即时动画gifs！

```dart
new Image.network(
  'https://github.com/flutter/plugins/raw/master/packages/video_player/doc/demo_ipod.gif?raw=true',
)
```

## 占位符和缓存

`Image.network`默认不处理更高级的功能，例如在将图像加载完成之后淡出图片或图片下载完成之后缓存到设备。要完成这些任务，请参阅以下章节：

- [淡入带有占位符的图像](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/images/Fade_in_images_with_a_placeholder.md)
- [处理缓存的图片](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/images/Working_with_cached_images.md)

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'Web Images',
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Web Images')
        ),
        body: new Image.network(
          'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/network-image.png)