# Everyday Jobs in the Browser

**Scope:** the handful of jobs that
cover most real errands — merge, split,
rotate, compress, and their close
cousins — each anchored to what the
lab verified the engine does, so the
page's promises are checked, not
repeated.

## Merge: many files, one document

*Drop the files in the order you want
them, press Merge, save.*

The lab's version (transcript t01):
a three-page newsletter and a
two-page set of minutes went in; a
single five-page PDF came back, named
`newsletter-spring_merged_unsigned.pdf`.
The check was not "a file arrived" —
the merged file's pages were counted
(5 = 3 + 2) and read: page 1 was the
newsletter's first page, page 5 the
minutes' last, in the order given.

Worth knowing:

- Order is the order you add the
  files; the tool offers sorting by
  name if you want it (`sortType` in
  the API).
- Merging does not shrink anything;
  the result is roughly the sum of
  its parts.
- If one input is not actually a
  PDF, the job fails rather than
  guessing — chapter 16 shows the
  exact error (t10).

## Split: one document, several files

*Drop the file, say which pages go
where, press Split.*

The lab split the three-page
newsletter by **all** pages (t02) and
received a zip holding three one-page
PDFs, each still carrying its own
footer — *page 1 of 3*, *page 2 of
3*, *page 3 of 3* — three parts,
totalling three pages, nothing lost
or duplicated. The page selector is
the whole skill here: it accepts
`all`, single pages (`2`), lists
(`1,3`), and ranges as the tool
documents them, and each choice
produces a zip of the pieces.

Nearby cousins: split by size or by
chapter exist in the same family for
the day a 200-page scan needs to
become email-sized pieces. They were
not exercised in the lab; the split
that was is the one above.

## Rotate: the sideways scan

*Drop the file, pick a quarter turn,
press Rotate.*

Rotation in a PDF is a number stored
in each page — its `/Rotate` entry —
and honest checking means reading the
number, not eyeballing a thumbnail.
The lab rotated both pages of the
minutes by 90 degrees and read the
result back (t03): two pages, each
carrying `/Rotate = 90`, page size
untouched. Two consequences worth
knowing:

- Rotating is not re-drawing. The
  page's contents are unchanged; the
  *instruction to viewers* changes.
  It is fast and lossless.
- If a page still looks wrong in one
  stubborn viewer afterwards, the
  chapter 8 tool *auto-rotate* —
  which guesses from the text's
  direction — is the heavier hammer,
  and it needs Tesseract present.

## Compress: smaller, maybe

*Drop the file, pick a level, press
Compress.*

Compress is the shelf's most
oversold tool, so the lab's result is
worth stating exactly (t09): the
3.3 KB text-only newsletter went in at
optimisation level 2 and came out at
3,326 bytes — **the same size**, all
three pages and its title intact.
There was nothing to gain: a small,
already-tight file has no fat. The
gains live in files heavy with
images, where compression re-encodes
pictures at lower weight. Judge the
tool by your fattest scan, not by a
text file — and keep the original
until you have opened the result,
because the space is saved out of
quality somewhere.

Under the hood, note the pattern from
chapter 2: the startup log listed
compress among tools affected by
missing Ghostscript, qpdf and
ImageMagick — and it worked anyway,
through a built-in implementation.
*Affected* and *absent* are different
words in this program's log.

## The close cousins

- **Rearrange and remove pages** —
  the visual editor's home ground:
  drag pages into order, delete the
  blank scanner page at the end.
  Same engine family as split and
  merge.
- **Update metadata** — title, author
  and friends. The polite fix for a
  file whose tab title says
  "scan0042."
- **Add watermark** — a word like
  DRAFT laid across each page, from
  the Secure family.
- **Repair** — an attempt on damaged
  files. In the lab's bare-jar run it
  sat out entirely (it needs qpdf or
  Ghostscript; t14), which is the
  honest summary of repair generally:
  worth a try, never a promise.

Check: you should be able to merge
two files and *prove* the result —
count its pages, read its first and
last — rather than trust that a
download happened. That habit is the
whole of chapter 9, automated.
