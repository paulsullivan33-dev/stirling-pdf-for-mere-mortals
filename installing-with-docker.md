# Installing with Docker

**Scope:** the container route, taught
from the project's own documentation.
**Read this first:** the lab did *not*
run Docker — the build sandbox has no
container runtime on it — so this
chapter is documented, not verified.
The jar route in chapter 3 carries the
verified weight of this guide; treat
what follows as the project's manual,
organised, with the pitfalls marked.

## Why people choose this route

Chapter 2's helpers. The Docker images
bundle LibreOffice, Tesseract and the
rest, so the disabled-endpoint report
that greeted the lab's bare jar never
appears: the full shelf, OCR included,
works out of the box. In exchange you
accept a large download and one more
layer between you and the program.

## The three images

From the project's documentation:

| Image tag | Login | Includes | Suggested memory |
|---|---|---|---|
| `stirlingtools/stirling-pdf:3.2.1` | Yes | OCR, office, HTML and e-book conversion helpers | 4 GB |
| `...:3.2.1-fat` | Yes | The standard image plus a bundled AI engine, off until enabled | 6 GB |
| `...:3.2.1-ultra-lite` | **No** | Core PDF tools only; no OCR, no conversions | 2 GB |

Images are published to the project's
own registry (`docker.stirlingpdf.com`),
Docker Hub and GitHub's registry under
the same name. Two habits worth
starting with: **pin a version tag** as
above rather than `latest` — a moving
target makes chapter 12's upgrade
conversation impossible — and know that
**ultra-lite has no login at all**:
anyone who can reach its page can use
it. On a network you trust, that may be
a fair trade for the small footprint;
chapter 10 is the discussion.

## The compose file

The working example in `examples/`
follows the project's documentation:

```yaml
services:
  stirling-pdf:
    image: docker.stirlingpdf.com/stirlingtools/stirling-pdf:3.2.1
    container_name: stirling-pdf
    ports:
      - "8080:8080"
    volumes:
      - ./data/tessdata:/usr/share/tessdata
      - ./data/configs:/configs
      - ./data/customFiles:/customFiles
      - ./data/logs:/logs
      - ./data/pipeline:/pipeline
    environment:
      - SECURITY_ENABLELOGIN=true
      - SYSTEM_DEFAULTLOCALE=en-US
      - LANGS=en_GB,en_US
    restart: unless-stopped
```

Start it the usual way:

```
docker compose up -d
```

Check (documented): the status address
from chapter 3 — `http://localhost:8080/api/v1/info/status`
— answers with the version and `"UP"`,
and the project's own health check asks
it the same question.

## The volumes are the install

Four of those folders are chapter 2's
folders wearing Docker names, and one
is new:

- **`configs`** — settings, the account
  database, and `credential-encryption.key`.
  This is the folder chapter 12 backs
  up; losing it loses your users and
  any stored secrets for good.
- **`tessdata`** — Tesseract's language
  files. OCR reads the languages it
  finds here; `LANGS` in the compose
  file tells the image which to fetch.
  Chapter 8 explains.
- **`customFiles`**, **`logs`**,
  **`pipeline`** — as in the jar route.

Skip a volume and the corresponding
piece resets every time the container
is recreated — the classic Docker
surprise, and the reason this section
exists.

## Login, first time

The standard and fat images expect a
login. The first admin can be seeded
from the compose file on first boot:

```yaml
      - SECURITY_INITIALLOGIN_USERNAME=admin
      - SECURITY_INITIALLOGIN_PASSWORD=<redacted>
```

Use your own inventions, obviously —
and remove or change the password line
once the account exists; it only acts
on first creation. Chapter 10 covers
the login build's behaviour, which the
lab verified against the login *jar*;
the machinery is the same.

## Where this chapter stops

Reverse proxies, TLS and exposing any
of this beyond your network are
deliberately not here — the neighbours
for that job are the series' Caddy and
Tailscale guides, and chapter 10 states
the risks plainly. If you ran the jar
in chapter 3 instead, you lose nothing
but the bundled helpers, and chapters
5 onward are identical for both
routes.
