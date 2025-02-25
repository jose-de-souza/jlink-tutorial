# jlink Hello World

## 1 Create the directory structure

```bash
mkdir -p hello-example/sample
```

## 2 Create the file hello-example/sample/HelloWorld.java

```java
package sample;

import java.util.logging.Logger;

public class HelloWorld {
    private static final Logger LOG = Logger.getLogger(HelloWorld.class.getName());
    public static void main(String[] args) {
        LOG.info("Hello World!");
    }
}
```

## 3 create the module info file hello-example/module-info.java

```java
module sample
{
    requires java.logging;
}
```

## 4 Compiling

```bash
./jdk-21/bin/javac -d example $(find hello-example -name \*.java)
```

## 5 Running

### 5.1 Without a custom JRE
```bash
./jdk-21/bin/java -cp example sample.HelloWorld
```

### 5.2 Building a custom JRE

### Checking the original JVM size
```bash
du -sh jdk-21/
```

### Creating an application module
```bash
mkdir sample-module
```bash

```bash
./jdk-21/bin/jmod create --class-path example/ --main-class sample.HelloWorld --module-version 1.0.0 -p example sample-module/hello.jmod
```

### Creating a custom JRE with the required modules and a custom application launcher for the application
```bash
./jdk-21/bin/jlink --launcher hello=sample/sample.HelloWorld --module-path sample-module --add-modules sample --output custom-runtime
```

### Checking the size of the newly created custom JRE
```bash
du -sh custom-runtime
```

### Checking the modules of the custom JRE
```bash
./custom-runtime/bin/java --list-modules
```

## Launch the application using the hello launcher
```bash
./custom-runtime/bin/hello
```



