# 发送数据到新页面

通常，我们不仅希望导航到一个新Screen，而且还将一些数据传递给Screen。例如，我们通常希望传递有关我们所使用的信息。

记住：Screen只是Widget。在本例中，我们将创建一个Todos列表。当一个todo被点击时，我们将导航到一个新Screen(Widget)，该Screen显示有关todo的信息。

## 指南

1、定义Todo类

2、显示Todos列表

3、常见可以显示Todo信息的Detail Screen

4、导航并且传递数据到Detail Screen

## 1、定义Todo类

首先，我们需要一种简单的方法来表示Todos。对于这个例子，我们将创建一个类，它包含两个数据：标题和描述。

```dart
class Todo {
  final String title;
  final String description;
  Todo(this.title, this.description);
}
```

## 2、显示Todos列表

然后，我们要显示一张待办事项清单。在本例中，我们将生成20个todo并使用ListView显示它们。

**生成Todos列表**

```dart
final todos = new List<Todo.generate(
  20,
  (i)=> new Todo(
    'Todo ${i}',
    'A description of waht needs to be done for Todo ${i}'
  )
);
```

**使用ListView显示Todos列表**

```dart
new ListView(
  itemCount: 20,
  itemBuilder: (context, index){
    return new ListTile(
      title: new Text(todos[index].title)
    );
  }
)
```
到目前为止，很好。我们将会生成20个待办事项并将其显示在ListView中！

## 3、常见可以显示Todo信息的Detail Screen

现在，我们将创建第二个Screen。Screen标题将包含待办事项的标题，Screen正文将显示描述。

因为它是一个普通的`StatelessWidget`，我们只需要创建Screen的用户传递`Todo`！然后，我们将使用给定的Todo构建一个UI。

```dart
class DetailScreen extends StatelessWidget {
  // 定义一个变量接收Todo
  final Todo todo;

  DetailScreen({Key key, @required this.todo}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 使用Todo创建UI
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("${todo.title}"),
      ),
      body: new Padding(
        padding: new EdgeInsets.all(16.0),
        child: new Text('${todo.description}'),
      ),
    );
  }
}
```

## 4、导航并且传递数据到Detail Screen

在我们的`DetailScreen`就位后，我们已经准备好执行导航任务了！在我们的例子中，当用户点击列表中的Todo时，我们希望导航到`DetailScreen`。当我们这样做的时候，我们也想把Todo传递给`DetailScreen`。

为了实现这一点，我们将为`ListTile`Widget编写一个`onTap`回调。在`onTap`回调中，我们将再次使用`Navigator.push`方法。

```dart
new ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context,index){
    return new ListTile(
      title: new Text(todos[index].title),
      //当用户点击ListTile时，导航到DetailScreen。
      //注意，我们不仅导航到一个新的DetailScreen，我们还还将当前的Todo传递给它！
      onTap: (){
        Navigator.push(
          context,
          new MaterialPageRoute(
            builder: (context)=> new DetailScreen(todo: todos[index])
          )
        )
      }
    );
  }
)
```

## 完整示例

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

class Todo {
  final String title;
  final String description;

  Todo(this.title, this.description);
}

void main() {
  runApp(new MaterialApp(
    title: 'Passing Data',
    home: new TodosScreen(
      todos: new List.generate(
        20,
        (i) => new Todo(
              'Todo $i',
              'A description of what needs to be done for Todo $i',
            ),
      ),
    ),
  ));
}

class TodosScreen extends StatelessWidget {
  final List<Todo> todos;

  TodosScreen({Key key, @required this.todos}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Todos'),
      ),
      body: new ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return new ListTile(
            title: new Text(todos[index].title),
            //当用户点击ListTile时，导航到DetailScreen。
            //注意，我们不仅导航到一个新的DetailScreen，我们还还将当前的Todo传递给它！
            onTap: () {
              Navigator.push(
                context,
                new MaterialPageRoute(
                  builder: (context) => new DetailScreen(todo: todos[index]),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  // 定义一个变量接收Todo
  final Todo todo;

  DetailScreen({Key key, @required this.todo}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 使用Todo创建UI
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("${todo.title}"),
      ),
      body: new Padding(
        padding: new EdgeInsets.all(16.0),
        child: new Text('${todo.description}'),
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/passing-data.gif)