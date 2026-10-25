# Stirling PDF for Mere Mortals

Every computer collects PDFs that need
something done to them: two files that
should be one, one file that should be
three, a scan lying sideways, a form
locked in the wrong format. The usual
answer is a website — upload your
document to a stranger's server and trust
the promises on their privacy page.
**Stirling PDF** is the other answer: an
open-source PDF workshop you run
yourself. It serves a friendly web page
on your own machine, offers dozens of
tools — merge, split, rotate, convert,
compress, watermark, OCR — and processes
every file locally. Nothing is uploaded
anywhere, because there is nowhere else
involved.

The lab behind this guide ran the real
thing: **Stirling PDF v3.2.1**, the
project's current release on build day,
downloaded as the project's own jar files
and run in the open. Into it went
invented specimens only — a three-page
garden-club newsletter, a two-page set of
minutes, a picnic flyer — generated on
the build machine; no real document was
used anywhere. The server was exercised
through the same **API** its web page
talks to: two files merged into a
five-page whole whose pages were then
counted and read back, a split that had
to add up, a rotation checked in the
file's own numbers, conversions to text,
to an image, and from a word-processor
file. What the lab could not do, it did
not fake: there is no Docker in the
build sandbox, so the container chapter
follows the project's documentation and
says so, and OCR — which needs extra
programs this machine lacks — is shown
being refused, with the server's own log
explaining why, rather than described as
if it had worked.

## What this guide covers

- [**What Stirling PDF is**](what-stirling-pdf-is.md) —
  the mental model: a workshop, not a
  website; files that never leave; what
  it is not (an authoring tool, a cloud)
- [**How it is put together**](how-it-is-put-together.md) —
  one Java program wearing three coats:
  the web page, the API, and the folders
  it keeps; the helper programs some
  tools lean on
- [**Installing the jar**](installing-the-jar.md) —
  the Java 25 requirement (learned the
  hard way), downloading the release,
  first folders, a health check, a
  custom port
- [**Installing with Docker**](installing-with-docker.md) —
  the three images, the compose file,
  the volumes that matter (documented —
  the lab ran the jar)
- [**First run and the home page**](first-run.md) —
  what the page shows, how tools are
  grouped, and the status address as a
  heartbeat
- [**Everyday jobs in the browser**](everyday-jobs.md) —
  merge, split, rotate, compress and
  friends, each tied to what the lab
  proved the machinery underneath does
- [**Converting files**](converting-files.md) —
  PDF to text, image, and office formats;
  office files and images to PDF; which
  conversions are built in and which
  borrow a neighbour program
- [**Scanned pages and OCR**](scanned-pages-and-ocr.md) —
  why a scan is a photograph, what OCR
  adds, the two extra programs it needs,
  and the polite refusal when they're
  missing
- [**Scripting with the API**](scripting-with-the-api.md) —
  the same tools with no clicking: the
  multipart shape, the lab's own curl
  commands, the error a bad file earns
- [**Keeping it private**](keeping-it-private.md) —
  the honest threat model: the plain
  build has no login at all, the login
  build gates every job, and the little
  status page that stays public either
  way
- [**Settings and customisation**](settings-and-customisation.md) —
  the generated settings file, the
  environment-variable override trick,
  names and limits, and the analytics
  switches worth knowing about
- [**Backing up and upgrading**](backing-up-and-upgrading.md) —
  the one folder that holds everything
  worth keeping, the key file inside it,
  and why your documents need no backup
  here at all
- [**Recipes**](recipes.md) — the
  standing home install, the club
  newsletter run, a folder of scans
  converted by script, the locked-down
  family server
- [**The neighbours**](the-neighbours.md) —
  Acrobat, the upload websites, the
  command-line elders: what each trades
- [**Limits**](limits.md) — a young Java
  appetite, tools that vanish without
  their helpers, OCR's fine print, and
  the paid rooms off the free corridor
- [**Troubleshooting**](troubleshooting.md) —
  the class-version error, the disabled
  endpoint, the file that isn't a PDF,
  the locked-out admin
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Running Stirling PDF as a public
service for strangers — internet
exposure, TLS certificates, and
reverse-proxy hardening are warned
about, not taught; the series' Caddy
and Tailscale guides live there. The
paid Pro and Enterprise features (SSO
and SAML sign-in, team management)
appear only as signposts in the
settings file. And deep PDF surgery —
repairing corrupt page trees by hand,
prepress work, PDF/A archiving
projects — belongs to the command-line
elders named in the neighbours
chapter.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which parts borrow which
programs turns later confusion into a
named problem. Chapter 3 gets a server
running on any machine with a current
Java; chapter 4 is the alternative for
Docker households. Do one real job in
chapter 6 with the server's log open
in another terminal: watching a tool
work once, end to end, teaches the
shape of all of them. Read chapter 10
before you let anyone else on your
network use it. The lab's transcripts
live in `stirling-pdf-lab/transcripts/`;
its notes are `examples/lab-notes.txt`,
and `criteria.md` keeps the ledger of
what was run and what was only read.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Pandoc for Mere Mortals](https://paulsullivan33-dev.github.io/pandoc-for-mere-mortals/)
- [LibreOffice for Mere Mortals](https://paulsullivan33-dev.github.io/libreoffice-for-mere-mortals/)
- [Docker Compose for Mere Mortals](https://paulsullivan33-dev.github.io/docker-compose-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/stirling-pdf-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/stirling-pdf-for-mere-mortals/blob/main/LICENSE-CODE).
