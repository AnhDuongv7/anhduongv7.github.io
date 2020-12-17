---
layout: default
title: SOLID
parent: Developer
nav_order: 1
---


# SOLID Principles: Explanation and examples
[https://itnext.io/solid-principles-explanation-and-examples-715b975dcad4](https://itnext.io/solid-principles-explanation-and-examples-715b975dcad4)


## S — Single responsibility principle
In programming, the Single Responsibility Principle states that every module or class should have responsibility over a single part of the functionality provided by the software.
You may have heard the quote: “Do one thing and do it well”.
This refers to the single responsibility principle.
In the article Principles of Object Oriented Design, Robert C. Martin defines a responsibility as a ‘reason to change’, and concludes that a class or module should have one, and only one, reason to be changed.
Let’s do an example of how to write a piece of code that violates this principle.

```java
class User
{
    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            db.LogError("An error occured: ", ex.ToString());
            File.WriteAllText("\LocalErrors.txt", ex.ToString());
        }
    }
}
```

We notice how the CreatePost() method has too much responsibility, given that it can both create a new post, log an error in the database, and log an error in a local file.
This violates the single responsibility principle.
Let’s try to correct it.

```java
class Post
{
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            errorLogger.log(ex.ToString())
        }
    }
}

class ErrorLogger
{
    void log(string error)
    {
      db.LogError("An error occured: ", error);
      File.WriteAllText("\LocalErrors.txt", error);
    }
}
```

By abstracting the functionality that handles the error logging, we no longer violate the single responsibility principle.
Now we have two classes that each has one responsibility; to create a post and to log an error, respectively.

## O — Open/closed principle

In programming, the open/closed principle states that software entities (classes, modules, functions, etc.) should be open for extensions, but closed for modification.

If you have a general understanding of OOP, you probably already know about polymorphism. We can make sure that our code is compliant with the open/closed principle by utilizing inheritance and/or implementing interfaces that enable classes to polymorphically substitute for each other.

This may sound confusing, so let’s do an example that will make it very clear what I mean.

```java
class Post
{
    void CreatePost(Database db, string postMessage)
    {
        if (postMessage.StartsWith("#"))
        {
            db.AddAsTag(postMessage);
        }
        else
        {
            db.Add(postMessage);
        }
    }
}
```

In this code snippet we need to do something specific whenever a post starts with the character ‘#’.
However, the above implementation violates the open/closed principle in the way this code differs the behavior on the starting letter.
If we later wanted to also include mentions starting with ‘@’, we’d have to modify the class with an extra ‘else if’ in the CreatePost() method.

Let’s try to make this code compliant with the open/closed principle by simply using inheritance.

```java
class Post
{
    void CreatePost(Database db, string postMessage)
    {
        db.Add(postMessage);
    }
}

class TagPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        db.AddAsTag(postMessage);
    }
}
```

By using inheritance, it is now much easier to create extended behavior to the Post object by overriding the CreatePost() method.
The evaluation of the first character ‘#’ will now be handled elsewhere (probably at a higher level) of our software, and the cool thing is, that if we want to change the way a postMessage is evaluated, we can change the code there, without affecting any of these underlying pieces of behavior.


## L — Liskov substitution principle

This one is probably the hardest one to wrap your head around when being introduced for the first time.

In programming, the Liskov substitution principle states that if S is a subtype of T, then objects of type T may be replaced (or substituted) with objects of type S.
This can be formulated mathematically as

Let ϕ(x) be a property provable about objects x of type T.
Then ϕ(y) should be true for objects y of type S, where S is a subtype of T.

More generally it states that objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.

Let’s take a look at an example of how to violate this principle

```java
class Post
{
    void CreatePost(Database db, string postMessage)
    {
        db.Add(postMessage);
    }
}

class TagPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        db.AddAsTag(postMessage);
    }
}

class MentionPost : Post
{
    void CreateMentionPost(Database db, string postMessage)
    {
        string user = postMessage.parseUser();

        db.NotifyUser(user);
        db.OverrideExistingMention(user, postMessage);
        base.CreatePost(db, postMessage);
    }
}

class PostHandler
{
    private database = new Database();

    void HandleNewPosts() {
        List<string> newPosts = database.getUnhandledPostsMessages();

        foreach (string postMessage in newPosts)
        {
            Post post;

            if (postMessage.StartsWith("#"))
            {
                post = new TagPost();
            }
            else if (postMessage.StartsWith("@"))
            {
                post = new MentionPost();
            }
            else {
                post = new Post();
            }

            post.CreatePost(database, postMessage);
        }
    }
}
```

Observe how the call of CreatePost() in the case of a subtype MentionPost won’t do what it is supposed to do; notify the user and override existing mention.
Since the CreatePost() method is not overridden in MentionPost the CreatePost() call will simply be delegated upwards in the class hierarchy and call CreatePost() from its parent class.
Let’s correct this

```java
...

class MentionPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        string user = postMessage.parseUser();

        NotifyUser(user);
        OverrideExistingMention(user, postMessage)
        base.CreatePost(db, postMessage);
    }

    private void NotifyUser(string user)
    {
        db.NotifyUser(user);
    }

    private void OverrideExistingMention(string user, string postMessage)
    {
        db.OverrideExistingMention(_user, postMessage);
    }
}

...
```

By refactoring the MentionPost class such that we override the CreatePost() method rather than calling it on its base class, we no longer violate the Liskov substitution principle.
This is but one simple example of how to correct a violation of this principle, however, this situation can manifest in a broad variety of ways, and is not always easy to identify.

## I — Interface segregation principle

This principle is fairly easy to comprehend.
In fact, if you’re used to using interfaces, chances are that you’re already applying this principle.
If not, it’s time to start doing it!
In programming, the interface segregation principle states that no client should be forced to depend on methods it does not use.
Put more simply: Do not add additional functionality to an existing interface by adding new methods.
Instead, create a new interface and let your class implement multiple interfaces if needed.
Let’s look at an example of how to violate the interface segregation principle.

```java
interface IPost
{
    void CreatePost();
}

interface IPostNew
{
    void CreatePost();
    void ReadPost();
}
```
In this example, let’s pretend that I first have an IPost interface with the signature of a CreatePost() method.
Later on, I modify this interface by adding a new method ReadPost(), so it becomes like the IPostNew interface.
This is where we violate the interface segregation principle.
Instead, simply create a new interface.

```java
interface IPostCreate
{
    void CreatePost();
}

interface IPostRead
{
    void ReadPost();
}
```

If any class might need both the CreatePost() method and the ReadPost() method, it will implement both interfaces.

## D - Dependency inversion principle

Finally, we got to D, the last of the 5 principles.
In programming, the dependency inversion principle is a way to decouple software modules.
This principle states that
High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend on details. Details should depend on abstractions.
To comply with this principle, we need to use a design pattern known as a dependency inversion pattern, most often solved by using dependency injection.
Dependency injection is a huge topic and can be as complicated or simple as one might see the need for.
Typically, dependency injection is used simply by ‘injecting’ any dependencies of a class through the class’ constructor as an input parameter.
Let’s look at an example.

```java
class Post
{
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            errorLogger.log(ex.ToString())
        }
    }
}
```

Observe how we create the ErrorLogger instance from within the Post class.
This is a violation of the dependency inversion principle.
If we wanted to use a different kind of logger, we would have to modify the Post class.
Let’s fix this by using dependency injection.

```java
class Post
{
    private Logger _logger;

    public Post(Logger injectedLogger)
    {
        _logger = injectedLogger;
    }

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            _logger.log(ex.ToString());
        }
    }
}
```

By using dependency injection we no longer rely on the Post class to define the specific type of logger.


