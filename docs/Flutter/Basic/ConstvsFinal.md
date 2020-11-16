---
layout: default
title: Dart - Const vs Final in Dart
parent: Basic
grand_parent: Flutter
nav_order: 1
---

## Dart - Const vs Final in Dart
{: .d-inline-block }

Basic
{: .label .label-blue }

Dart provides the user to set the value of a certain variable as constants.

1. `const` keyword
2. `final` keyword


### Compare

| const        | final |
|:------------------|:------------------|
| Deeply, transitively immutable | "final" means single-assignment: a final variable or field must have an initializer. Once assigned a value, a final variable's value cannot be changed. final modifies variables.  |
|Should use at `complile time`| Should use at `runtime`|


### Example
```dart
final int age = 10;
age = 100; /// Will throw error

const String name = 'Ahihihi';
name = "Jack"; /// Will throw error as well
```

Usage `const`
It is used to increase Flutter performance as it helps Flutter to rebuild only widgets that should be updated. Means while using setState() in StateFulWidget, only those components will be rebuild that are non const constructor.

```dart
class _MyWidgetState extends State<MyWidget> {

  String title = "Title";

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Column(
        children: <Widget>[
          const Text("Text 1"),
          const Padding(
            padding: const EdgeInsets.all(8.0),
            child: const Text("Another Text widget"),
          ),
          const Text("Text 3"),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        child: const Icon(Icons.add),
        onPressed: () {
          setState(() => title = 'New Title');
        },
      ),
    );
  }
}

```








<small>[Dart – Const And Final Keyword](https://www.geeksforgeeks.org/dart-const-and-final-keyword/?ref=rp)</small>


