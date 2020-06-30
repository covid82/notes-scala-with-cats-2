##### Export workspace directory path: 
```
export WORKSPACE_DIR=$HOME/dev/workspaces
```

##### Create an empty scala project `fps`:
```
cd $WORKSPACE_DIR
mkdir -p fps/project
mkdir -p fps/src/main/scala/io/covid82/fps
mkdir -p fps/src/test/scala/io/covid82/fps
```

##### Create `build.sbt`:
```
cd fps
touch build.sbt
```
with this content:
```scala
name := "fps"

version := "0.1"

scalaVersion := "2.13.3"

scalacOptions ++= Seq(
  "-feature",
  "-deprecation",
  "-unchecked",
  "-language:postfixOps",
  "-language:higherKinds",
  "-Ymacro-annotations")

libraryDependencies += "org.typelevel"     %% "simulacrum"      % "1.0.0"
libraryDependencies += "org.scalatest"     %% "scalatest"       % "3.2.0"   % Test
libraryDependencies += "org.scalacheck"    %% "scalacheck"      % "1.14.3"  % Test
libraryDependencies += "org.scalatestplus" %% "scalacheck-1-14" % "3.1.0.0" % Test

addCompilerPlugin("org.typelevel" %% "kind-projector" % "0.11.0" cross CrossVersion.full)
```
