# Installing the Jar

**Scope:** the route this guide verified
end to end — download the release jar,
run it under a current Java, and prove
it is alive. Docker households can skip
to chapter 4; everyone else starts
here.

## What you need

- A machine that can stay on while you
  work: a desktop, a home server, a
  small cloud box. Windows, macOS or
  Linux — the jar does not care.
- **Java 25, or newer.** Not "a Java."
  Read the next section before skipping
  it; it is the one sharp edge on this
  whole install.
- About a gigabyte of free disk for the
  jar and its folders, and room to
  breathe after that.

## The Java 25 lesson

Stirling PDF 3.x is compiled for
**Java 25**. Run v3.2.1 under an older
Java — the lab tried the perfectly good
Java 21 installed on the build machine —
and it stops instantly with:

```
java.lang.UnsupportedClassVersionError:
stirling/software/SPDF/SPDFApplication
has been compiled by a more recent
version of the Java Runtime (class file
version 69.0), this version of the Java
Runtime only recognizes class file
versions up to 65.0
```

Translated: *this program speaks Java
25; your Java is older.* Class file 69
is Java 25's number, 65 is Java 21's.
The fix is not a setting — it is a
newer Java. Eclipse Temurin, Amazon
Corretto, Oracle's JDK and your
distribution's `openjdk-25` packages
all serve. Then check what you actually
have before running anything:

```
java -version
```

Check: the first line names version
**25** (or later). If it names anything
else, the jar will refuse; fix the
Java, not the jar. If several Javas
share your machine, call the right one
by its full path — the lab did exactly
that, keeping the machine's Java 21
untouched for its other work.

## Download and first run

1. From the project's GitHub releases
   page, get **`Stirling-PDF.jar`** for
   the current release (v3.2.1 on the
   lab's build day; 298 MB — these jars
   are not small).
2. Put it in a folder of its own. The
   program writes its `configs`,
   `customFiles`, `logs` and `pipeline`
   folders beside the jar, and you want
   those somewhere findable:

   ```
   mkdir stirling-pdf && cd stirling-pdf
   # move the downloaded jar here
   ```

3. Run it:

   ```
   java -jar Stirling-PDF.jar
   ```

4. Wait for the line that says it
   started — on the lab's machine the
   plain build took about twenty
   seconds of program time; the login
   build, on its first run, took nearer
   eighty while it built its database:

   ```
   Started SPDFApplication in 20.423 seconds
   ```

## Prove it is alive

Do not trust the log alone. Ask the
server (transcript t00):

```
curl http://localhost:8080/api/v1/info/status
```

Check: it answers, in full,

```
{"version":"3.2.1","status":"UP"}
```

Then open `http://localhost:8080` in a
browser. The page's title — *Stirling
PDF* — and a shelf of tools should
appear. That pair, status answer plus
page, is this guide's standing health
check; chapter 12 uses it in the backup
drill, and the Docker health check in
chapter 4 asks exactly the same
question.

## A different port

If 8080 is taken on your machine — it
often is — move the server before
starting it, with an environment
variable (transcript t15):

```
SERVER_PORT=8081 java -jar Stirling-PDF.jar
```

Check: the status address answers on
8081, and 8080 stays silent. Nearly
every setting in chapter 11 can be
overridden this way; the port is just
the first one people need.

## Running it for keeps

A terminal you must leave open is a
try-out, not an install. The honest
options:

- **Linux:** a small systemd unit that
  runs `java -jar ...` from the jar's
  folder, restarted on failure, started
  at boot. The series' systemd guide
  teaches the shape; the only specifics
  here are the working directory (the
  folders land beside the jar) and the
  Java 25 path.
- **Windows:** the desktop installer
  from the same release page, or a
  shortcut to the jar in the Startup
  folder for a single-user machine.
- **macOS:** the desktop build, or a
  launchd entry for the jar.

Whichever you choose, the test is the
same: reboot, then ask the status
address. If it answers, the install is
real.

Check: you should now have a server
that answers `{"version":"3.2.1",
"status":"UP"}` (your version may be
newer), its four folders beside the
jar, and a browser tab showing the
tool shelf. Chapter 5 walks that
shelf; chapter 4 first, for the Docker
route.
