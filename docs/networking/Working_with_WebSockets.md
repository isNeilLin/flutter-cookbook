# WebSocket

除了正常的HTTP请求之外，我们还可以使用WebSocket连接到服务器。WebSocket允许与服务器进行双向通信，而无需轮询。

在本例中，我们将连接到[test server provided by websocket.org](http://www.websocket.org/echo.html)提供的测试服务器。服务器将简单地返回我们发送给它的消息！

## 指南

1、连接到WebSocket服务器

2、监听服务器返回的数据

3、向服务器发送数据

4、关闭WebSocket连接

## 1、连接到WebSocket服务器

[web_socket_channel](https://pub.dartlang.org/packages/web_socket_channel)包提供了我们需要连接到WebSocket服务器的工具

该软件包提供了一个`WebSocketChannel`允许我们监听服务器的信息以及推送消息到服务器。

在Flutter中，我们可以在一行中创建连接到服务器的`WebSocketChannel`：

```dart
final channel = new IOWebScoketChannel.connect('ws://echo.websocket.org');
```

## 2、监听服务器返回的数据

现在我们已经建立了连接，我们可以监听来自服务器的消息了。

在向测试服务器发送消息之后，它将返回相同的消息。

我们如何监听和显示消息？在本例中，我们将使用[`StreamBuilder`](https://docs.flutter.io/flutter/widgets/StreamBuilder-class.html)Widget侦听新消息，并使用`Text`Widget来显示它们。

```dart
new StreamBuilder(
  stream: widget.channel.stream,
  builder: (context, snapshot) {
    return new Text(snapshot.hasData ? '${snapshot.data}' : '');
  }
)
```

**这是怎么回事？**

`WebSocketChannel`提供来自服务器的消息流[`Stream`](https://docs.flutter.io/flutter/dart-async/Stream-class.html)。

`Stream`类是Dart的一个基本部分：`dart:async`包。它提供了一种从数据源侦听异步事件的方法。与`Future`返回单个异步响应不同，`Stream`类可以随时间传递许多事件。

[`StreamBuilder`](https://docs.flutter.io/flutter/widgets/StreamBuilder-class.html)Widget将连接到一个流，每次它使用给定的`builder`函数接收一个事件时，它都会请求Flutter进行重建！

## 3、向服务器发送数据

为了向服务器发送数据，我们将向`WebSocketChannel`提供的`sink`添加消息

```dart
channel.sink.add('Hello!');
```

**这是怎么回事？**

`WebSocketChannel`提供一个[`StreamSink`](https://docs.flutter.io/flutter/dart-async/StreamSink-class.html)将消息推送到服务器。

`StreamSink`类提供了向数据源添加同步或异步事件的一般方法。

## 4、关闭WebSocket连接

在我们使用完WebSocket之后，我们需要关闭连接！要做到这一点，我们可以关闭`sink`。

```dart
channel.sink.close();
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:web_socket_channel/io.dart';
import 'package:flutter/material.dart';
import 'package:web_socket_channel/web_socket_channel.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildConetxt context){
    final title = 'WebSocket Demo';
    return new MaterialApp(
      title: title,
      home: new MyHomePage(
        title: title,
        channel: new IOWebSocketChannel.connect('ws://echo.websocket.org')
      )
    );
  }
}

class MyHomePage extends StatefulWidget {
  final String title;
  final WebSocketChannel channel;
  
  MyHomePage({Key key, @required this.title, @required this.channel}) : super(key: key);
  
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  TextEditingController _controller = new TextEditingController();
  
  @override
  void dispose(){
    widget.channel.sink.close();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context){
    return new Scaffold(
      appBar: new AppBar(title:new Text(widget.title)),
      body: new Padding(
        padding: const EdgeInsets.all(20.0),
        child: new Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            new Form(
              child: new TextFormField(
                controller: _controller,
                decoration: new InputDecoration(labelText: 'Send a message'),
              )
            ),
            new StreamBuilder(
              stream: widget.channel.stream,
              builder: (context, snapshot) {
                return new Padding(
                  padding: const EdgeInsets.symmetric(vertical: 24.0),
                  child: new Text(snapshot.hasData ? '${snapshot.data}' : ''),
                );
              }
            ),
          ]
        )
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _sendMessage,
        tooltip: 'Send message',
        child: new Icon(Icons.send)
      )
    );
  }
  
  void _sendMessage() {
    if(_controller.text.isNotEmpty){
      widget.channel.sink.add(_controller.text);
    }
  }
}
```

![效果图](https://flutter.io/images/cookbook/web-sockets.gif)