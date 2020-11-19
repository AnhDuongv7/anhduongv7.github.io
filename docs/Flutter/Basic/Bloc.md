---
layout: default
title: Bloc
parent: Basic
grand_parent: Flutter
nav_order: 3
---



| Concepts        | Description |
|:------------------|:------------------|
|Streams|- A stream is a sequence of asynchronous data|
|Cubit|- A **[Cubit](https://bloclibrary.dev/#/coreconcepts?id=cubit)** is a special type of Stream which is used as the base for the Bloc class|
|BlocObserver||
|Bloc|A Bloc is a special type of Cubit which transforms `incoming events` into `outgoing states`.|
|[Data Layer](https://bloclibrary.dev/#/architecture?id=data-layer)|- The data layer's responsibility is to retrieve/manipulate data from one or more sources.<br>- This layer is the lowest level of the application and interacts with databases, network requests, and other asynchronous data sources.|
|[Data Provider](https://bloclibrary.dev/#/architecture?id=data-provider)|The data provider's responsibility is to provide raw data. The data provider should be generic and versatile.|
|[Repository](https://bloclibrary.dev/#/architecture?id=repository)|The repository layer is a wrapper around one or more data providers with which the Bloc Layer communicates.|

![My image Name](https://bloclibrary.dev/assets/bloc_architecture_full.png)


<small>[BloC Architecture in Flutter: a Modern Architectural Approach and How We Use it at Jimdo](https://medium.com/@artemsidorenko/bloc-architecture-in-flutter-a-modern-architectural-approach-and-how-we-use-it-at-jimdo-bea143b56d01)</small>


