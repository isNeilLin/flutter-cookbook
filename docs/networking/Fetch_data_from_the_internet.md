# 获取数据

从互联网上获取数据对于大多数应用程序来说都是必要的。幸运的是，Dart和Flutter为这类工作提供了工具！

## 指南

1、使用`http`包创建网络请求

2、将响应转换为自定义的Dart对象

3、用Flutter获取和显示数据

## 1、使用`http`包创建网络请求

[`http`](https://pub.dartlang.org/packages/http)包提供了从Internet获取数据的最简单方法。

在本例中，我们将使用[`http.get`](https://docs.flutter.io/flutter/package-http_http/package-http_http-library.html)方法从[`JSONPlaceholder REST API`](https://jsonplaceholder.typicode.com/)中获取一个示例数据。

```dart
Future<http.Response> fetchPost(){
  return http.get('https://jsonplaceholder.typicode.com/posts/1');
} 
```
`http.get`方法返回包含`Response`的`Future`

- [`Future`](https://docs.flutter.io/flutter/dart-async/Future-class.html)是用于处理异步操作的核心Dart类。它用于表示将来某个时候可能出现的值或错误。

- `http.Response`类包含从成功的http调用中接收的数据。

## 2、将响应转换为自定义的Dart对象

虽然发起网络请求很容易，但是使用原始的`Future<http.Response>`并不十分方便。为了简化，我们可以将`http.Response`转换成我们自己的Dart对象。

**创建一个`Post`类**

首先，我们需要创建一个`Post`类，它包含来自网络请求的数据。它还将包括一个工厂构造函数，允许我们从json创建一个`Post`。

手动转换JSON只是一种选择。有关更多信息，请参见[`JSON and serialization`](https://flutter.io/json/)

```dart
class Post {
  final int userId;
  final int id;
  final String title;
  final String body;
  
  Post({this.userId, this.id, this.title, this.body});
  
  factory Post.fromJson(Map<String, dynamic> json){
    return new Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body']
    );
  }
}
```

**将`http.Response`转化为`Post`**

现在，我们将更新`fetchPost`函数以返回`Future<Post>`。为此，我们需要：

1、使用`dart:convert`包将响应体转换为json`Map`

2、使用`fromJson`工厂函数将json`Map`转换为`Post`

```dart
Future<Post> fetchPost() async {
  final response = await http.get('https://jsonplaceholder.typicode.com/posts/1');
  final json = json.decode(response.body);
  
  return new Post.fromJson(json); 
}
```

终于！我们有了一个功能，可以调用从互联网上获取数据！

## 3、用Flutter获取和显示数据

为了获取数据并显示在屏幕上，我们可以使用[`FutureBuilder`](https://docs.flutter.io/flutter/widgets/FutureBuilder-class.html)Widget！Flutter附带了`FutureBuilder`Widget，使得使用异步数据源变得很容易。

我们必须提供两个参数：

1、我们要使用的`Future`，在本例中，我们将调用`fetchPost()`方法

2、一个`builder`函数，它根据`Future`的状态，告诉Flutter如何渲染：加载、成功或错误。

```dart
new FutureBuilder<Post>(
  future: fetchPost(),
  builder: (context, snapshot) {
    if (snapshot.hasData){
      return new Text(snapshot.data.title);
    } else if (snapshot.hasError) {
      return new Text("${snapshot.error}");
    }
    // 默认显示加载器
    return new CircularProgressIndicator();
  }
)
```

## 完整示例

```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http:/http.dart' as http;

Future<Post> fetchPost() async{
  final response = await http.get('https://jsonplaceholder.typicode.com/posts/1');
  final json = json.decode(response.body);
  
  return new Post.fromJson(json);
}

class Post {
  final int userId;
  final int id;
  final String title;
  final String body;
  
  Post({this.userId, this.id, this.title, this.body});
  
  factory Post.fromJson(Map<String, dynamic> json){
    return new Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body']
    );
  }
}

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context){
    return new MaterialApp(
      title: 'Fetch Data Example',
      theme: new ThemeData(
        primarySwatch: Colors.blue
      ),
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Fetch Data Example')
        ),
        body: new Center(
          child: new FutureBuilder<Post>(
            future: fetchpost(),
            builder: (context, snapshot){
              if (snapshot.hasData){
                return new Text(snapshot.data.title);
              } else if (snapshot.hasError) {
                return new Text("${snapshot.error}");
              }
              // 默认显示加载器
              return new CircularProgressIndicator();  
            }
          ),
        ),
      ),
    );
  }
}
```