# 内存中存储键值对

如果我们想要保存的键值相对较少，我们可以使用[shared_preferences](https://pub.dartlang.org/packages/shared_preferences)插件。

通常，我们必须编写本地平台集成来存储两个平台的数据。幸运的是，[shared_preferences](https://pub.dartlang.org/packages/shared_preferences)插件可以用于此。shared_preferences插件在iOS和Android上封装了`NSUserDefaults`和`SharedPreferences`，为简单数据提供了持久存储。

## 设置

在开始之前，我们需要将[shared_preferences](https://pub.dartlang.org/packages/shared_preferences)插件添加到`pubspec.yaml`文件中：

```
dependencies:
    flutter:
        sdk: flutter
    shared_perferences: "<newest version>"
```

## 存储数据

要持久化键值数据，我们可以使用`SharedPreferences`类。为了保存数据，我们调用`set`方法。注意，数据是异步保存的。如果我们希望在保存数据时得到通知，请使用`commit()`函数。

```dart
// 获取shared preferences
SharedPreferences prefs = await SharedPreferences.getInstance();

// 设置新值
prefs.setInt('counter', counter);
```

## 读取数据

```dart
SharedPreferences prefs = await SharedPreferences.getInstance();

int counter = (prefs.getInt('counter') ?? 0) + 1;
```

在上面的例子中，我们从`counter`键加载数据，如果它不存在，则返回0。

## 删除数据

```dart
SharedPreferences prefs = await SharedPreferences.getInstance();

prefs.remove('counter');
```

setter和getter方法可用于所有基本类型。

## 支持的数据类型

虽然使用键值存储简单方便，但它也有局限性：

- 只能使用基本类型
    - `int`, `double`, `bool`, `string` and `string list`

- 它不是设计用来存储大量数据的，所以它不适合作为应用程序缓存。

## 完整示例

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of our application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Shared preferences demo',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new MyHomePage(title: 'Shared preferences demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  @override
  void initState() {
    super.initState();
    _loadCounter();
  }

  //Loading counter value on start 
  _loadCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = (prefs.getInt('counter') ?? 0);
    });
  }
  
  //Incrementing counter after click
  _incrementCounter() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    _counter = (prefs.getInt('counter') ?? 0) + 1;
    setState(() {
      _counter;
    });
    prefs.setInt('counter', _counter);
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Text(
              'You have pushed the button this many times:',
            ),
            new Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```

## 测试支持

通过运行以下代码，我们可以在测试中使用初始值填充SharedPreferences：

```dart
const MethodChannel('plugins.flutter.io/shared_preferences')
  .setMockMethodCallHandler((MethodCall methodCall) async {
    if (methodCall.method == 'getAll') {
      return <String, dynamic>{}; // set initial values here if desired
    }
    return null;
  });
```

上面的代码应该放在`test`文件夹下的测试文件中。