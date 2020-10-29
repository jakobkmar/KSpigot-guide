---
description: Every plugins needs a main class.
---

# Main class

## Basic structure

If you come from Spigot, you already know that every plugins needs a main class.

In the case of KSpigot, **do not override `JavaPlugin`**. Override `KSpigot` instead.

```kotlin
class InternalMainClass : KSpigot() {
    override fun load() { }
    override fun startup() { }
    override fun shutdown() { }
}
```

{% hint style="info" %}
Coming from spigot?

* `load()` replaces `onLoad()`
* `startup()` replaces `onEnable()`
* `shutdown()` replaces `onDisable()`
{% endhint %}

### Optional: Create a shortcut to your main class \(which is a plugin instance\)

{% hint style="warning" %}
Due to limitations of the Bukkit class loader, main classes must still be classes. Kotlin's singleton objects are not possible and cause errors.
{% endhint %}

As a workaround for that problem, we can use a little trick.  
Build your main class file as follows:

```kotlin
class InternalMainClass : KSpigot() {
    companion object {
        lateinit var INSTANCE: InternalMainClass; private set
    }

    override fun load() {
        INSTANCE = this
    }
}

val Manager by lazy { InternalMainClass.INSTANCE }
```

Now you are able to use the value **`Manager`** everywhere in your project. It is a `Plugin`, `JavaPlugin`, `KSpigot` instance and holds all of your fields in your main class.

