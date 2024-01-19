---
title: Managing Multiple Java Versions on Mac
published: true
---

Using `/usr/libexec/java_home -V`, you can view all Java versions installed on your Mac system:

```jsx
➜  bin /usr/libexec/java_home -V
Matching Java Virtual Machines (3):
    20.0.1 (arm64) "Homebrew" - "OpenJDK 20.0.1" /opt/homebrew/Cellar/openjdk/20.0.1/libexec/openjdk.jdk/Contents/Home
    11.0.19 (arm64) "Homebrew" - "OpenJDK 11.0.19" /opt/homebrew/Cellar/openjdk@11/11.0.19/libexec/openjdk.jdk/Contents/Home
    1.8.371.11 (x86_64) "Oracle Corporation" - "Java" /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
/opt/homebrew/Cellar/openjdk/20.0.1/libexec/openjdk.jdk/Contents/Home
```

The command `/usr/libexec/java_home -v version_number` can be used to find the installation location of a specific Java version, allowing us to quickly switch between different Java versions:

```jsx
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`
```

To facilitate quick switching between different Java versions, add the following function in `~/.zprofile`. To switch Java versions, just type `jdk version_number`.

```jsx
jdk () {
    # Check for no arguments or help argument
    if [ "$#" -eq 0 ] || [ "$1" = "-h" ] || [ "$1" = "--help" ]; then
        echo "Usage: jdk [version|-l]"
        echo "  version: Set JAVA_HOME to the JDK version specified."
        echo "  -l     : List installed JDKs."
        return 1
    fi

    # Check if first argument is -l, list JDK versions
    if [ "$1" = "-l" ]; then
        /usr/libexec/java_home -V
    else
        # Set JAVA_HOME to the specified version
        export JAVA_HOME=`/usr/libexec/java_home -v $1`
        # Execute java -version to check the active version
        echo "Switching to Java version: $1"
        java -version
        if [ $? -eq 0 ]; then
            echo "Java version switched successfully."
        else
            echo "Failed to switch Java version."
        fi
    fi
}
```

![Untitled](https://api.2h0ng.wiki:443/noteimages/2024/01/19/17-28-58-c26a0f8f64b36f8aaa6a3a6f29b833c6.png)

# Memo

Java installations downloaded from the Oracle website are located at `/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home`.

Java installations via Brew are located at `/opt/homebrew/Cellar/openjdk/`, or `/Library/Java/JavaVirtualMachines/openjdk`.

# References

[https://juejin.cn/post/6871959224314757134](https://juejin.cn/post/6871959224314757134)

[https://linuxize.com/post/bash-functions/](https://linuxize.com/post/bash-functions/)