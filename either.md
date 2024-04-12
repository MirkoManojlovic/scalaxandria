# Either

Either is a Scala type with two values left and right. As the name suggest it can be either left or right. Most used cased is for error handling.

##### Create an either
The simplest case is using a `String`/`Throwable` as a left side of an either and any other type as right hand value.

```scala
val either1: Either[String, Int] = Right(1)
val either2: Either[String, Int] = Left("This is an error")
val either3 = None
```

##### Get value form an either
```scala
val numberEither: Either[String, Int] = Right(1)  
  
// map/getOrElse  
numberEither  
  .map(number => number * 10)  
  .getOrElse(0)

numberEither
  .fold(
    error => {
      println(s"Something bad happened $error. Let's return 0")
      0
    },
    number => number * 10
  )
```

Or if you don't want to work with eithers, convert them to another type and work with that

[[option]]
```scala
numberEither.toOption
```

[[sequence]]
```scala
numberEither.toSeq
```

[[try]]
```scala
// when converting either to try left type must be a throwable
val numberEitherThrowable: Either[Throwable, Int] = Right(1)
numberEitherThrowable.toTry
```

### Coming from Java
Think of Eithers as a replacement for checked exceptions. Example

```privade doSomething
```