# 后台解析JSON

默认情况下，Dart应用程序在一个线程上完成所有的工作。在许多情况下，这个模型简化了编码，而且速度足够快，不会导致应用程序性能差或动画卡顿，通常被称为“jank”。

但是，我们可能需要执行大量的计算，例如解析一个非常大的JSON文档。如果这项工作需要超过16毫秒，我们的用户将体验“jank”。

为了避免“jank”，我们需要在后台执行这样昂贵的计算。在Android上，这意味着在另一个线程上调度工作。在Flutter中，我们可以使用隔离的[Isolate](https://docs.flutter.io/flutter/dart-isolate/Isolate-class.html)。

## 步骤

1、 添加`http`包

2、 使用http包发出网络请求

3、 将响应转换为照片列表

4、 将此工作移动到隔离的Isolate

## 1. 添加`http`包

首先，我们要将[http](https://pub.dartlang.org/packages/http)包添加到我们的项目中。HTTP包可以更容易地执行网络请求，例如从JSON端点获取数据。

```
dependencies:
  http: <latest_version>
```

## 2. 使用http包发出网络请求

在本例中，我们将使用[`http.get()`](https://pub.dartlang.org/documentation/http/latest/http/get.html)方法从[JSONPlaceHolder REST API](https://jsonplaceholder.typicode.com/)获取包含5000个照片对象列表的JSON大型文档。

```dart
Future<http.Response> fetchPhotos(http.Client client) async {
  return client.get('https://jsonplaceholder.typicode.com/photos');
}
```

> 注意：我们为本例中的函数提供了`http.Client`。这将使该功能更容易在不同的环境中测试和使用！

## 3. 将响应转换为照片列表

接下来，按照[获取数据](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/networking/Fetch_data_from_the_internet.md)的指导，我们希望将`http.Response`转换为Dart对象列表。这将使数据在将来更容易处理。

### 创建一个Photo类

首先，我们需要创建一个包含照片数据的`Photo`类。我们还将包括一个`from Json`工厂函数，以便于创建一个以json对象开始的Photo。

```dart
class Photo {
    final int id;
    final String title;
    final String thumbnailUrl;

    Photo({this.id, this.title, this.thumbnailUrl});

    factory Photo.fromJson(Map<String, dynamic> json) {
        return Photo(
            id: json['id'] as int,
            title: json['title'] as String,
            thumbnailUrl: json['thumbnailUrl'] as String
        );
    }
}
```

### 将响应转换为照片列表

现在，我们将更新`fetchPhotos`函数，以便它能够返回`Future<list<Photo>>`。为此，我们需要：
    1. 创建将响应体转换为`List<Photo>`的`parsePhotos`方法
    2. 在`fetchPhotos`方法中使用`parsePhotos`方法

```dart
List<Photo> parsePhotos(String responseBody) {
  final parsed = json.decode(responseBody).cast<Map<String, dynamic>>();

  return parsed.map<Photo>((json) => Photo.fromJson(json)).toList();
}

Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response =
      await client.get('https://jsonplaceholder.typicode.com/photos');

  return parsePhotos(response.body);
}
```

## 4. 将此工作移动到隔离的Isolate

如果你在较慢的手机上运行了`fetchPhotos`功能，你可能会注意到这个应用程序在解析和转换json时会有片刻白屏。这就是“jank”，我们需要摆脱它！

那我们怎么做呢？通过使用Flutter提供的[`compute`](https://docs.flutter.io/flutter/foundation/compute.html)函数将解析和数据转换移动到后台隔离。`compute`函数将在后台隔离中运行昂贵的计算函数，并返回结果。在本例中，我们希望在后台运行`parsePhotos`函数！

```dart
Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response =
      await client.get('https://jsonplaceholder.typicode.com/photos');

  // 使用compute函数在单独的隔离中运行parsePhotos
  return compute(parsePhotos, response.body);
}
```

## 关于使用隔离(Isolate)的注意事项

隔离通过来回传递消息进行通信。这些消息可以是原始值，如`null`、`num`、`bool`、`double`或`String`，也可以是简单对象(如`List<Photo>`)。

如果尝试传递更复杂的对象，例如`Future`或`http.Response`，您可能会遇到错误。

## 完整示例

```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response =
      await client.get('https://jsonplaceholder.typicode.com/photos');

  return compute(parsePhotos, response.body);
}

List<Photo> parsePhotos(String responseBody) {
  final parsed = json.decode(responseBody).cast<Map<String, dynamic>>();

  return parsed.map<Photo>((json) => Photo.fromJson(json)).toList();
}

class Photo {
  final int albumId;
  final int id;
  final String title;
  final String url;
  final String thumbnailUrl;

  Photo({this.albumId, this.id, this.title, this.url, this.thumbnailUrl});

  factory Photo.fromJson(Map<String, dynamic> json) {
    return Photo(
      albumId: json['albumId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
      url: json['url'] as String,
      thumbnailUrl: json['thumbnailUrl'] as String,
    );
  }
}

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Isolate Demo';

    return MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  MyHomePage({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: FutureBuilder<List<Photo>>(
        future: fetchPhotos(http.Client()),
        builder: (context, snapshot) {
          if (snapshot.hasError) print(snapshot.error);

          return snapshot.hasData
              ? PhotosList(photos: snapshot.data)
              : Center(child: CircularProgressIndicator());
        },
      ),
    );
  }
}

class PhotosList extends StatelessWidget {
  final List<Photo> photos;

  PhotosList({Key key, this.photos}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GridView.builder(
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
      ),
      itemCount: photos.length,
      itemBuilder: (context, index) {
        return Image.network(photos[index].thumbnailUrl);
      },
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/isolate.gif)
