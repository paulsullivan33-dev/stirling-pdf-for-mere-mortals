# First Run and the Home Page

**Scope:** what you see when the server
wakes up, how the tool shelf is
organised, and the one address to
bookmark for "is it alive?"

**An honesty note before the tour:**
the lab drove the server's API — the
same calls the web page itself makes —
and loaded the page to confirm it
serves (transcript t00). The tour
below describes the page as served and
documented; the *work* each tool does
was verified at the engine, in
chapters 6 to 9, not by clicking.

## The first minute

Open `http://localhost:8080` (or your
chosen port). On the plain build you
land directly on the tool shelf. On a
login build — chapter 10 — a sign-in
page comes first; the lab's login jar
answered `/login` with the title
*Sign In - Stirling PDF* (t11).

Behind the page, that first start did
real work: it wrote `configs/`,
`customFiles/`, `logs/` and
`pipeline/` beside the jar (chapter
2), and in the log it left the
dependency report — which helpers it
found, which tools sat out. Read that
log once, early. It is the difference
between "this feature is broken" and
"this feature was never on my shelf,"
and chapter 16 will send you back to
it.

## The shelf

The page groups dozens of tools the
way a hardware shop groups stock.
Names vary slightly between releases,
but the families are stable, and they
mirror the API's own families
(chapter 9):

| Family | Typical jobs | Chapter |
|---|---|---|
| Organise | Merge, split, rotate, rearrange, remove pages | 6 |
| Convert to PDF | Images, office files, HTML | 7 |
| Convert from PDF | Text, images, Word, spreadsheets, e-books | 7 |
| Read and annotate | OCR, add images, watermarks | 8 |
| Secure | Passwords, permissions, signatures, redaction | 10 |
| Fix and measure | Compress, repair, compare, metadata | 6 |

Two habits make the shelf friendly:
the search box at the top finds a tool
by the verb in your head ("rotate,"
"merge"), and most tools follow one
rhythm — *drop the file, set the few
options, press the button, save the
result*. Learn one tool's rhythm and
you have learned the shelf's.

## The heartbeat

Bookmark this address somewhere you
keep such things:

```
http://localhost:8080/api/v1/info/status
```

It answers `{"version":"3.2.1",
"status":"UP"}` — no login needed, on
either build (t00, t11). It is meant
for exactly the question it answers:
*is the server up, and which version?*
Monitoring tools, health checks
(chapter 4's compose file uses it) and
your own scripts can all ask it. What
it does not do is any work on files —
chapter 10 explains why it is allowed
to stay public while everything else
on a login build is gated.

## Your first job, gently

Before the tour ends, do one harmless
thing in the page: take any PDF you
can afford to experiment on — this
guide's specimens were invented for
the purpose; a downloaded manual does
as well — and **rotate** it a quarter
turn and back. Watch the rhythm: drop,
choose, apply, save. Then, if you are
the curious sort, chapter 9 shows
the identical job as one command,
with the server's numbers checked at
the end.

Check: you should be able to find the
status address from memory, name the
folder the server wrote its settings
into, and say which family the merge
tool lives in. Chapter 6 does the
everyday jobs properly.
