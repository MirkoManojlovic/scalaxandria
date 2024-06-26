# Option

Option is a Scala typed used to model a scenario where there is no value present. It can help for start to think of it as a replacement for `null`, or as a list with just one element.


### Coming from Java
The equivalent of Option in Scala is Optional in Java.

##### Create an option
```scala
val option1 = Option("John")
val option2 = Some("John")
val option3 = None
```

##### Get value from an option
```scala
val nameOpt = Some("John")  
  
// map/getOrElse  
nameOpt  
  .map(name => name.toUpperCase)  
  .getOrElse("UNKNOWN")  
  
// fold  
nameOpt.fold("UNKNOWN")(name => name.toUpperCase())
```

##### Chaining two options
```scala
def findById(id: Long): Option[String] =
  Some("Titanic")

def findMovieScoreByName(movieName: String): Option[Double] =
  if (movieName == "Titanic") Some(5.0)
  else None

val movieScore1: Option[Double] =  
  findById(1)  
    .map(movieName => findMovieScoreByName(movieName)) // Option[Option[Double]  
    .flatten // Option[Double]  

val movieScore2: Option[Double] = findById(1).flatMap(movieName => findMovieScoreByName(movieName))
```
##### Object oriented way
```scala
val nameOpt = Some("John")

if (nameOpt.isDefined) nameOpt.get
else "No name is set"

if (nameOpt.isEmpty) "No name is set"
else nameOpt.get
```

## Parallel options  

##### Pattern matching
```scala
val serverPortOpt: Option[Int] = Some(9090)  
val serverAddressOpt: Option[String] = Some("localhost")  
  
(serverPortOpt, serverAddressOpt) match {  
  case (Some(port), Some(serverAddress)) => s"$serverAddress:$port"  
  case (None, Some(serverAddress))       => s"$serverAddress:8080"  
  case (Some(serverAddress), None)       => s"remoteHost:$serverPortOpt"  
  case (None, None)                      => s"remoteHost:8080"  
}  
```
##### OOP way
```scala
val serverPortOpt: Option[Int] = Some(9090)  
val serverAddressOpt: Option[String] = Some("localhost")  

val port: Int =  
  if (serverPortOpt.isDefined) serverPortOpt.get  
  else 8080  
  
val serverAddress: String =  
  if (serverAddressOpt.isDefined) serverAddressOpt.get  
  else "remoteHost"  
  
s"$serverAddress:$port" // localhost:9090  
```
##### Using getOrElse  
```scala
val serverPortOpt: Option[Int] = Some(9090)  
val serverAddressOpt: Option[String] = Some("localhost")  

val port = serverPortOpt.getOrElse(8080)  
val serverAddress = serverAddressOpt.getOrElse("remoteHost")  

s"$serverAddress:$port" // localhost:9090  
```

##### Using orElse with comprehension 
This is not really working in parallel since every time you have a [[for_compershension]] you are working with a flatMap
```scala
val serverPortOpt: Option[Int] = Some(9090)  
val serverAddressOpt: Option[String] = Some("localhost")  

for {  
  port <- serverPortOpt.orElse(Some(8080))  
  serverAddress <- serverAddressOpt.orElse(Some("remoteHost"))  
} yield s"$serverAddress:$port"  
```

##### Using orElse with flatMap/map
```scala
val serverPortOpt: Option[Int] = Some(9090)  
val serverAddressOpt: Option[String] = Some("localhost")  

serverPortOpt.orElse(Some(8080)).flatMap { port =>  
  serverAddressOpt.orElse(Some("remoteHost")).map(serverAddress => s"$serverAddress:$port")  
}
```

