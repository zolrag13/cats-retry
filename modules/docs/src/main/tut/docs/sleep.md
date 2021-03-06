---
layout: docs
title: Sleep
---

# Sleep

You can configure how the delays between retries are implemented.

This is done using the `Sleep` type class:

```scala
trait Sleep[M[_]] {

  def sleep(delay: FiniteDuration): M[Unit]

}
```

Out of the box, the core module provides instances for `Id` and `Future` that
simply do a `Thread.sleep(...)`.

```tut:book
import retry.Sleep
import cats.Id
import scala.concurrent.duration._

Sleep[Id].sleep(10.milliseconds)
```

```tut:book
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

Sleep[Future].sleep(10.milliseconds)
```

The `cats-effect` module provides an instance that uses a cats-effect
[`Timer`](https://typelevel.org/cats-effect/datatypes/timer.html).

```tut:book
import cats.effect.IO
import scala.concurrent.ExecutionContext.Implicits.global
import retry.CatsEffect._

Sleep[IO].sleep(10.milliseconds)
```

Being able to inject your own `Sleep` instance can be handy in tests, as you
can mock it out to avoid slowing down your unit tests.
