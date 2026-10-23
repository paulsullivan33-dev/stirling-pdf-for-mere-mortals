# How It Is Put Together

**Scope:** the moving parts — one Java
program, its web page, its API, its
folders, and the helper programs some
tools depend on.

## One program, three faces

Stirling PDF is a single self-contained
program written in **Java** — the
release file is a *fat jar*, one
download that carries the application
and everything it needs to run. Start it
and three faces appear:

| Face | What it is | Who uses it |
|---|---|---|
| The web page | The tool shelf in a browser tab | People |
| The API | The same tools as web addresses (`/api/v1/...`) | Scripts, other programs |
| The folders | Settings, logs, custom files on the server's disk | The program itself |

There is no separate database server,
no installer wizard, no second service
to babysit. The program embeds its own
web server (it listens on port **8080**
by default) and serves the page and the
API from the same process. When you
stop the program, everything stops;
when you start it, everything is back.

Because the page is a *client* of the
API — every button press is an API call
wearing a nice coat — anything the page
can do, a script can do. Chapter 9
leans on this throughout.

## Where your documents go

A job runs like this: the browser (or
script) sends the file to the server in
one request; the server does the work
in its own memory and temp space; the
result comes straight back in the
response; the server keeps no copy.
Run the lab's merge and then look
through the server's folders: settings
and logs, and no merged PDF anywhere.
That is the design, not an accident —
and it is why chapter 12's backup list
is so short.

## The folders it keeps

Start the program in an empty folder
and it writes four things next to
itself:

| Folder | Holds | Chapter |
|---|---|---|
| `configs/` | `settings.yml`, and in login builds the account database and an encryption key | 11, 12 |
| `customFiles/` | Your own touches — logos, disclaimer texts | 11 |
| `logs/` | The server's diary, including which tools sat out and why | 16 |
| `pipeline/` | Watched-folder automation recipes | 13 |

Everything else about the program is
inside the jar. Move the jar and the
`configs` folder together and the
server has moved house intact.

## The builds on the shelf

The same program is packaged several
ways. The release page for v3.2.1
offers:

- **`Stirling-PDF.jar`** — the standard
  jar. No login machinery; anyone who
  can open the page can use every tool.
  This guide's main route.
- **`Stirling-PDF-with-login.jar`** —
  the jar with user accounts and
  sign-in switched on. Chapter 10 runs
  this one.
- **`Stirling-PDF-server.jar`** — a
  slimmer server-only variant.
- **Desktop installers and an AppImage**
  — for Windows, macOS and Linux, for
  people who want an icon, not a server.
- **Docker images** — the standard image
  (login, OCR and conversion helpers
  bundled), a `-fat` image (adds a
  bundled AI engine, off until enabled),
  and an `-ultra-lite` image (core tools
  only, no login, no OCR). Chapter 4.

The core of the project is open source
under the MIT licence; a few rooms off
the corridor — SSO sign-in, team
features — are paid Pro and Enterprise
extras, and the settings file labels
them as such where they appear.

## The helpers

Some tools are not Stirling's own work
but a neighbour program driven politely.
At startup the server looks for each
helper on the machine's PATH and
**disables the tools that need a missing
one**, writing plain lines to its log.
The lab's bare machine — current Java,
plus a LibreOffice that was already
there — produced this report
(transcript t14):

- Missing `tesseract` and `ocrmypdf` →
  **OCR** and auto-rotate disabled
- Missing `gs` (Ghostscript), `qpdf`,
  ImageMagick → parts of **repair** and
  **compress** disabled on paper — though
  compress in fact still worked through
  a built-in implementation (t09). Treat
  the log as "these implementations are
  off," not always "this tool is gone."
- Missing WeasyPrint → HTML, URL and
  email **to PDF** disabled
- Missing Calibre → PDF **to e-book**
  disabled
- Present: LibreOffice → office files
  **to PDF** worked first try (t07)

Sixteen endpoints sat out in total.
The full list is in transcript t14; the
pattern matters more than the list: *the
shelf you see is the shelf your machine
equips*. The Docker images sidestep the
whole question by bundling the helpers.

## The account database (login builds)

The login build keeps its users in a
small database file inside `configs/`
(an H2 database, `stirling-pdf-DB-*.mv.db`),
plus a `credential-encryption.key` file
created on first start. Lose the key
and any secrets the server stored —
integration credentials, for example —
cannot be recovered. Chapter 12 puts
both files on the backup list, in
bold.

Check: you should be able to name the
three faces, the four folders, and at
least two helpers with the tools that
depend on them. That map is enough to
install it — chapter 3 does the jar.
