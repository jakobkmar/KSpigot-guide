---
description: Implementing KSpigot with your maven build script.
---

# Maven

Open your `pom.xml` file.

## Add the required repositories

```markup
<repositories>
    <repository>
        <id>jcenter</id>
        <name>jcenter</name>
        <url>https://jcenter.bintray.com</url>
    </repository>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```

{% hint style="info" %}
`jcenter`is for KSpigot itself, the other repositories are required for KSpigot's dependencies.
{% endhint %}

## Add the dependency

The latest version of KSpigot: [ ![kspigot version](https://img.shields.io/bintray/v/bluefireoly/KSpigot/KSpigot?label=version&style=flat-square) ](gradle.md)

```markup
<dependencies>
    <dependency>
        <groupId>net.axay</groupId>
        <artifactId>KSpigot</artifactId>
        <version>INSERT_VERSION</version>
    </dependency>
</dependencies>
```

## Shading KSpigot into your jar file <a id="shading-kspigot-into-your-jar-file"></a>

KSpigot must be available at runtime. Therefore, the best way of providing it is to shade it into your final jar file.

You will need the [Apache Maven Shade Plugin](https://maven.apache.org/plugins/maven-shade-plugin/index.html).

Now KSpigot needs [to be relocated](https://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html).

{% hint style="warning" %}
If you do not relocate KSpigot, it could come to conflicts with other plugins using KSpigot aswell.
{% endhint %}

## Build your project

If you now build your project \(for example with the `package` task\) to create the jar file.

