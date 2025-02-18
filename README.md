# eff-zio 
[![Build Status](https://travis-ci.org/takayahilton/eff-zio.png?branch=master)](https://travis-ci.org/takayahilton/eff-zio)
[![Maven Central](https://img.shields.io/maven-central/v/com.github.takayahilton/eff-zio_2.12.svg)](https://maven-badges.herokuapp.com/maven-central/com.github.takayahilton/eff-zio_2.12)

This library is [eff](https://github.com/atnos-org/eff) extension for [ZIO](https://github.com/zio/zio) effects.

# Installation

```
// check maven badge above for latest version
libraryDependencies += "com.github.takayahilton" %% "eff-zio" % "0.1.2"

// to write types like Reader[String, ?]
addCompilerPlugin("org.typelevel" %% "kind-projector" % "0.10.0")

// to get types like Reader[String, ?] (with more than one type parameter) correctly inferred for scala 2.11.11+ and 2.12.x
scalacOptions += "-Ypartial-unification"

// to get types like Reader[String, ?] (with more than one type parameter) correctly inferred for scala 2.11.9 and before
// you can use the [Typelevel Scala compiler](http://typelevel.org/scala)
scalaOrganization in ThisBuild := "org.typelevel"
```

# Usage

```scala
import com.github.takayahilton.eff.zio._
import org.atnos.eff._
import org.atnos.eff.all._
import org.atnos.eff.syntax.all._
import zio.{DefaultRuntime, UIO}

object Runtime extends DefaultRuntime

type S1 = Fx.fx2[UIO, Option]

def action[R: _uio: _option]: Eff[R, Int] =
  for {
    a <- succeedLazy(10)
    b <- succeedLazy(20)
  } yield a + b

val zio = action[S1].runOption.runAsync

Runtime.unsafeRun(zio) //Some(30)
```
