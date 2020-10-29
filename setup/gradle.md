---
description: Implementing KSpigot with your gradle build script.
---

# Gradle

## Add the required repositories

{% tabs %}
{% tab title="build.gradle.kts" %}
```kotlin
repositories {
    jcenter()
    maven("https://jitpack.io")
}
```
{% endtab %}

{% tab title="build.gradle" %}
```groovy
repositories {
    jcenter()
    maven { url 'https://jitpack.io' }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`jcenter()` is for KSpigot itself, the other repositories are required for KSpigots' dependencies.
{% endhint %}

## Add the dependency

[ ![kspigot version](https://img.shields.io/bintray/v/bluefireoly/KSpigot/KSpigot?label=kspigot%20latest%20version&style=flat-square) ]()

{% tabs %}
{% tab title="build.gradle.kts" %}
```kotlin
dependencies {
    implementation("net.axay", "KSpigot", "INSERT_VERSION")
}
```
{% endtab %}

{% tab title="build.gradle" %}
```kotlin
dependencies {
    implementation 'net.axay:KSpigot:INSERT_VERSION'
}
```
{% endtab %}
{% endtabs %}

## Shading KSpigot into your jar file

KSpigot must be available at runtime. Therefore, the best way of providing it is to shade it into your final jar file.

You will need the shadow plugin by John Engelman.

The latest version of the plugin:
[ ![shadow-plugin version](https://api.bintray.com/packages/johnrengelman/gradle-plugins/gradle-shadow-plugin/images/download.png) ]()

{% tabs %}
{% tab title="build.gradle.kts" %}
```kotlin
plugins {
    id("com.github.johnrengelman.shadow") version "INSERT_VERSION"
}
```
{% endtab %}

{% tab title="build.gradle" %}
```groovy
plugins {
    id 'com.github.johnrengelman.shadow' version 'INSERT_VERSION'
}
```
{% endtab %}
{% endtabs %}

Now KSpigot should be relocated. To do that, you have two options:

1. relocate only KSpigot
2. relocate all of your dependencies

{% tabs %}
{% tab title="Option 1 \(build.gradle.kts\)" %}
```kotlin
tasks {
    shadowJar {
        relocate("net.axay.kspigot", "${project.group}.shadow.net.axay.kspigot")
    }
}
```

{% hint style="success" %}
KSpigot does not have any known issues with relocating.
{% endhint %}
{% endtab %}

{% tab title="Option 2 \(build.gradle.kts\)  " %}
```kotlin
import com.github.jengelman.gradle.plugins.shadow.tasks.ConfigureShadowRelocation

val relocateShadowJar by tasks.creating(ConfigureShadowRelocation::class) {
    target = tasks.shadowJar.get()
    prefix = "${project.group}.shadow"
}

tasks.shadowJar.get().dependsOn(relocateShadowJar)
```

{% hint style="danger" %}
This option may cause issues with some dependencies. Especially if they use reflection.
{% endhint %}
{% endtab %}

{% tab title="Option 1 \(build.gradle\)" %}
```groovy
shadowJar {
   relocate 'net.axay.kspigot', 'YOUR_PACKAGE.shadow.net.axay.kspigot'
}
```

{% hint style="success" %}
KSpigot does not have any known issues with relocating.
{% endhint %}
{% endtab %}

{% tab title="Option 2 \(build.gradle\)" %}
```groovy
import com.github.jengelman.gradle.plugins.shadow.tasks.ConfigureShadowRelocation

task relocateShadowJar(type: ConfigureShadowRelocation) {
    target = tasks.shadowJar
    prefix = "YOUR_PACKAGE.shadow"
}

tasks.shadowJar.dependsOn tasks.relocateShadowJar
```

{% hint style="danger" %}
This option may cause issues with some dependencies. Especially if they use reflection.
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
If you do not relocate KSpigot, it could come to conflicts with other plugins using KSpigot aswell.
{% endhint %}

## Build your project

Now execute the gradle task `shadowJar` to build your jar file.



