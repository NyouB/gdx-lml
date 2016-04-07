# LibGDX utilities
This repository contains most of my libraries aimed at improving LibGDX framework. While they used to be kept in multiple separate repositories, this setup is much easier to maintain for the contributor(s) and, well, makes it harder to overlook some of my less popular libraries.

### Note
This used to be `gdx-lml` library repository before I finally forced myself to merge all my LibGDX extensions into one huge Gradle project. Switching to the previous setup is highly unlikely, so if you watched or starred some of my other repositories, it's time to do the same with this one. Sorry.

If you came here looking for `gdx-lml` documentation, [check this out](https://github.com/czyzby/gdx-lml/tree/master/lml). `gdx-lml-tests` project, which shows usage of all LML tags, was moved [here](https://github.com/czyzby/gdx-lml/tree/master/examples/gdx-lml-tests).

## Projects

### gdx-kiwi
[Kiwi](https://github.com/czyzby/gdx-lml/tree/master/kiwi) is a Guava-inspired set of utilities for pretty much any LibGDX-based application. It makes it easier to use LibGDX collections, assets and its API in general. While there are no `gdx-kiwi`-specific test projects, most of examples use Kiwi utilities to handle heavy assets and collections.

### gdx-lml
[LML](https://github.com/czyzby/gdx-lml/tree/master/lml) (*LibGDX Markup Language*) allows to parse HTML-like templates with FreeMarker-inspired macros into Scene2D actors. Since making your UI in Java can become unreadable and tedious thanks to this language's verbosity, LML can be a useful alternative. Especially since LML templates can be modified or reloaded without having to recompile the whole application. Comes with extra support for managing preferences, assets and internationalization.

#### gdx-lml-vis
[VisUI](https://github.com/kotcrab/VisEditor/wiki/VisUI) is a superb library, extending Scene2D with additional widgets and a modern skin. [LML Vis](https://github.com/czyzby/gdx-lml/tree/master/lml-vis) allows to parse LML templates into VisUI widgets, instead of standard Scene2D ones and extends the syntax with ways to construct the new actors. Even if you want to have a custom skin in your application, consider using this library for the improved widgets.

### gdx-autumn
[Autumn](https://github.com/czyzby/gdx-lml/tree/master/autumn) is a dependency injection mechanism with component scanning. Using a set of annotations and class scanners, it allows you to build your application without singletons, global variables or even direct calls to constructors. Base for `gdx-autumn-mvc` framework.

#### gdx-autumn-android
[Autumn Android](https://github.com/czyzby/gdx-lml/tree/master/autumn/natives/android) provides class scanner for Android applications.

#### gdx-autumn-fcs
[Autumn FCS](https://github.com/czyzby/gdx-lml/tree/master/autumn/natives/fcs) provides class scanner for desktop applications using lightweight [`fast-classpath-scanner`](https://github.com/lukehutch/fast-classpath-scanner).

#### gdx-autumn-gwt
[Autumn GWT](https://github.com/czyzby/gdx-lml/tree/master/autumn/natives/gwt) provides class scanner for GWT applications.

### gdx-autumn-mvc
[Autumn MVC](https://github.com/czyzby/gdx-lml/tree/master/mvc) is a model-view-controller framework on top of LibGDX. It uses **Autumn** to manage components and **LML** as view templates. Makes it easier to maintain assets, internationalization, preferences, music, screen transitions, and so on. While other libraries are rather general-purpose, this one forces its structure upon your application - but should be still worth it, considering the amount of things it handles for you.

### gdx-websocket
[LibGDX web sockets library](https://github.com/czyzby/gdx-lml/tree/master/websocket) aims to extend the default `Net` implementations with cross-platform client-side web sockets. Rather than being a huge framework with server-side libraries, this set of libraries offers a simple and somewhat low level, yet pretty powerful client networking API.

#### gdx-websocket-common
[Common web sockets library](https://github.com/czyzby/gdx-lml/tree/master/websocket/natives/common) contains web socket natives for desktop and Android applications using high quality [nv-websocket-client](https://github.com/TakahikoKawasaki/nv-websocket-client) library.

#### gdx-websocket-gwt
[GWT web sockets library](https://github.com/czyzby/gdx-lml/tree/master/websocket/natives/gwt) contains web socket natives for GWT applications.

#### gdx-websocket-serialization
[Serialization library for LibGDX web sockets](https://github.com/czyzby/gdx-lml/tree/master/websocket/natives/serialization) contains serialization mechanism that works on every LibGDX platform. While default object serialization (using LibGDX `Json` API, based on reflection) is fine for most project, some performance-critical applications might require a different solution. Since most popular and tested serialization libraries are rarely GWT-compatible, `gdx-websocket` comes with its own alternative - this library. Warning: JSON-based communication is much easier to use, switch to this serialization only when necessary.

### Examples
See [examples section](https://github.com/czyzby/gdx-lml/tree/master/examples) to check out some simple applications using presented libraries.

## Dependencies
All libraries follow the same schema:
```
        compile "com.github.czyzby:lib-name:$libVersion.$gdxVersion"
```
`lib-name` is the name of the library (one of the ones listed above). `libVersion` follows `MAJOR.MINOR` schema and is the actual version of the library. `gdxVersion` is the version of the LibGDX library used to build the archive. For example, this is a valid LML dependency (although it might be out of date by now!): `'com.github.czyzby:gdx-lml:1.5.1.9.2'`. To find out the current version ID and GWT module, check out the specific library's `README` file. Looking through [Maven Central](http://search.maven.org/#search|ga|1|g%3A%22com.github.czyzby%22) might also help.

## Working with the sources
Clone this repository. The whole setup is Gradle-based, with very similar structure to default LibGDX projects generated with `gdx-setup`. Note that Gradle wrapper is not included in the root project, so you should have Gradle installed locally.

To deploy the libs, the project requires some additional "secret" properties, used for archives signing and logging to Maven Central. While you most likely will not need these functionalities, Gradle still forces you to provide these properties. A default unfilled `gradle.properties` file is available in the root folder, so Gradle will not complain about missing properties. Eventually you might want to fill these in your Gradle home folder:
```
        signing.keyId= 
        signing.password= 
        signing.secretKeyRingFile= 

        ossrhUsername= 
        ossrhPassword= 
```
Note that deploying to Maven Local does *not* require the signing task, so if you just keep signing properties commented out - you should be fine. Try running `gradle installAll` to check if deploying to Maven Local works.

Before pulling any requests, make sure your code is formatted with `eclipse-formatter.xml` (or its equivalent for other IDE). Note that this is *not* the official LibGDX code formatter, as I'm not really a huge fan of its setup.

### Useful Gradle tasks
- `gradle eclipse` - generates Eclipse project structure.
- `gradle idea` - generates IntelliJ project structure.
- `gradle build install` - builds the libraries' archives and pushes them to Maven Local.
- `gradle installAll` - same as the previous one, but the tasks are always invoked in the correct order. Use when changing libraries' versions to avoid missing artifacts errors.
- `gradle uploadArchives` - pushes the archives to Maven Central. Requires proper `gradle.properties` with signing and logging data.
- `gradle clean` - removes built archives.

Additionally, in `examples` directory you can find a utility Gradle project. This is *not* the root project of example applications: they are all autonomous and can be copied outside the repository (and should still work!). Still, it contains some utility tasks that modify or test example projects en masse:

- `gradle updateVersion` - copies `gradle.properties` from `examples` to all projects directories.
- `gradle eclipseAll` - generates Eclipse project meta-data for all examples.
- `gradle runAll` - starts each desktop application, one by one. Useful for quick testing. Some applications (web socket tests) might fail to run if their corresponding server application is not turned on - this is expected.
- `gradle cleanAll` - cleans build directories of example projects.

To run a task on a specific library, proceed task name with its project ID. For example, `gradle kiwi:build` will build archives of Kiwi library.
