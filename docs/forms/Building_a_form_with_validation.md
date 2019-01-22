# 使用验证构建表单

应用程序经常需要用户再文本框中输入信息。比如，我们可能正在开发一个应用程序，它要求我们的用户使用email和密码来登录。

为了使我们的程序更加安全和易用，我们可以检查用户提供的信息是否有效。如果用户在表单里填写了正确的信息，我们就可以提交这些信息。但如果用户提交了不正确的信息，我们可以显示一个友好的错误提示来告诉用户哪里出错了。

在这个例子中，我们将看到如何用单个文本字段向表单添加验证。

## 步骤

1、 创建一个带有`GlobalKey`的`Form`

2、 添加一个有验证逻辑的`TextFormField`

3、 在表单中添加一个验证和提交表单的按钮

## 1. 创建一个带有`GlobalKey`的`Form`

首先，我们需要一个[Form](https://docs.flutter.io/flutter/widgets/Form-class.html)。Form部件充当一个容器来对多个表单域进行分组和验证。

创建了form之后，我们同时需要提供一个[GlobalKey](https://docs.flutter.io/flutter/widgets/GlobalKey-class.html)。这将唯一地标识我们正在使用的表单，并允许我们在稍后的步骤中验证表单。

```dart
// 定义自定义的Form部件
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// 定义相应的状态类。这个类将保存与表单相关的数据
class MyCustomFormState extends State<MyCustomForm> {
  // 创建GlobalKey
  // 注意: 这是 `GlobalKey<FormState>`, 不是 GlobalKey<MyCustomFormState>!
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // 使用上面创建的_formKey构建一个表单小部件
    return Form(
      key: _formKey,
      child: // 我们将在接下来的步骤中建立子部件
    );
  }
}
```

## 2. 添加一个有验证逻辑的`TextFormField`

现在我们已经有了Form表单，但我们还没有为用户提供输入文本的方式。这正是[TextFormField](https://docs.flutter.io/flutter/material/TextFormField-class.html)要做的。`TextFormField`部件渲染了Material Design的文本输入框，并知道如何在出现验证错误时显示验证错误。

如何验证输入呢？通过向`TextFormField`提供`validator`函数。如果用户提供的信息出现错误，`validator`函数必须返回包含错误消息的字符串。如果没有错误，则函数不应返回任何内容。

在本例中，我们将创建一个`validator`，以确保TextFormField不为空。如果它是空的，我们将返回一个友好的错误消息。

```dart
TextFormField(
  // validator接受用户输入的文本作为参数
  validator: (value) {
    if (value.isEmpty) {
      return 'Please enter some text';
    }
  },
);
```

## 3. 在表单中添加一个验证和提交表单的按钮

现在我们有了一个带有文本字段的表单，我们需要提供一个按钮，用户可以点击这个按钮来提交信息。

当用户试图提交表单时，我们需要检查表单是否有效。如果是的话，我们将展示一个成功的信息。如果文本字段没有内容，我们需要显示错误消息。

```dart
RaisedButton(
  onPressed: () {
    // 如果表单有效，验证将返回true，如果表单无效，则返回false。
    if (_formKey.currentState.validate()) {
      // 如果表单有效，则显示一个snackbar。
      // 在现实世界中，您通常希望调用服务器或将信息保存在数据库中
      Scaffold
          .of(context)
          .showSnackBar(SnackBar(content: Text('Processing Data')));
    }
  },
  child: Text('Submit'),
);
```

## 它是如何工作的

为了验证表单，我们需要使用步骤1中创建的`_formKey`。我们可以使用`_formKey.currentState`方法来访问[FormState](https://docs.flutter.io/flutter/widgets/FormState-class.html)，在构建表单时，`FormState`是由Flutter自动创建的。

`FormState`类包含`validate`方法。当调用`validate`方法时，它将为表单中的每个文本字段运行`validator`函数。如果一切正常，该方法将返回true。如果任何文本字段包含错误，它将显示每个无效文本字段的错误消息并返回false。

## 完整示例

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Form Validation Demo';

    return MaterialApp(
      title: appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: Text(appTitle),
        ),
        body: MyCustomForm(),
      ),
    );
  }
}

// 创建Form部件
class MyCustomForm extends StatefulWidget {
  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// 定义相应的状态类。这个类将保存与表单相关的数据
class MyCustomFormState extends State<MyCustomForm> {
  // 创建GlobalKey
  // 注意: 这是 `GlobalKey<FormState>`, 不是 GlobalKey<MyCustomFormState>!
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // 使用上面创建的_formKey构建一个表单小部件
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            validator: (value) {
              if (value.isEmpty) {
                return 'Please enter some text';
              }
            },
          ),
          Padding(
            padding: const EdgeInsets.symmetric(vertical: 16.0),
            child: RaisedButton(
              onPressed: () {
                // 如果表单有效，则显示一个snackbar。
                // 在现实世界中，您通常希望调用服务器或将信息保存在数据库中
                if (_formKey.currentState.validate()) {
                  // If the form is valid, we want to show a Snackbar
                  Scaffold.of(context)
                      .showSnackBar(SnackBar(content: Text('Processing Data')));
                }
              },
              child: Text('Submit'),
            ),
          ),
        ],
      ),
    );
  }
}
```

![效果图](https://flutter.io/images/cookbook/form-validation.gif)
