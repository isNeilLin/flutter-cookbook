# 淡入带有占位符的图像

当使用默认`Image`小部件显示图像时，您可能会注意到它们只是在加载时弹出到屏幕上。这可能会给用户带来视觉上的不适。

相反，如果你一开始就能显示一个占位符，并且图像在加载时会褪色，这不是很好吗？我们可以使用[`FadeInImage`](https://docs.flutter.io/flutter/widgets/FadeInImage-class.html)，正是为了这个目的！

`FadeInImage`可以处理任何类型的图像：内存中的图像、本地资源或来自网络的图像。

在本例中，我们将使用[transparent_image](https://pub.dartlang.org/packages/transparent_image)作为一个简单的透明占位符。您还可以考虑使用本地资源作为占位符，方法是遵循[Assets and Images](https://flutter.io/assets-and-images/)指南。

```dart
new FadeInImage.memoryNetwork(
  placeholder: KTransparentImage,
  image: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
)
```

## 完整示例

```dart
import 'package:flutter/material.dart';
import 'package:transparent_image/transparent_image.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    final title = 'Fade in images';
    
    return new MaterialApp(
      title: title,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(title),
        ),
        body: new Stack(
          children: <Widget>[
            new Center(
              child: new CircularProgressIndictor(),
            ),
            new Center(
              child: new FadeInImage.memoryNetwork(
                placeholder: KTransparentImage,
                image: 'https://github.com/flutter/website/blob/master/_includes/code/layout/lakes/images/lake.jpg?raw=true',
              )
            ),
          ]
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/fading-in-images.gif)