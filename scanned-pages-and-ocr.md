# Working with Scanned Pages

**Scope:** what a scan really is, what
OCR adds to it, the two extra programs
OCR needs, and the exact refusal the
server gives without them — which the
lab collected so you don't have to.

## A scan is a photograph

A scanner (or a phone camera) does not
produce a document; it produces a
picture *of* a document. The lab's
specimen makes the point cleanly: the
scanned page wished its readers an
open day in letters you can read at a
glance — and a text extractor found
**nothing in it at all** (t08's
pre-check). No words, so no searching,
no copying a sentence out, no screen
reader. Everything in this chapter is
about closing that gap.

**OCR** — optical character
recognition — is the closing: software
looks at the photograph, guesses the
letters, and lays an invisible text
layer over the page in the right
places. The picture is unchanged; the
page becomes searchable and quotable.
It is done by **Tesseract**, the
venerable open-source reading engine,
usually driven by **OCRmyPDF**, a
program whose whole job is adding
that layer to PDFs carefully.

## What the lab can honestly show you

On this guide's bare jar install,
asking for OCR produced the chapter's
most useful artefact (t08):

```
HTTP 403  {"status":403,
  "error":"Forbidden",
  "message":"This endpoint is disabled",
  "path":"/api/v1/misc/ocr-pdf"}
```

And the startup log had already
explained, in plain words (t14):

```
Missing dependency: tesseract -
  Disabling group: tesseract
  (Affected features: Ocr PDF, Auto Rotate PDF)
Missing dependency: ocrmypdf -
  Disabling group: OCRmyPDF
  (Affected features: Ocr PDF)
```

That is the whole diagnosis pattern
for this feature, and it is *well
behaved*: the tool does not fail
mysteriously mid-job; it is absent
from the shelf, announced in the log,
and refuses politely if asked directly
anyway. The lab could not complete an
OCR run — the sandbox's package
manager was unavailable for installing
the helpers — so no OCR *result* is
claimed anywhere in this guide. What
follows is the documented way to equip
it, labelled as such.

## Equipping OCR (documented)

- **Docker route:** the standard and
  fat images already carry Tesseract
  and OCRmyPDF. Nothing to install;
  languages are the only question
  (next section).
- **Jar route:** install **Tesseract**
  and **OCRmyPDF** with your system's
  package manager (the server expects
  them on the PATH, plus Ghostscript,
  which OCRmyPDF needs), then restart
  Stirling PDF and re-read the startup
  log: the "Missing dependency" lines
  should be gone, and the tool joins
  the shelf.
- **Language data** is separate from
  the engine: Tesseract reads the
  languages it has data files for —
  the *tessdata*. English ships by
  default in most installs; add your
  languages' data (Docker: the `LANGS`
  list and the `tessdata` volume in
  chapter 4) and name them in the OCR
  tool's language picker — the API
  takes `languages` the same way
  (t08's request asked for `eng`).

## Using it, once equipped

The tool's shape, from its page in
the shelf and its API entry: give it
the scanned PDF, choose the languages
on the page, and choose the output —
a PDF with the text layer added
(optionally with a plain-text
*sidecar* file beside it), with
options to deskew crooked pages and
straighten sideways ones as it goes.
Then **check the result like the lab
checked everything else**: extract
the text and look for a word you know
is on the page. OCR is a guesser: it
is superb on clean print, shaky on
handwriting, stamps, and carbon
copies, and it will guess wrong with
complete confidence at least once in
any long document. Names and numbers
deserve a human glance.

Two neighbours in the same family:
**auto-rotate**, which reads the
text's direction and rights sideways
pages (needs Tesseract too, and sat
out beside it), and **extract image
scans**, which pulls the photographs
back out of scanned pages.

Check: you should be able to say why
a scanned PDF searches as empty, what
a 403 with "endpoint is disabled"
means, and which two programs — plus
which data files — stand between a
bare install and working OCR.
