### Choice of JDKs

There are 2 Java development kits available with Scoop.

OpenJDK is the default, and can be installed with `scoop install openjdk`.

Oracle's JDK is also available, but not being open source, it's in the `java` [bucket](https://github.com/lukesampson/scoop/wiki/Buckets). So to install it, you'll need to first run `scoop bucket add java`, and then you can install the Oracle JDK with `scoop install oraclejdk`.

### Switching JDKs

Java apps in the main (default) bucket will typically depend on `openjdk`. The reason Scoop defaults to `openjdk` rather than leaving the decision of JDKs up to the user is that we want to make it "just work" when you install a Java-based app like Ant.

You can however switch to Oracle JDK with `scoop reset oraclejdk`, assuming you've already installed it with `scoop install oraclejdk`.

You can switch back and forth between the 2 JDKs at any time.

### Java Bucket

There is a `java` bucket maintained by **[@se35710](https://github.com/se35710/)** for more info go to the [scoop-java project](https://github.com/se35710/scoop-java).