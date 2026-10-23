# Scripting with the API

**Scope:** the same tools with no
clicking — the API's shape, the exact
commands the lab ran, and how to check
a result like you mean it. Every
command below was run against the
lab's server; transcripts t01–t10 are
the receipts.

## The shape of the thing

Every tool is one **POST** to an
address under `/api/v1/`, grouped the
way the shelf is grouped:

```
/api/v1/general/merge-pdfs
/api/v1/general/split-pages
/api/v1/general/rotate-pdf
/api/v1/convert/pdf/text
/api/v1/convert/pdf/img
/api/v1/convert/img/pdf
/api/v1/convert/file/pdf
/api/v1/misc/ocr-pdf
/api/v1/misc/compress-pdf
```

The request is a **multipart form** —
the web form idea, scripted: the file
travels in a field named **`fileInput`**,
and each option is another field. The
result comes back as the response
body: the finished file itself, with
its filename in the headers. The
project publishes the full reference
as an **OpenAPI** document — the lab
counted 148 addresses in it — served
by your own server at `/v1/api-docs`,
with a browsable version at
`/swagger-ui`. Your server is its own
manual; check there before believing
any list, including this one.

## The lab's commands, as a set

Merge two files into one:

```
curl -F "fileInput=@newsletter-spring.pdf" \
     -F "fileInput=@garden-minutes.pdf" \
     -o merged.pdf \
     http://localhost:8080/api/v1/general/merge-pdfs
```

(Merge takes `fileInput` more than
once — once per input file, in
order.)

Split every page into its own file:

```
curl -F "fileInput=@newsletter-spring.pdf" \
     -F "pageNumbers=all" \
     -o split.zip \
     http://localhost:8080/api/v1/general/split-pages
```

Rotate a document a quarter turn:

```
curl -F "fileInput=@garden-minutes.pdf" \
     -F "angle=90" \
     -o rotated.pdf \
     http://localhost:8080/api/v1/general/rotate-pdf
```

Convert — the pattern is
`/convert/<from>/<to>`:

```
curl -F "fileInput=@newsletter-spring.pdf" -F "outputFormat=txt" \
     -o newsletter.txt http://localhost:8080/api/v1/convert/pdf/text

curl -F "fileInput=@picnic-flyer.docx" \
     -o flyer.pdf http://localhost:8080/api/v1/convert/file/pdf
```

## Check the result, don't admire it

A 200 response and a downloaded file
prove a download, not a job. The lab
checked every result with a small
PDF reader library (Python's
`pypdf`; `qpdf --check` and friends
serve on other benches):

| Job | The check that means something |
|---|---|
| Merge | Page count equals the sum; first and last pages are the right ones |
| Split | The parts' page counts total the original; known text appears in the right part |
| Rotate | Each page's `/Rotate` reads the angle you asked for |
| PDF → text | A line you know is in the document is in the text |
| PDF → image | The file opens as an image of the expected size |
| Office → PDF | The text survived the trip |

That discipline is what turned the
compress test honest (t09): the
command "worked," and the check is
what let the guide report *no size
change on this file* instead of a
success story.

## When it goes wrong

The API's errors are plain. The lab's
deliberate mistake — sending a text
file to the merge tool (t10) — came
back:

```
HTTP 500
{"error":"Job failed: java.io.IOException: JPDFium merge failed"}
```

Not poetic, but it tells you the job,
the failure, and whose desk it died
on. The other statuses you will meet:
**400** for a malformed request,
**403** with *"This endpoint is
disabled"* for a tool whose helper is
missing (chapter 8), **401** for
*Authentication required* on a login
build (chapter 10), and **413** when
a file exceeds the size limit
(chapter 11). A script should treat
anything but 200 as "the result file
is an error message — read it before
proceeding," because with `-o` the
error JSON happily saves itself under
your PDF's name.

## From commands to a script

The step from these commands to a
useful script is small: a loop over a
folder, one call per file, a check
per result. Chapter 13's scan-batch
recipe is exactly that, grown up.
Two ground rules travel with it: keep
the server's address in one variable
at the top (port changes stop being
archaeology), and never let a script
assume yesterday's tool is on today's
shelf — the startup log, not hope,
says which endpoints a given server
answers.

Check: you should be able to merge
two files from the command line and
prove the count, and to say where
your own server keeps its API manual.
