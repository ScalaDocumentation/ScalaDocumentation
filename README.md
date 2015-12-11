# ScalaDocumentation

An alternative to ScalaDoc inspired by Hoogle.

(work in progress)

## What's Hoogle?

In Haskell, functions have very precise types, so precise that if you don't know the name of a library function or even whether it exists, you can think of the type such a function would have, perform a search on that type, and if the function exists in some library, Hoogle will find it.

We would like to reproduce this workflow in Scala: you don't know the name of the trait/class/object/method you need, you describe what you need in some formal query language (not necessarily a type like in Hoogle), and ScalaDocumentation will find matching implementations in a bunch of libraries.

## What kind of queries are supported?

Nothing is implemented yet, but here are examples of the kind of things we'd like to be able to search for.

### By method name

Even when I do know the name of the method because I'm looking at it, the ScalaDoc workflow for getting to the documentation of this method has a lot of room for improvement. First, I need to guess which library the method is from and find the ScalaDoc url for that library. Then, the ScalaDoc search box only searches for objects and classes, so I must instead click on the first letter of the method name, then use my browser's search functionality to find that name, open all the classes which are listed as containing that method, search for that method in each of those, and compare the method types to figure out which one I'm looking for.

The workflow I'd like to have instead: I type the name of the method, and I get back a list of method signatures with that name.

### By return type

I need to create a value of a particular class, say [`HttpRequest`](http://doc.akka.io/api/akka-stream-and-http-experimental/0.5/index.html#akka.http.model.HttpRequest). I'm looking at the ScalaDoc page for that class, so I know which constructors I could call. However, I suspect that there is a builder API somewhere (in this case [`RequestBuilder`](http://doc.akka.io/api/akka-stream-and-http-experimental/0.9/index.html#akka.http.client.RequestBuilding$RequestBuilder)) which would allow me to create this value in an easier way. I'd like to type some query which would return a bunch of methods that could produce an `HttpRequest`, such as those of `RequestBuilder`, which would be towards the top of the list because their return type is exactly `RequestBuilder`, and certainly also methods which return subtypes of `HttpRequest`, and those which return a `Future[HttpRequest]` or an `Option[HttpRequest]`, and if possible methods which return `this.Request` if it is statically known that the path-dependent type resolves to `HttpRequest`, but probably not `Predef.identity`, even though it could return an `HttpRequest` if I gave it one, and certainly not methods which take an `HttpRequest` as an argument and return something else.

### By polymorphic type

If I'm looking for a function to convert between Java lists and Scala lists, I'd like to be able to type `java.lang.List[A] => scala.collection.immutable.List[A]` and to obtain suitable conversion functions even if they happen to have chosen `T` instead of `A` for their pattern variable.

Ideally I'd like a query for `A => B` to match all of the following:

* `val f: A => B`
* `def f(a: A) => B`
* `val f: SuperclassOfA => SubclassOfB`
* `class A { val b: B }`
* `class A { def b(): B }`
* `case class RichA(a: A) { val b: B }`
* `trait Convert[AA,BB] { val f: AA => BB }; object AtoB extends Convert[A,B]`

I would also like the queries `A1 => A2 => B` and `A2 => A1 => B` to return the same set of results.
