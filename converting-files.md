# Converting Files

**Scope:** crossing the border in both
directions — PDFs into text, images
and office formats; images and office
files into PDFs — with each route
labelled *built in*, *borrowed*, or
*absent without its helper*, as the
lab found them.

## The map

Conversion is where chapter 2's helper
system bites hardest. The same menu
on two machines can hold different
tools:

| Route | Engine | In the lab |
|---|---|---|
| PDF → text | Built in | ✅ Ran (t04) |
| PDF → image (PNG/JPG) | Built in | ✅ Ran (t05) |
| Image → PDF | Built in | ✅ Ran (t06) |
| Office file → PDF | LibreOffice | ✅ Ran (t07) |
| PDF → Word / spreadsheet | Built in | On the shelf; not lab-run |
| PDF → e-book (EPUB/CBR) | Calibre / rar | ❌ Sat out (t14) |
| HTML / web page / email → PDF | WeasyPrint | ❌ Sat out (t14) |
| PDF → vector, vector → PDF | Ghostscript | ❌ Sat out (t14) |

The Docker images bundle the helpers,
which is why the same rows read
"works" in the project's screenshots.
On the jar route, *you* are the
bundle: install the helper, restart,
and the tool joins the shelf.

## PDF to text: the words, only the words

The lab converted the newsletter
(t04) and got a plain text file whose
headlines — *Seed swap on Saturday*,
*Soil testing kits available* — were
found exactly once each. What text
extraction gives you is the document's
words in reading order: no columns, no
fonts, no pictures. It is the right
tool for searching, quoting, feeding a
document to another program, or
checking what a PDF *says* as opposed
to what it *shows*. It cannot give
you words that are not there — a
scanned page is a photograph, and its
text layer is empty (chapter 8's
specimen proved that before OCR was
even attempted).

## PDF to image: pages as pictures

Converting the minutes' first page at
the default settings produced a PNG
of 2480 × 3507 pixels — an A4 page
rendered at 300 DPI (t05). Options
that matter: the format (PNG for
crisp pages, JPG for smaller files),
the DPI (300 is print territory; 150
is plenty for a screen), and one image
per page versus a single tall strip.
Reach for this when a program wants a
picture of a page — a slide, a
thumbnail, an attachment a stubborn
system will accept.

## Image to PDF: the envelope for pictures

The lab's generated photo-walk image
came back as a one-page A4 PDF with
the picture embedded (t06). Several
images become several pages, in
order — the quick way to turn a
phone's worth of photographed
whiteboard notes or form pages into
one sendable document. Remember what
it is: an *envelope*. The pages are
pictures; their words are not text
until chapter 8's OCR has been
through them.

## Office files to PDF: LibreOffice does the work

Stirling's *file to PDF* is a polite
front desk for **LibreOffice**: Word
documents, spreadsheets, presentations
and their OpenDocument cousins are
handed to LibreOffice headless, and
the PDF it produces is handed back.
The lab's picnic-flyer `.docx` came
back a one-page PDF with its text —
picnic, *dish to share* and all —
intact (t07). Two honest consequences:

- **Fidelity is LibreOffice's, not
  Stirling's.** A document that leans
  on a font or feature LibreOffice
  renders differently will show it.
  Open the result before you send it.
- **No LibreOffice, no tool.** On a
  machine without it (or its
  `unoconvert` front end) this route
  is one of the startup log's missing
  dependencies.

## PDF to Word and friends

The shelf also offers PDF → Word,
spreadsheet, slides, CSV, XML and
Markdown. These are the project's own
implementations and were on the lab's
shelf — but the lab did not verify
them, and conversion *backwards* out
of PDF is where every tool in this
field earns an asterisk: a PDF
remembers what the page looked like,
not what the document *meant*, so
columns, tables and reading order are
reconstructed by educated guessing.
Expect a good draft, never the
original document back. The
neighbours chapter names the
specialists if this is your main job.

Check: you should be able to say,
for each row of the map, whether the
tool is Stirling's own, a borrowed
neighbour's, or missing on a bare
install — and to predict, from a
startup log alone, which conversions
a given machine will refuse.
