# Using .g8 Templates for Quick Scaffolding

[Giter8](http://www.foundweekends.org/giter8) is a command line tool to generate files and directories from templates published on Github or any other git repository. It’s implemented in Scala and runs through the sbt launcher, but it can produce output for any purpose.

## Requirements
### SBT (Simple Build Tool) must be installed.
sbt is a build tool for Scala and Java.  It requires Java 1.8 or later.
#### Installing SBT
##### Windows
The easiest way to do this is to use the [Windows Installer](https://github.com/sbt/sbt/releases/download/v1.1.0/sbt-1.1.0.1.msi)

##### Mac
Use homebrew:

```
brew install sbt@1
```

##### Linux
```
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
sudo apt-get update
sudo apt-get install sbt
```

## Getting Started

### Creating your First Scala Project Using .g8 Templates

Create a directory called ~/kickoff/hackathon-2018/scala/

```
droach at macbook-13 in ~/kickoff
$ mkdir hackathon-2018
droach at macbook-13 in ~/kickoff
$ ls
hackathon      hackathon-2018 site           target
droach at macbook-13 in ~/kickoff
$ cd hackathon-2018
droach at macbook-13 in ~/kickoff/hackathon-2018
$ cd scala
droach at macbook-13 in ~/kickoff/hackathon-2018/scala
$ sbt new sbt/scala-seed.g8
WARN: No sbt.version set in project/build.properties, base directory: /Users/droach/kickoff/hackathon-2018/scala
[warn] Executing in batch mode.
[warn]   For better performance, hit [ENTER] to switch to interactive mode, or
[warn]   consider launching sbt without any commands, or explicitly passing 'shell'
[info] Loading global plugins from /Users/droach/.sbt/0.13/plugins
[info] Updating {file:/Users/droach/.sbt/0.13/plugins/}global-plugins...
[info] Resolving org.fusesource.jansi#jansi;1.4 ...
[info] Done updating.
[info] Set current project to scala (in build file:/Users/droach/kickoff/hackathon-2018/scala/)

A minimal Scala project.

name [Scala Seed Project]: hello-world

Template applied in ./hello-world
```


You should now see the hello-world and target directories
```
droach at macbook-13 in ~/kickoff/hackathon-2018/scala
$ ls
hello-world target
```
Change directory to hello-world and you will see a build.sbt file.
```
droach at macbook-13 in ~/kickoff/hackathon-2018/scala
$ cd hello-world
droach at macbook-13 in ~/kickoff/hackathon-2018/scala/hello-world
$ ls
build.sbt project   src
```
From here you can use sbt to compile and run the new project.  When you compile for the first time, it will download a bunch of necessary artifacts and update the projects dependencies.  It will then compile.

```
sbt compile

Downloading sbt launcher for 1.0.2:
  From  http://repo.scala-sbt.org/scalasbt/maven-releases/org/scala-sbt/sbt-launch/1.0.2/sbt-launch.jar
    To  /Users/droach/.sbt/launchers/1.0.2/sbt-launch.jar
Getting org.scala-sbt sbt 1.0.2 ...
downloading https://repo1.maven.org/maven2/org/scala-sbt/ivy/ivy/2.3.0-sbt-a3314352b638afbf0dca19f127e8263ed6f898bd/ivy-2.3.0-sbt-a3314352b638afbf0dca19f127e8263ed6f898bd.jar ...
        [SUCCESSFUL ] org.scala-sbt.ivy#ivy;2.3.0-sbt-a3314352b638afbf0dca19f127e8263ed6f898bd!ivy.jar (720ms)
:: retrieving :: org.scala-sbt#boot-app
        confs: [default]
        69 artifacts copied, 0 already retrieved (22000kB/139ms)
Getting Scala 2.12.3 (for sbt)...
:: retrieving :: org.scala-sbt#boot-scala
        confs: [default]
        5 artifacts copied, 0 already retrieved (19004kB/48ms)
[info] Loading project definition from /Users/droach/kickoff/hackathon-2018/scala/hello-world/project
[info] Updating {file:/Users/droach/kickoff/hackathon-2018/scala/hello-world/project/}hello-world-build...
[info] Done updating.
[info] Compiling 1 Scala source to /Users/droach/kickoff/hackathon-2018/scala/hello-world/project/target/scala-2.12/sbt-1.0/classes ...
[info] Non-compiled module 'compiler-bridge_2.12' for Scala 2.12.3. Compiling...
[info]   Compilation completed in 6.876s.
[info] Done compiling.
[info] Loading settings from build.sbt ...
[info] Set current project to Hello (in build file:/Users/droach/kickoff/hackathon-2018/scala/hello-world/)
[info] Executing in batch mode. For better performance use sbt's shell
[info] Updating {file:/Users/droach/kickoff/hackathon-2018/scala/hello-world/}root...
[info] downloading https://repo1.maven.org/maven2/org/scalatest/scalatest_2.12/3.0.3/scalatest_2.12-3.0.3.jar ...
[info] downloading https://repo1.maven.org/maven2/org/scalactic/scalactic_2.12/3.0.3/scalactic_2.12-3.0.3.jar ...
[info]  [SUCCESSFUL ] org.scalactic#scalactic_2.12;3.0.3!scalactic_2.12.jar(bundle) (405ms)
[info]  [SUCCESSFUL ] org.scalatest#scalatest_2.12;3.0.3!scalatest_2.12.jar(bundle) (593ms)
[info] Done updating.
[info] Compiling 1 Scala source to /Users/droach/kickoff/hackathon-2018/scala/hello-world/target/scala-2.12/classes ...
[info] Done compiling.
[success] Total time: 1 s, completed Jan 15, 2018 9:43:58 PM

```

When the project is done compiling, you can:

 ``` sbt run ```

If all goes well, you'll see this:

```
[info] Loading project definition from /Users/droach/kickoff/hackathon-2018/scala/hello-world/project
[info] Loading settings from build.sbt ...
[info] Set current project to Hello (in build file:/Users/droach/kickoff/hackathon-2018/scala/hello-world/)
[info] Packaging /Users/droach/kickoff/hackathon-2018/scala/hello-world/target/scala-2.12/hello_2.12-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[info] Running example.Hello
[debug] Waiting for threads to exit or System.exit to be called.
[debug]   Classpath:
[debug]         /var/folders/mc/yzvq76451x3f02r9f90sgjsm0000gn/T/sbt_331c23c4/job-1/target/ca7c12ca/hello_2.12-0.1.0-SNAPSHOT.jar
[debug]         /var/folders/mc/yzvq76451x3f02r9f90sgjsm0000gn/T/sbt_331c23c4/target/f2e496f2/scala-library.jar
hello
[debug] Interrupting remaining threads (should be all daemons).
[debug] Sandboxed run complete..
[debug] Exited with code 0
[success] Total time: 0 s, completed Jan 15, 2018 9:46:20 PM
```

