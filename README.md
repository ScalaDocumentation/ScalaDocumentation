# ScalaDocumentation

An alternative to ScalaDoc inspired by Hoogle.

(work in progress)

## What's Hoogle

In Haskell, functions have very precise types, so precise that if you don't know the name of a library function or even whether it exists, you can think of the type such a function would have, perform a search on that type, and if the function exists in some library, Hoogle will find it.

We would like to reproduce this workflow in Scala: you don't know the name of the trait/class/object/method you need, you describe what you need in some formal query language (not necessarily a type like in Hoogle), and ScalaDocumentation will find matching implementations in a bunch of libraries.
