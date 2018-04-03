# 读写文件

在某些情况下，将文件读写到磁盘是很方便的。这可以用于在应用程序启动时将数据持久化，也可以从互联网下载数据，并将其保存起来供以后离线使用。

为了将文件保存到磁盘，我们需要将[`path_provider`](https://pub.dartlang.org/packages/path_provider)插件与`dart:io`库结合起来。

## 指南

1、找到正确的本地路径

2、创建对文件位置的引用

3、将数据写入文件

4、从文件中读取数据

## 1、找到正确的本地路径

在本例中，我们将显示一个计数器。当计数器发生变化时，我们希望在磁盘上写入数据，以便在应用程序加载时再次读取数据。因此，我们需要问：我们应该把这些数据存储在哪里？

`path_provider`插件提供了一种与平台无关的方式来访问设备文件系统上常用的位置。该插件目前支持访问两个文件系统位置：

- **Temporary directory**：系统可以随时清除的临时目录(缓存)。在IOS上，这对应于`NSTemporaryDirectory()`返回的值。在Android上，这是`getCacheDir()`返回的值。

- **Documents directory**：一个目录，用于存储只有应用程序才能访问的文件。只有当应用程序被删除时，系统才会清除该目录。在IOS上，这对应于`NSDocumentDirectory`。在Android上，这是`AppData`目录。

在我们的例子中，我们希望将信息存储在*Documents directory*目录中！我们可以找到到*Documents directory*目录的路径，如下所示：

```dart
Future<String> get _localPath async {
  final directory = await getApplicationDocumentsDirectory();
  return directory.path;
}
```

## 2、创建对文件位置的引用

一旦我们知道了在哪里存储文件，我们就需要创建一个对文件的完整位置的引用。我们可以使用`dart:io`库中的[`File`](https://docs.flutter.io/flutter/dart-io/File-class.html)类来实现这一点。

```dart
Future<File> get _localFile async {
  final path = await _localPath;
  return new File('${path}/counter.txt');
}
```

## 3、将数据写入文件

现在我们有了一个`File`可以处理，我们可以使用它来读取和写入数据！首先，我们将把一些数据写入文件。因为我们使用的是计数器，所以我们只需将整数存储为字符串。

```dart
Future<File> writeCounter(int counter) async {
  final file = await _localFile;
  return file.writeAsString('$counter');
}
```

## 4、从文件中读取数据

现在我们有一些数据在磁盘上，我们可以读取它！同样，我们将使用`File`类来完成此操作。

```dart
Future<int> readConter() async {
  try{
      final file = await _localFile;
      //读取文件
      String contents = file.readAsString();
      
      return int.parse(contents);
  } catch(e) {
      // 如果发生错误，返回0
      return 0;
  }
}
```

## 测试

为了测试与文件交互的代码，我们需要模拟对`MethodChannel`的调用。`MethodChannel`是Flutter使用与主机平台通信的类。

在我们的测试中，我们不能与设备上的文件系统交互。我们需要与测试环境的文件系统交互！

为了模拟方法调用，我们可以在测试文件中提供一个`setupAll`函数。此函数将在执行测试之前运行。

```dart
setUpAll(() async {
  // 创建临时工作目录
  final directory = await Directory.systemTemp.createTemp();
  
  const MethodChannel('plugins.flutter.io/path_provider')
      .setMockMethodCallHandler((MethodCall methodCall) async {
    if (methodCall.method == 'getApplicationDocumentsDirectory') {
      return directory.path;
    }
    return null;
  });
});
```

## 完整示例

```dart
import 'dart:async';
import 'dart:io';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';

void main() {
  runApp(
    new MaterialApp(
      title: 'Reading and Writing Files',
      home: new FlutterDemo(storage: new CounterStorage()),
    ),
  );
}

class CounterStorage {
  Future<String> get _localPath async {
    final directory = await getApplicationDocumentsDirectory();

    return directory.path;
  }

  Future<File> get _localFile async {
    final path = await _localPath;
    return new File('$path/counter.txt');
  }

  Future<int> readCounter() async {
    try {
      final file = await _localFile;

      // Read the file
      String contents = await file.readAsString();

      return int.parse(contents);
    } catch (e) {
      // If we encounter an error, return 0
      return 0;
    }
  }

  Future<File> writeCounter(int counter) async {
    final file = await _localFile;

    // Write the file
    return file.writeAsString('$counter');
  }
}

class FlutterDemo extends StatefulWidget {
  final CounterStorage storage;

  FlutterDemo({Key key, @required this.storage}) : super(key: key);

  @override
  _FlutterDemoState createState() => new _FlutterDemoState();
}

class _FlutterDemoState extends State<FlutterDemo> {
  int _counter;

  @override
  void initState() {
    super.initState();
    widget.storage.readCounter().then((int value) {
      setState(() {
        _counter = value;
      });
    });
  }

  Future<File> _incrementCounter() async {
    setState(() {
      _counter++;
    });

    // write the variable as a string to the file
    return widget.storage.writeCounter(_counter);
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(title: new Text('Reading and Writing Files')),
      body: new Center(
        child: new Text(
          'Button tapped $_counter time${_counter == 1 ? '' : 's'}.',
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      ),
    );
  }
}
```