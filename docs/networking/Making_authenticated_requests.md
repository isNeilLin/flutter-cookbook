# 授权认证

为了从许多Web服务中获取数据，您需要提供授权。有许多方法可以做到这一点，但最常见的可能是使用`Authorization`HTTP header。

## 添加授权header

`http`包提供了一种向请求中添加header的便捷方法。您还可以利用用于`HttpHeaders`的`dart:io`包

```dart
Future<http.Response> fetchPost() {
  return http.get(
    'https://jsonplaceholder.typicode.com/posts/1',
    // 向后端发送授权头
    headers: {
      HttpHeaders.AUTHORIZATION: "Basic your_api_token_here",
    }
  );
}
```

## 完整示例

此示例构建在[获取数据](https://github.com/isNeilLin/flutter-cookbook/tree/master/docs/networking/Fetch_data_from_the_internet.md)的基础上。

```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';
import 'package:http/http.dart' as http;

Future<Post> fetchPost() async {
  final response = await http.get(
    'https://jsonplaceholder.typicode.com/posts/1',
    // 向后端发送授权头
    headers: {
      HttpHeaders.AUTHORIZATION: "Basic your_api_token_here",
    }
  );
  final json = json.decode(response);
  return new Post.fromJson(json);
}

class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return new Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}
```