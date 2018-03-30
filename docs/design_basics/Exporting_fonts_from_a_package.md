# 从包导出字体

与其将字体声明为应用程序的一部分，我们还可以将字体声明为单独包的一部分。这是一种方便的方式，可以在不同的项目之间共享字体或者将包发布到[pub website](https://pub.dartlang.org/)。

## 指南

1、将字体添加到包中

2、将包和字体添加到应用程序中

3、使用字体

## 1、将字体添加到包中

要从包导出字体，我们需要将字体文件导入到包项目的`lib`文件夹中。我们可以将字体文件直接放置在`lib`文件夹或子目录中，例如`lib/fonts`。

在本例中，我们假定我们有一个名为`awesome_package`的Flutter库，其中的字体位于`lib/fonts`文件夹中。

```
awesome_package/
  lib/
    awesome_package.dart
    fonts/
      Raleway-Regular.ttf
      Raleway-Italic.ttf
```

## 2、将包和字体添加到应用程序中

我们现在可以使用包并使用它提供的字体，这涉及更新应用程序跟目录中的`pubspec.yaml`文件。

**添加包到项目中**

```
dependencies:
    awesome_package: <last_version>
```

**声明字体**

现在我们已经导入了包，我们需要告诉Flutter在哪里可以从`awesome_package`中找到字体。

要声明字体包，我们必须在字体的路径前加上`package/awesome_package`.这将告诉Flutter查看`lib`文件夹中包的字体。

```
flutter:
    fonts:
        - family: Raleway
          fonts:
            - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
            - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
              style: italic  
```

## 3、使用字体

我们可以使用[`TextStyle`](https://docs.flutter.io/flutter/painting/TextStyle-class.html)来更改文本的外观。要使用包字体，我们不仅需要声明要使用的字体，还需要声明字体所属的包。

```dart
new Text(
  'Using the Raleway font from the awesome_package',
  style: new TextStyle(
    family: 'Raleway',
    package: 'awesome_package',
  ),
)
```

## 完整示例

> Raleway和RobotoMono是从[Google Fonts](https://fonts.google.com/)下载的字体。

**`pubspec.yaml`**

```
name: package_fonts
description: An example of how to use package fonts with Flutter

dependencies:
  awesome_package:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
  uses-material-design: true
```

**`main.dart`**

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'Package Fonts',
      home: new MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Package Fonts')
      ),
      body: new Center(
        child: new Text(
          'Using the Raleway font from the awesome_package',
          style: new TextStyle(
            family: 'Raleway',
            package: 'awesome_package'
          )
        )
      )
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/package-fonts.png)