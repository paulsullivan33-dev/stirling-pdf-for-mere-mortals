# Backing Up and Upgrading

**Scope:** the shortest backup chapter
in this series — because the workshop
keeps almost nothing — plus how to
move to a new version without drama.

## What there is to keep

Stirling PDF does not store your
documents. Files arrive with a
request and leave with the response
(chapter 2; the lab's folders held no
processed PDF at any point). So there
is exactly one pile worth backing up:

| Keep | Why |
|---|---|
| **`configs/`** | Settings, and on login builds the account database (`stirling-pdf-DB-*.mv.db`) and **`credential-encryption.key`** |
| **`customFiles/`** | Your customisation raw material, if you use it |
| **`pipeline/`** | Your watched-folder recipes, if you use them |
| The jar itself (or its version number) | So a restore can fetch the same release again |

`logs/` is a diary, not a possession;
back it up only if you are mid-diagnosis.

The bold entry deserves its sentence:
on a login build, `credential-encryption.key`
is created on first start, and any
secrets the server has stored
encrypted — integration credentials,
for instance — **cannot be recovered
without it**. The project's own first
start warns to back it up. Treat the
configs backup the way chapter 10
treats the folder itself: it contains
your users and your key, so it
deserves the same discretion as the
documents you never upload anywhere.

## The drill

1. Stop the server (so the database
   file is quiet).
2. Copy `configs/`, `customFiles/`
   and `pipeline/` somewhere that is
   not this machine — the series'
   backups and rclone guides are the
   vehicles.
3. Start the server; ask the status
   address (chapter 5). `UP` means
   the drill's first half is done.
4. The restore half, rehearsed once
   while it doesn't matter: on a
   spare folder, lay the same jar
   beside the backed-up `configs/`,
   start it, and confirm your
   settings — and on a login build,
   your sign-in — survived the trip.

Step 4 is the difference between a
backup and a hope; it takes five
minutes at household scale, which is
exactly why household scale is the
time to do it.

## Upgrading

The release cadence is quick (3.2.1
was a day old when the lab ran), so
upgrading is a standing habit, not an
event:

1. **Read the release notes.** A
   project moving this fast moves
   furniture; the notes say what.
2. **Back up `configs/`** — the drill
   above, compressed to its first two
   steps.
3. **Swap the program, keep the
   folders.** Jar route: download the
   new jar beside the old one's
   folders (keep the old jar until
   the new one proves itself). Docker
   route: change the pinned tag,
   pull, and restart (chapter 4).
4. **Verify like a first run:** the
   status address for the new
   version, the startup log for the
   dependency report (a helper that
   vanished in an OS upgrade shows up
   here first), and one everyday job
   with its chapter 7 check — merge
   something and count the pages.
5. On a login build, sign in. Your
   users live in `configs/`, so they
   should simply be there; the day
   they are not is the day the drill
   in step 2 earns its keep.

## Moving house

Because the state is folders,
migrating machines is the restore
drill with a longer walk: copy the
jar and the three folders to the new
machine, install Java 25 there
(chapter 3's lesson travels), start,
verify. The documents never moved,
because they were never there.

Check: you should be able to name
the folder, the key file inside it,
and the four verify steps after an
upgrade — and to have done the
restore rehearsal once, before it
is a rehearsal no longer.
