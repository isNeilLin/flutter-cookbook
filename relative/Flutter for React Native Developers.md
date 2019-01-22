# React Native开发者Flutter指南

本文档是针对React Native开发人员的，他们希望应用现有的React Native知识构建移动应用程序。如果您了解React Native的基本原理，那么您可以使用这个文档作为您的Flutter开发的起点。

## 针对Javascript开发者的Dart介绍

React Native使用Javascript，而Flutter则使用一种名为[Dart](https://www.dartlang.org/)的语言。Dart是一种开源的、可扩展的编程语言，用于构建Web、服务器和移动应用程序。它是一种面向对象的单一继承语言，使用C风格的语法，AOT编译成本机，也可以选择编译成JavaScript。它支持接口、抽象类和强类型。

### 程序入口

虽然Javascript没有任何特定的入口函数，但是Dart(类似C语言)确实有一个名为`main()`的入口函数。

```javascript
// Javascript
function main(){
    //可以当作入口函数
}
// 但是必须被手动调用
main();
```

```dart
// Dart
main(){
  
}
```

### 打印到控制台

打印到控制台可以使用下面这种方式

```javascript
// Javascript
console.log('Level completed.')
```

```dart
// Dart
print('Hello world');
```

### 变量

> 创建变量并给变量赋值

虽然Javascript变量不能类型化，但Dart变量是可选类型的，使用类型化变量是一种很好的做法。
