---
layout: default
title: Stateful vs Stateless Widget
parent: Flutter
nav_order: 6
---

As you know in Flutter all UI components are known as widgets.
The widget which contains the code for a single screen of the app can be just 2 types:
- Stateful
- Stateless


### Stateless

If you want to redraw the Stateless Widget, then you will need to create a new instance of the widget.


### Stateful
Stateful widgets have a mutable state, i.e., they are mutable and can be drawn multiple times within its lifetime.

```dart
import 'package:flutter/material.dart';

class StartScreen extends StatefulWidget {
    @override
    _StartScreenState createState() -> _StartScreenState();
}

class _StartScreenState extends State<StartScreen> {
    @override
    Widget build(BuildContext context) {
        return Container(
            // Here you can design the UI of this stateful screen
        );
    }
}
```

