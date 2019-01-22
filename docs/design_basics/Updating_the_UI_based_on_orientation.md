# 根据横竖屏方向更新UI

在某些情况下，当用户将屏幕从竖屏模式旋转到横屏模式时，更新应用程序的设计对用户可能会更友好。例如，我们可能希望在竖屏模式下垂直排列项目，在横屏模式中将这些项目水平排列。

在Flutter中，我们可以根据给定的[`Orientation`](https://docs.flutter.io/flutter/widgets/Orientation-class.html)构建不同的布局。在本例中，我们将构建一个列表，在竖屏模式下显示2列，在横屏模式中显示3列。

## 步骤

1、 构建一个两列的GridView

2、 使用`OrientationBuilder`改变列数

## 1. 构建一个两列的GridView

首先，我们需要使用的列表。与使用普通列表不同，我们需要一个在网格中显示项的列表。现在，我们将创建一个包含2列的网格。

```dart
GridView.count(
  // 一个两列的网格列表
  crossAxisCount: 2,
);
```

## 2. 使用`OrientationBuilder`改变列数

为了确定当前的方向，我们可以使用[`OrientationBuilder`](https://docs.flutter.io/flutter/widgets/OrientationBuilder-class.html)部件。`OrientationBuilder`通过比较父部件可用的宽度和高度来计算当前的方向，并在父部件的大小更改时重新构建。

使用`Orientation`，我们可以构建一个列表，在竖屏模式下显示2列，在横屏模式中显示3列。

```dart
OrientationBuilder(
  builder: (context, orientation) {
    return GridView.count(
      crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
    );
  },
);
```

注：如果您对屏幕的方向感兴趣，而不是对父部件可用的空间量感兴趣，请使用`MediaQuery.of(context).orientation`，而不是`OrientationBuilder`部件。

![效果图](https://flutter.io/images/cookbook/orientation.gif)
