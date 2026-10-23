# Glossary

The terms this guide uses, collected
in one place. Each is explained in the
chapter that leans on it; these are
the short versions.

**API** — the programming interface:
the set of web addresses (`/api/v1/...`)
that the web page, and any script, call
to do jobs. If the page can do it, the
API can do it.

**Endpoint** — one of those addresses,
doing one job: `/general/merge-pdfs` is
an endpoint. A tool whose helper is
missing has a *disabled* endpoint.

**Fat jar** — a single Java file that
carries a whole application and its
libraries. Stirling PDF's releases are
fat jars: download one file, run it.

**H2 database** — the small embedded
database the login build keeps its
user accounts in, as one file inside
`configs/`.

**Helper** (external dependency) — a
separate program some tools borrow:
LibreOffice for office files,
Tesseract and OCRmyPDF for OCR,
Ghostscript and qpdf for repair work.
Missing helpers mean missing tools.

**Java / JVM** — the language the
program is written in, and the runtime
that executes it. Version matters
harshly here: the current release
needs Java 25.

**Multipart form** — the web's
file-upload shape: a request with
named fields, files attached. The
API's file field is called
`fileInput`.

**OCR** — optical character
recognition: software reading the
letters in a picture of a page and
adding a text layer, so the page can
be searched and copied from.

**OpenAPI document** — a machine-readable
map of an API. Your server serves its
own at `/v1/api-docs`; the browsable
version is Swagger UI.

**Pipeline** — Stirling's watched-folder
automation: recipes in the `pipeline/`
folder that process files dropped
into watched places.

**Port** — the numbered door a server
listens on. Stirling PDF's default is
8080; `SERVER_PORT` moves it.

**Reverse proxy** — a server that
stands in front of another, adding
doors like HTTPS and its own
sign-in. Chapter 10's suggested
chaperone for anything exposed.

**Sidecar** — in OCR, a plain-text
file produced *beside* the PDF,
holding the words the OCR found.

**Swagger UI** — the browsable face
of an OpenAPI document: your server's
API manual, at `/swagger-ui`.

**Tessdata** — Tesseract's language
data files. The engine reads only
the languages whose data it has;
Docker mounts them at
`/usr/share/tessdata`.

**Text layer** — the invisible words
inside a PDF page, in their places.
Born-digital PDFs have one; scans
don't, until OCR adds it.
