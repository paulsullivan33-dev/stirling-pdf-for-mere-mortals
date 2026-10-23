# Keeping It Private

**Scope:** the security chapter, and
the one that justifies the whole
project: what "private" does and does
not mean for a self-hosted PDF
workbench, the plain build's missing
login, and the login build's verified
behaviour.

## The point, stated plainly

People reach for Stirling PDF because
their documents are sensitive. Running
the tool at home removes the biggest
risk in the category — *your file on a
stranger's server, under a stranger's
privacy policy* — completely. It does
not remove the smaller risks, and this
chapter's job is to name them at their
true size:

- Whoever can **reach the page** can
  use the tools — on the plain build,
  that is the whole statement.
- The server **sees your documents in
  the clear** while it works on them.
  That is what a workbench is. Its
  disk keeps no copies (chapter 2),
  but its network should be one you
  trust.
- A tool is only as private as the
  **helpers** it borrows — all of
  chapter 2's neighbours are local
  programs too; none of them phone
  home with your file. That is a
  property of *this* shelf, worth
  appreciating.

## The plain build has no login

Verified, and worth the bold type:
the standard jar answers every tool
to anyone who can open its page. The
lab's whole test suite — merges,
conversions, the lot — ran with no
credentials, because there was
nowhere to present any. On one
person's own computer, or a home
network you trust, that is a
reasonable design and the project's
default for this build. It also means
**the network is the lock**: if the
port is reachable from somewhere you
wouldn't hand your documents to, the
tools are reachable from there too.

Never expose the plain build to the
internet. Not "with a strong
password" — there is no password.
Not behind an obscure port — ports
get scanned. If the server must be
reachable from away, put a real door
in front of it: the login build
below, a VPN (the series' Tailscale
guide), or a reverse proxy with its
own authentication (the Caddy guide),
and TLS if it crosses public wires.

## The login build, verified

`Stirling-PDF-with-login.jar` is the
same workshop with an account book.
The lab ran it and probed it
(transcript t11):

- `/login` serves a sign-in page
  (*Sign In - Stirling PDF*).
- An API call with no credentials:

  ```
  HTTP 401  {"error":"Authentication required"}
  ```

- The settings file (chapter 11)
  carries the manners: an account
  locks after **5 failed attempts**,
  for **120 minutes** by default —
  figures you can change, and should
  know before you lock yourself out
  (chapter 16).
- The first admin is seeded by
  settings or the `SECURITY_INITIALLOGIN_*`
  environment variables (chapter 4);
  after that, users are managed in
  the app, and OAuth2 sign-in can be
  configured in the same settings
  file. SAML SSO exists too — marked
  in the settings as an Enterprise
  extra, and outside this guide.

One exception, verified and fair:
the **status address stays public**
on the login build — it answered
`{"version":"3.2.1","status":"UP"}`
without credentials (t11). It does
no work and opens no files; it
announces that a Stirling PDF of a
certain version is alive. If even
that is more than you want to tell
the network, the answer is the same
as above: don't give the network the
port.

## Smaller print worth knowing

- **`configs/` is a secrets folder**
  on a login build: it holds the
  account database and
  `credential-encryption.key`. File
  permissions are part of security
  here; chapter 12 backs both up,
  and you should guard the backups
  the same way.
- **Analytics switches exist.** The
  settings file carries toggles for
  product analytics (PostHog and a
  Scarf pixel), defaulting to *ask
  on first launch*. Know they are
  there; set them deliberately
  rather than by reflex, either way.
- **Search engines are kept out** by
  default: the server serves a
  `robots.txt` that disallows
  everything (t12), unless someone
  deliberately flips
  `system.googlevisibility` (t13's
  demonstration flipped it, in a
  lab, on purpose, and flipped it
  back by stopping the server).
- **Documents in transit** on your
  own LAN travel unencrypted HTTP to
  the server. At home, most households
  accept that; if yours doesn't, the
  reverse-proxy guides are the fix,
  not a Stirling setting.

Check: you should be able to say
which build has a login, what an
unauthenticated stranger gets from
each build, and why the status
address is allowed to be the
exception. If any of that is fuzzy,
your install is not ready for a
second user.
