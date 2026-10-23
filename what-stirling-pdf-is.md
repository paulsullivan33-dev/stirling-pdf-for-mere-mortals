# What Stirling PDF Is

**Scope:** the mental model — a PDF
workshop that runs on your own machine
and serves a web page — plus what it is
not, and the lab this guide's claims
rest on.

## The one-paragraph version

**Stirling PDF** is open-source software
that does jobs on PDF files: merging
several into one, splitting one into
many, rotating pages, converting to and
from other formats, shrinking, signing,
watermarking, reading scanned pages
aloud to itself so they become
searchable. You run it on a computer in
your own house (or a small server you
control), open its page in a browser,
drop in a file, and get the result back.
The work happens on that machine, in
that room. There is no account with any
company, and no upload to anyone's
cloud, because the program *is* the
destination.

That last point is the whole idea, and
it is why the project exists. PDF tools
are the classic errand people hand to
random websites — and PDFs are so often
exactly the documents you least want to
hand over: contracts, bank letters,
medical forms, the scan of a passport.
Stirling PDF keeps the errand at home.

## The shape of the thing

Three pieces of the experience are worth
naming before any machinery:

- **It is a workshop, not a filing
  cabinet.** Stirling PDF does not keep
  your documents. You bring a file, it
  does the job, you take the result
  away. Its only lasting possessions
  are its settings — chapter 12 shows
  how small the "keep this" pile is.
- **The web page and the scripts share
  one engine.** Every button on the
  page calls the same **API** a script
  can call. Learn a job in the browser
  and you already know its shape at the
  command line; chapter 9 is chapter 6
  with the clicking removed.
- **Some tools borrow other programs.**
  The core jobs — merge, split, rotate —
  are built in. Others lean on famous
  neighbours: LibreOffice converts
  office files, Tesseract reads scanned
  pages. A tool whose helper is missing
  quietly sits out, and the server's log
  says which and why. Chapter 2 maps
  the borrowing; it explains almost
  every surprise later.

## What it is not

Four confusions worth clearing away
early:

- It is **not an upload website that
  happens to be nearby**. The popular
  PDF sites process your file on *their*
  computers. Stirling PDF's page looks
  similar, but the address in the
  browser is your own machine — check
  it: `localhost`, or your server's
  name, never a company's.
- It is **not a document author**.
  There is light editing — rearranging
  pages, editing metadata, a modest
  text editor — but for writing and
  laying out documents you still start
  in a word processor or typesetter,
  and convert the result. Chapter 7
  covers that border crossing.
- It is **not a vault**. It does not
  encrypt your archive, manage your
  files, or back anything up. It is a
  bench you bring work to. Paperless
  and friends, in the neighbours
  chapter, are the shelves.
- It is **not finished software in the
  old sense**. The project releases
  often — this guide's version, 3.2.1,
  was published the day before the lab
  ran — and features move between
  releases. Pinning the version you
  run, and reading the release notes
  before an upgrade, are standing
  habits; chapter 12 builds both in.

## Versions and the lab

This guide was built against **Stirling
PDF v3.2.1**, the current release on the
day of the build, from the project's own
published files: the standard jar and
the login jar, fetched from the
project's release page and run under a
current Java. On top of it the lab used
invented specimens only — a garden-club
newsletter, a set of minutes, a picnic
flyer, all generated on the build
machine — and exercised the project's
published **API** (148 addresses in its
own OpenAPI document) to check what the
server claimed against what it did:
page counts, rotations read out of the
files' own numbers, text found or
honestly absent.

Two sandbox limits, stated once: the
build machine has no Docker, so the
container route is taught from the
project's documentation and labelled as
such wherever it appears; and the OCR
helpers could not be installed there,
so chapter 8 shows the refusal the
server gives without them — which is
itself the commonest first-run
experience of that feature. `criteria.md`
keeps the full ledger of what was run
and what was only read.

Check: you should now be able to say
where a file goes when Stirling PDF
merges it (nowhere — it never leaves
the machine), and why two neighbours'
tools might be missing from its shelf
on your install. If that is clear,
chapter 2 opens the box.
