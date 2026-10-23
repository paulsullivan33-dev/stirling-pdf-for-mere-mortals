# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Built and verified 2026-10-22 against
**Stirling PDF v3.2.1** (GitHub release
published 2026-10-21, the current release on
build day), running the project's own release
jars on a Linux machine.

**Primary sources read, not remembered:** the
release page and its jar files (downloaded and
size-checked); the program's own generated
`settings.yml`, startup dependency report and
OpenAPI document (148 paths) taken from the
running server; and the project's
documentation and official compose examples
for everything labelled documented-only
below.

**Ran for real** (transcripts in
`stirling-pdf-lab/transcripts/`, t00–t15).
The specimens were generated in the lab —
reportlab wrote the newsletter and minutes,
Pillow drew the image and the scanned page,
and LibreOffice produced the `.docx` — all of
them invented documents for a fictional
garden club:

- ✅ **Version and home page (t00):**
  `/api/v1/info/status` answered
  `{"version":"3.2.1","status":"UP"}` and
  the web page served; startup took ~20 s
  for the plain jar.
- ✅ **Merge (t01):** two generated PDFs
  (3 pp + 2 pp) merged via
  `/api/v1/general/merge-pdfs` into a 5-page
  result; first and last pages verified by
  extracted text.
- ✅ **Split (t02):** `/split-pages`
  returned a zip of three one-page PDFs in
  the right order, page counts totalling 3.
- ✅ **Rotate (t03):** both pages of the
  result carry `/Rotate = 90`, read back
  with pypdf; page size unchanged.
- ✅ **PDF → text (t04):** extracted text
  contains both specimen headlines exactly
  once each.
- ✅ **PDF → image (t05):** a real PNG,
  2480×3507 — A4 at the default 300 dpi.
- ✅ **Image → PDF (t06):** one A4 page,
  image embedded.
- ✅ **Office → PDF (t07):** the `.docx`
  converted through the machine's
  LibreOffice; its text survived.
- ✅ **Compress (t09):** at level 2 the
  3,326-byte specimen came back 3,326 bytes
  — reported in chapter 7 as no gain, not
  as a success.
- ✅ **Error behaviour (t10):** a text file
  sent to merge returned HTTP 500 with the
  JPDFium failure message, quoted in
  chapters 10 and 17.
- ✅ **Login build (t11):** the with-login
  jar served its sign-in page, refused an
  unauthenticated API call with 401, and
  kept the status address public.
- ✅ **Configuration (t12, t13, t15):**
  robots.txt defaults to `Disallow: /`;
  `SYSTEM_GOOGLEVISIBILITY=true` flipped it
  on restart; `SERVER_PORT=8081` moved the
  server. The settings file's lockout
  figures (5 attempts, 120 minutes) were
  read from the generated file.
- ✅ **Dependency report (t14):** ten
  missing helpers and sixteen disabled
  endpoints recorded from the startup log,
  including the nuance that compress worked
  despite its group being listed —
  fallback implementations, in the log's
  own words.

**Labelled documented-only, never claimed
as run:**

- ❌ **Docker route (chapter 4):** the
  sandbox has no container runtime. The
  chapter follows the project's own
  documentation and compose examples, and
  says so in its first paragraph.
- ❌ **OCR succeeding (chapter 9):** the
  lab collected the honest refusal — HTTP
  403, "This endpoint is disabled" (t08) —
  because tesseract and ocrmypdf were
  missing and the sandbox's package
  manager was unavailable to install
  them. The equip path is documented;
  no OCR output is claimed anywhere.
- ❌ **Pipelines / watched folders:**
  the `pipeline/` folder was seen
  created; the feature was not
  exercised (recipe 5 says so).
- ❌ **SSO, OAuth2, SAML:** read in the
  generated settings file only; SAML is
  marked there as a paid Enterprise
  feature.
- ❌ **Desktop installers, the AppImage
  and the server jar:** they exist on
  the release page; none was run.
- ❌ **UI click-paths:** the page was
  loaded to confirm it serves (t00); the
  jobs were driven and verified at the
  API, the same engine the page calls.
  Chapter 5 states this.
- ❌ **A systemd service:** chapter 3
  describes the pattern; the health
  check such a unit would use (the
  status endpoint) is the part that was
  verified.
- ❌ **Windows and macOS:** the Java 25
  requirement and the jar route are the
  project's cross-platform shape, but
  this lab ran on Linux only.

**Surprises the guide reports rather than
hides:** the release requires Java 25 and
refuses Java 21 outright (chapter 3 keeps
the error text); the login build is a
separate, larger jar (386 MB against
298 MB, both measured); merge results
arrive named with an `_unsigned` suffix;
and a "disabled group" in the startup log
does not always mean a dead tool.
