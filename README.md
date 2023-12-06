# Learning about Kotlin's [interactive shell, KI](https://github.com/Kotlin/kotlin-interactive-shell)

This is just a place for me to document what I'm learning about Kotlin's KI shell.


## Define a Plugin
Ideas for plugin would be to auto-load a private maven repository with credentials loaded, so that the user wouldn't have to type in `:repository ...` whenever they start the KI shell.

```kotlin
package com.dt2js.ki.plugins

import org.jetbrains.kotlinx.ki.shell.Plugin
import org.jetbrains.kotlinx.ki.shell.Shell
import org.jetbrains.kotlinx.ki.shell.configuration.ReplConfiguration
import org.jetbrains.kotlinx.ki.shell.BaseCommand
import org.jetbrains.kotlinx.ki.shell.Command


open class RandomPlugin : Plugin {
  override fun init(repl: Shell, config: ReplConfiguration) {
    repl.registerCommand(HelloCommand(config))
  }

  override fun cleanUp() {
  }

  override fun sayHello() {
    println("hello from plugin")
  }

  inner class HelloCommand(val conf: ReplConfiguration) : BaseCommand() {

    override val name: String by conf.get(default = "hello")
    override val short: String by conf.get(default = "howdy")
    override val description: String = "say hello from my first plugin"

    override val params = "<this is just random>"

    override fun execute(line: String) : Command.Result {

      return Command.Result.Success("it worked!!!")

    }
  }

}

```

## Create a custom configuration class
KI appears to allow you to override the configuration class by passing in a system property for `config.class`. For example: `-Dconfig.class=com.dt2js.ki.conf.MyFantasticConfiguration`.

## Compile the configuration class
Compile the configuration class. 

## Define a custom script to add add config jar to classpath and specify this configuration class via system property
```
java -cp ./path/to/configuration.jar \
-jar \
-Dconfig.class=com.dt2js.ki.conf.MyFantasticConfiguration /usr/local/Cellar/ki/0.5.2/libexec/ki-shell.jar
```

## Ideas
Projects could pull in KI as a dependency and create a gradle module which runs the shell. This would give you a nice, project-scoped REPL.
