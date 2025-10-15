# JVM vs JRE vs JDK — and the roles of java and javac

Great topic — these three acronyms confuse many people at first. I'll explain what each is, how they relate, what they contain, how the Java program lifecycle uses them, and practical examples/commands. I'll also point out modern nuances and when you need each.

## 1) Short summary (one-liners)

**JVM (Java Virtual Machine)** — the runtime engine that executes Java bytecode (and manages memory, GC, JIT, class loading). It's a specification plus many implementations (HotSpot is the most common).

**JRE (Java Runtime Environment)** — a package that contains a JVM + the standard Java class libraries and other runtime components needed to run Java programs.

**JDK (Java Development Kit)** — everything in the JRE plus developer tools (compiler javac, javadoc, jar, jlink, debuggers, etc.). You need the JDK to write/compile/build Java code.

And:

**javac** — Java source code compiler (.java → .class bytecode).

**java** — Java launcher that starts the JVM and runs bytecode (.class or .jar).

## 2) The full picture — lifecycle of a Java program

1. You write Java source: `Hello.java`.

2. You compile with `javac`:
   ```bash
   javac Hello.java → produces Hello.class (Java bytecode).
   ```

3. You run with `java`:
   ```bash
   java Hello → the java launcher starts a JVM, which loads Hello.class, verifies/link/init, JITs or interprets bytecode, runs main(...).
   ```

4. While running the JVM provides GC, thread scheduling, native calls (JNI), classloading, security checks, etc.

So `javac` (compile time) is in the JDK; `java` (runtime) is part of the JRE/JDK (launcher). The actual execution work is performed by the JVM.

## 3) JVM — deeper (what it does)

The JVM is the runtime engine that executes Java bytecode. Major responsibilities:

**Class loading** — loads .class files using a class-loader architecture (bootstrap, platform, application loaders).

**Bytecode verification** — ensures loaded classes follow JVM safety rules.

**Linking** — resolve symbolic references (resolve fields, methods), allocate static memory.

**Initialization** — run static initializers and `<clinit>` blocks.

**Execution** — interpret bytecode or compile hot methods to native code (JIT — Just-In-Time compilation) for speed.

**Garbage collection (GC)** — automatic memory management for heap objects. JVM provides multiple GC algorithms (G1, ParallelGC, ZGC, Shenandoah, etc.).

**Threading & synchronization** — manages Java threads mapped to OS threads (implementation dependent).

**Native interface (JNI)** — allows calling native code (C/C++) and loading native libraries.

**Runtime services** — class metadata, reflection, security manager (legacy), runtime instrumentation.

**Implementation note:** the JVM is a specification. Oracle/OpenJDK's most common implementation is HotSpot (includes Interpreter + C1/C2 JIT compilers, GC implementations). There are other JVM implementations (GraalVM, IBM J9, etc.).

## 4) JRE — deeper (what you get)

JRE = JVM + standard libraries + runtime support:

- **JVM implementation** (HotSpot or other)
- **rt.jar / platform modules** — the standard Java APIs (java.lang, java.util, java.io, java.net, javax.*, etc.). (Modern Java uses modules instead of a single rt.jar).
- **core native libraries** (native code that the JVM uses)
- **launcher** (`java`) and runtime utilities

You install a JRE if you only need to run Java applications (end-users, production systems without compilation). Historically there used to be separate installer bundles named "JRE", but distribution practices vary by vendor and Java version.

## 5) JDK — deeper (developer kit)

JDK = JRE + developer tools. Typical tools included:

- **javac** — Java compiler (source → bytecode)
- **java** — launcher / runtime
- **jar** — create/read JAR files
- **javadoc** — generate API docs
- **jdb** — debugger
- **jlink, jmod** — create runtime images and module operations (since Java 9 modularity)
- **jshell** — REPL for Java (since Java 9)
- **jmap, jstack, jstat** — diagnostic tools

Build tools (maven/gradle) are external but use JDK tools under the hood.

If you are developing Java (writing/compiling), you must have a JDK installed. On a build server, CI/CD pipeline, or local dev machine — JDK is required.

## 6) javac vs java — practical details and examples

### javac — compiler

**Input:** .java source files.

**Output:** .class files (bytecode).

**Example:**

```bash
javac Hello.java               # produces Hello.class
javac -d out src/com/x/*.java  # put .class files under out/ preserving package dirs
```

**Common flags:**

- `-d <dir>` — output directory for .class
- `-classpath / -cp` — classpath for compilation dependencies
- `--module-path` — for module-based compilation
- `-source / -target` — source/target compatibility levels

`javac` checks syntax, types, annotations, and emits class files or compilation errors.

### java — launcher (starts JVM and runs bytecode)

**Input:** .class files or an executable .jar.

**Example:**

```bash
java Hello                        # runs Hello.main
java -cp . com.example.App        # run class in package
java -jar myapp.jar               # run jar (jar manifest must have Main-Class)
```

**Common flags:**

- `-cp / -classpath` — where the JVM finds classes and jars
- `-jar` — run jar file; classpath from manifest used
- **JVM options:** `-Xmx512m` (heap size), `-XX:+UseG1GC` (GC selection), `-agentlib:jdwp=...` (debug)
- `--module-path` and `--module` options for modular applications

**Important:** `javac` is not required to run Java programs (you run compiled bytecode). `java` cannot run .java files directly (except via tools that compile+run behind the scenes like jshell/some build tools).

**Newer convenience:** since JDK 11 you can run a one-file program directly:

```bash
java Hello.java
```

This compiles and runs in one step (a convenience feature in modern JDKs), but under the hood a compilation step happens before running. For multi-file projects or real builds, you use `javac` + tooling.
