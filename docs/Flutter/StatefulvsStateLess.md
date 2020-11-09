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


#h3 Stateless

If you want to redraw the Stateless Widget, then you will need to create a new instance of the widget.


#h3 Stateful
Stateful widgets have a mutable state, i.e., they are mutable and can be drawn multiple times within its lifetime.



<div class="code-example" markdown="1">
Default label
{: .label }

Blue label
{: .label .label-blue }

Stable
{: .label .label-green }

New release
{: .label .label-purple }

Coming soon
{: .label .label-yellow }

Deprecated
{: .label .label-red }
</div>


