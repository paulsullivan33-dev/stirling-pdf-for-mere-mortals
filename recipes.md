# Recipes

**Scope:** four standing arrangements
built from the verified pieces. Each
says which chapters it uses; none
needs anything this guide hasn't
shown.

## 1. The standing home install

*Chapters 3, 5, 12.*

- Java 25, the plain jar in its own
  folder, started by your system's
  service manager so it survives
  reboots.
- Bookmarked: the page for people,
  the status address for you.
- A calendar note, monthly: read the
  release page; if a new version is
  out, do chapter 12's five steps.
- The `configs/` folder joins your
  household backup like any other
  small, precious folder.

You now own the errand that used to
go to upload websites. That is the
whole recipe; the rest are variations.

## 2. The club newsletter run

*Chapters 6, 9.*

A newsletter assembled from pieces —
the editor's pages, the treasurer's
minutes, a flyer — merged in order
and checked before it goes to the
printer:

```
curl -F "fileInput=@newsletter-spring.pdf" \
     -F "fileInput=@garden-minutes.pdf" \
     -o print-run.pdf \
     http://localhost:8080/api/v1/general/merge-pdfs
```

Then the chapter 6 discipline: count
the pages (3 + 2 = 5 in the lab's
identical run, t01), read the first
and last, *then* send it. If the
printer wants it smaller, compress a
**copy** and compare — never let the
only version be the squeezed one
(t09's lesson).

## 3. A folder of scans, converted by script

*Chapters 7, 8, 9.*

The shape of every honest batch job
in this field — one folder in, one
folder out, every result checked,
failures listed rather than hidden:

```bash
SERVER=http://localhost:8080
mkdir -p done
for f in scans/*.pdf; do
  out="done/$(basename "${f%.pdf}").txt"
  code=$(curl -s -o "$out" -w '%{http_code}' \
    -F "fileInput=@$f" -F "outputFormat=txt" \
    "$SERVER/api/v1/convert/pdf/text")
  if [ "$code" != "200" ]; then
    echo "FAILED ($code): $f -- see $out for the error"
  fi
done
```

Three honest notes: the loop checks
the status of every call because an
error message will otherwise move
into your results wearing a `.txt`
name (chapter 9); it converts to
*text*, which works on documents that
have text — a folder of true scans
(photo-pages) comes back empty until
OCR is equipped (chapter 8), and the
empty files are the script telling
you so; and the same skeleton serves
any endpoint — change the address
and the fields, keep the checking.

## 4. The locked-down family server

*Chapters 4 or 3, 10, 11, 12.*

For a house where several people use
the bench and the documents are
properly private:

- The **login build** (jar, or the
  standard Docker image with its
  helpers), never the plain one.
- First admin seeded deliberately
  (`SECURITY_INITIALLOGIN_*` in
  chapter 4), then an account per
  person from inside the app.
- Lockout figures read once in
  `settings.yml` so nobody learns
  them by surprise (5 attempts, 120
  minutes by default).
- Reachable on the home network
  only; from away, through the
  family's VPN — never a bare port
  on the internet (chapter 10).
- `configs/` — users, key and all —
  in the household backup, guarded
  like the documents it protects.

## 5. Watched folders (the pipeline room)

*Documented, not lab-run.*

The program keeps a `pipeline/` folder
(chapter 2) for its automation
feature: recipes that watch a folder
and process whatever lands in it —
the scanner's output folder becomes,
say, deskewed, OCR'd, filed output
without a human pressing anything.
The lab saw the folder created and
did not exercise the feature; the
project's documentation describes
the recipe format. Build it on the
back of recipes 2 and 3 first — a
watched folder is a script that
nobody watches, and the checking
habits are the same ones, or they
are nothing.
