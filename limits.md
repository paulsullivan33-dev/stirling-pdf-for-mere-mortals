# Limits

**Scope:** what this tool will not
do, stated as plainly as the chapters
stated what it will. Collected from
the lab where possible; the rest from
the project's own file.

- **It demands a young Java.** The
  current release line is compiled
  for Java 25; the Java 21 many
  machines already have is refused
  with a class-version error
  (chapter 3). Expect this to keep
  moving — a fast-releasing Java
  project rides Java's own cadence,
  and your runtime becomes part of
  the maintenance.
- **The shelf depends on the
  machine.** Sixteen endpoints sat
  out on the lab's bare machine
  (t14). Two installs of "the same"
  Stirling PDF can honestly offer
  different tools; the Docker images
  narrow the gap by bundling the
  helpers, at the price of size.
- **It is not small.** The plain jar
  is 298 MB, the login jar 386 MB
  (measured on the release page's own
  files), and a JVM carries its
  appetite for memory on top —
  budget a home for it, not a
  corner. The Docker `-ultra-lite`
  image exists for the constrained,
  paid for with login and OCR.
- **OCR is a beginning, not a
  reading.** Even fully equipped
  (chapter 8), OCR guesses. It will
  misread a digit in a bank table
  with the same confidence as a
  headline. Verification of anything
  that matters stays a human job.
- **Conversion backwards is
  reconstruction.** PDF → Word and
  friends rebuild a plausible
  document from a picture of one
  (chapter 7). Layouts with columns,
  footnotes and floats will need
  repair by hand, in every tool in
  this field, this one included.
- **Compression is not a promise.**
  On an already-tight file the lab's
  result was byte-for-byte the same
  size (t09). The tool squeezes
  images; it cannot invent savings in
  text.
- **It is a bench, not a system of
  record.** No library, no archive,
  no version history of your
  documents, no multi-user document
  management beyond accounts and
  tools. Filing is another program's
  job (neighbours chapter).
- **The paid rooms are paid.**
  SAML SSO is marked Enterprise in
  the settings file; team and
  business features are the
  project's living. The free shelf is
  broad — this guide never wanted
  for a tool — but it is not the
  whole building.
- **A fast-moving project moves.**
  Releases arrive weekly-ish; menus
  rename, defaults shift, a chapter
  like this one ages. The defence is
  the standing one: pin versions,
  read release notes, verify after
  upgrading (chapter 12).
- **The lab's own ceiling.** This
  guide verified the jar route on
  one Linux machine. Docker is
  documented, OCR is shown only in
  refusal, and the desktop builds
  were never opened. `criteria.md`
  keeps the exact ledger; trust it
  over this chapter's summary of it.
