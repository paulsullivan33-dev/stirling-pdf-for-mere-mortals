# Settings and Customisation

**Scope:** how this program is
configured — the settings file it
writes for you, the environment
variable override that beats it, and
the handful of settings a household
actually touches.

## The settings file

On first start the server writes
**`configs/settings.yml`** — a long,
heavily commented YAML file covering
security, the interface, the system,
and more (an extract lives in
`examples/`). Three of its own rules,
quoted from its header and behaviour,
matter more than any single setting:

- **Do not comment entries out.** The
  file warns that an entry commented
  out *will be removed on the next
  startup* — the server tidies the
  file back to its full template.
  Change values; don't delete lines.
- **Your own file sits beside it.**
  `custom_settings.yml` is the place
  for overrides you want kept apart
  from the generated template.
- **Environment variables override
  both.** A setting named
  `system.googlevisibility` in the
  file is set from the shell as
  `SYSTEM_GOOGLEVISIBILITY` — dots and
  camelCase become underscores and
  capitals. The file's own header
  gives the pattern
  (`SECURITY_INITIALLOGIN_USERNAME`).

## The override, proven

Scepticism is a virtue with config
systems, so the lab proved the
mechanism end to end (t12, t13): by
default the server answers
`/robots.txt` with

```
User-agent: *
Disallow: /
```

Restarted with one environment
variable —

```
SYSTEM_GOOGLEVISIBILITY=true java -jar Stirling-PDF.jar
```

— the same address answered:

```
User-agent: *
Allow: /
```

One variable, one restart, one
visible flip. (The lab then stopped
that server; a public-facing
household should leave the default
alone — chapter 11.) This is the
shape of every configuration story
in this chapter: find the setting's
name in `settings.yml`, set the
variable or edit the value, restart,
and verify at the address the
setting governs.

## The settings a household touches

| Setting (file → environment) | What it does | Lab status |
|---|---|---|
| `security.enableLogin` → `SECURITY_ENABLELOGIN` | Login on/off in builds that carry it | Login build verified (t11) |
| `security.loginAttemptCount` (5), `loginResetTimeMinutes` (120) | Lockout manners | Read from the generated file |
| `security.initialLogin.*` → `SECURITY_INITIALLOGIN_*` | Seeds the first admin | Documented; chapter 4 |
| `system.defaultLocale` → `SYSTEM_DEFAULTLOCALE` | Interface language for new users | In the file |
| `system.maxFileSize` family / upload limits | How large a file a job may take; the 413 error when exceeded | Named in the API's responses |
| `ui.appName` and friends → `UI_APPNAME` | The name in the page and navbar | Shown in the project's compose examples |
| `system.googlevisibility` → `SYSTEM_GOOGLEVISIBILITY` | Search-engine welcome mat | **Verified both ways (t12, t13)** |
| Analytics toggles (`enableAnalytics`, PostHog, Scarf) | Product analytics, default *ask* | Read from the generated file |

The port from chapter 3
(`SERVER_PORT`) is the same trick in
Spring Boot's own clothes — the
framework the program is built on
honours it everywhere.

## Customisation beyond settings

- **`customFiles/`** holds the raw
  material for personal touches —
  the settings file points here for
  things like the login disclaimer
  texts, per language.
- **The interface name and
  description** (`UI_APPNAME`,
  `UI_HOMEDESCRIPTION`,
  `UI_APPNAMENAVBAR`) let a household
  server introduce itself properly —
  *The House PDF Bench* beats a
  default when three people share it.
- **Turning tools off on purpose.**
  The API's responses and the
  project's compose examples show an
  *endpoint groups* switch
  (`ENDPOINTS_GROUPS_TO_REMOVE`) for
  hiding whole families — a parent
  might retire the tools that modify
  files on the kids' shared instance.
  Documented here, not lab-run.

## Restraint, as a feature

The settings file is hundreds of
lines, and nearly all of it is
defaults doing quiet work: OAuth
providers unconfigured, SAML
unconfigured, signature-trust lists
off, audit logging available. The
household path through it is short:
set the login behaviour, the locale,
a name, a file-size limit you
understand — then stop, and let the
file's comments be the manual for
the rest, on the day you need them.

Check: you should be able to change
one setting two ways (file and
environment), prove the change at
the address it governs, and explain
why a commented-out line disappears.
