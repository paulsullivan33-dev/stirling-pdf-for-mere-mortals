# Troubleshooting

**Scope:** symptom → cause → fix, for
the failures this guide actually met
or can point to in the program's own
words. The lab's errors come first;
they are the ones with transcripts.

## It stops instantly, complaining about a class file version

```
java.lang.UnsupportedClassVersionError:
... class file version 69.0 ... only
recognizes class file versions up to 65.0
```

Your Java is older than the release
needs: 69 means the program was built
for Java 25, 65 is Java 21's ceiling.
Install a current Java (chapter 3),
check `java -version` says 25 or
later, and if two Javas share the
machine, call the right one by full
path. Nothing in the jar is broken;
nothing in the settings will fix it.

## A tool answers 403, "This endpoint is disabled"

```
{"status":403,"error":"Forbidden",
 "message":"This endpoint is disabled",
 "path":"/api/v1/misc/ocr-pdf"}
```

The tool's helper program is not on
the machine. Read the server's
**startup log** — it names every
missing dependency and the features
it took with it:

```
Missing dependency: tesseract - ...
Missing dependency: ocrmypdf - ...
```

Fix: install the named programs (or
take the Docker route, where they are
bundled), restart, and confirm the
lines are gone. Chapter 2 explains
the pattern; chapter 8 is the OCR
case in full. Note the subtlety from
t14/t09: a group listed as affected
may still work through a built-in
implementation (compress did). After
the restart, *try the tool* — the log
predicts, the attempt decides.

## A job returns 500 and mentions a failed merge/convert

```
{"error":"Job failed: java.io.IOException: JPDFium merge failed"}
```

The input was not what the tool
eats — in the lab's deliberate test
(t10), a plain text file wearing a
`.pdf` errand. Check the file itself:
its real type (not its name), whether
it opens in another reader, whether
it is a password-protected PDF whose
password the job doesn't have. A file
that fails everywhere is a damaged
file; the **repair** tool (chapter 6)
is the one legitimate next try, where
it is equipped.

## The login build says 401, "Authentication required"

Your request carried no credentials
— a script written against the plain
build meeting the login build for the
first time (t11). Browser users get
the sign-in page; scripts must
authenticate the way the project's
API documentation describes, or the
job stays refused. Related trap: five
wrong passwords lock the account for
two hours by default
(`security.loginAttemptCount`,
`loginResetTimeMinutes` in chapter
11) — if the admin is locked out,
wait it out or adjust the settings
file *and restart*, with the server
stopped while you edit.

## The page loads but a tool you expect is missing

Same root as the 403, seen from the
other side: the shelf shows only the
tools whose helpers exist. Compare
the startup log's disabled list (t14
is the lab's) with your expectation
before suspecting the version, the
browser, or yourself.

## The status address answers but the version surprises you

Two servers are sharing the machine —
an old one still holding the port,
perhaps — or you are looking at the
other build. `{"version": ...}` tells
you which release answered; the
process list tells you how many
Javas are running. One server, one
port, one version is the healthy
shape; chapter 3's `SERVER_PORT`
separates rivals cleanly.

## Settings changes vanish or the file "edits itself"

By design: `settings.yml` is a
generated template. Commented-out
entries are removed at startup, and
values can be overridden by
environment variables you set months
ago and forgot (chapter 11's naming
pattern — hunt for `STIRLING`-less
suspects like `SYSTEM_*` and
`SECURITY_*` in the service's
environment). Edit values, don't
delete lines; keep personal overrides
in `custom_settings.yml`.

## It worked yesterday; an upgrade changed the furniture

A weekly-ish release cadence moves
menus and defaults. Roll back by
putting the previous jar back beside
the same folders (chapter 12 keeps
the old jar for exactly this), verify
with the status address and one
checked job, then read the release
notes at leisure and decide whether
the new furniture is worth the move.
