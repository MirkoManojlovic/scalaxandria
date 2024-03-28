# Options

##### Create an option
```scala
val optionNumber1 = Option("John")
val optionNumber2 = Some("John")
val optionNumber3 = None
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

