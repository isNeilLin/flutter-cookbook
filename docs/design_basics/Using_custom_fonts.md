# 使用自定义字体

虽然Android和IOS提供高质量的系统字体，但设计者最常见的要求之一是使用自定义字体！例如，我们可以从设计人员那里定制字体，或者从[Google Fonts](https://fonts.google.com/)下载字体。

Flutter对于自定义字体有着开箱即用的体验。我们可以将字体应用于整个应用程序，也可以应用于各个小部件。

## 指南

1、 导入字体文件

2、 在`pubspec.yaml`中声明字体

3、 选择一种默认字体

4、 在指定的部件中使用字体

## 1、 导入字体文件

为了使用字体，我们需要将字体文件导入到项目中。通常的做法是将字体文件放在Flutter项目的根目录中的`fonts`或`assets`文件夹中。

例如，如果我们想要将Raleway和RobotoMono字体文件导入到我们的项目中，文件夹结构将如下所示：

```
awesome_app/
    fonts/
       Raleway-Regular.ttf
       Raleway-Italic.ttf
       RobotoMono-Regular.ttf
       RobotoMono-Bold.ttf  
```

## 2、 在`pubspec.yaml`中声明字体

现在我们项目中有了字体文件，还需要告诉Flutter在哪里找到它。我们可以通过在`pubspec.yaml`中包含一个字体定义来做到这一点。

```
flutter:
    fonts:
       - family: Raleway
         fonts:
            - asset: fonts/Raleway-Regular.ttf
            - asset: fonts/Raleway-Italic.ttf
              style: italic
       - family: RobotoMono
         fonts:
            - asset: fonts/RobotoMono-Regular.ttf
            - asset: fonts/RobotoMono-Bold.ttf
              weight: 700  
```

**`pubspec.yaml`选项说明**

`family`决定字体的名称，我们可以在[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)对象的[`fontFamily`](https://docs.flutter.io/flutter/painting/TextStyle/fontFamily.html)属性中使用该字体。

`asset`是字体文件的路径，相对于`pubspec.yaml`文件。这些文件包含字体中的文字的概述。在构建应用程序时，这些文件包含在应用程序的静态资源包中。

单一字体可以引用具有不同字重和样式的许多不同文件：
    - `weight`属性将文件中的字重指定为100到900之间的整数倍数。这些值对应于[`FontWight`](https://docs.flutter.io/flutter/dart-ui/FontWeight-class.html)，可以在[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)对象的[`fontWight`](https://docs.flutter.io/flutter/painting/TextStyle/fontWeight.html)属性中使用。
    - `style`属性指定文件中的字体是斜体还是正常。这些值对应于[`FontStyle`](https://docs.flutter.io/flutter/dart-ui/FontStyle-class.html)，可以在[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)对象的[`fontStyle`](https://docs.flutter.io/flutter/painting/TextStyle/fontStyle.html)属性中使用。
 
## 3、 选择一种默认字体

对于如何将字体应用于文本，我们有两个选项：默认字体或仅在特定部件中。

要使用字体作为默认设置，我们可以将`fontFamily`属性设置为应用程序`theme`的一部分。我们提供给`fontFamily`的值必须与在`pubspec.yaml`中声明的名字相匹配。

```dart
new MaterialApp(
  title: 'Custom Fonts',
  theme: new ThemeData(fontFamily: 'Raleway'),
  home: new MyHomePage(),
);
```

## 4、 在指定的部件中使用字体

如果我们想将字体应用到特定的Widget，比如`Text`Widget，我们可以为Widget提供一个[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)。

在本例中，我们将将RobotoMono字体应用于单个`Text`Widget。同样，`fontFamily`必须与我们在`pubspec.yaml`中的声明相匹配。

```dart
new Text(
  'Roboto Mono sample',
  style: new TextStyle(fontFamily: 'RobotoMono')
);
```

#### TextStyle

如果[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)对象指定的是没有确切字体文件的字重或样式，则引擎将使用该字体的一个更通用的文件，并试图推断所请求的字重和样式。

## 完整示例

> Raleway和RobotoMono是从[Google Fonts](https://fonts.google.com/)下载的字体。

**`pubspec.yaml`**

```
name: custom_fonts
description: An example of how to use custom fonts with Flutter

dependencies:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
  uses-material-design: true
```

**`main.dart`**

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Custom Fonts',
      // 设置Raleway为默认字体 
      theme: new ThemeData(fontFamily: 'Raleway'),
      home: new MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      // AppBar将会使用Raleway默认字体
      appBar: new AppBar(title: new Text('Custom Fonts')),
      body: new Center(
        // 这个Text将会使用RobotoMono字体
        child: new Text(
          'Roboto Mono sample',
          style: new TextStyle(fontFamily: 'RobotoMono'),
        ),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/fonts.png)