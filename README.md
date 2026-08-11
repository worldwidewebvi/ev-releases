# E.V. — releases

This repository exists for one reason: it is where E.V. looks when it checks
whether there is a newer version of itself.

It holds two things — `manifest.json`, and the build archives attached to each
release. **The source code is not here.** It lives in a private repository;
this one is public only because the app must be able to fetch its own updates
without carrying a credential on the user's machine.

## What the app actually does

On launch, and every few hours, E.V. fetches `manifest.json`:

```json
{
  "version": "0.5.4",
  "sha256":  "<64 hex characters of the zip>",
  "url":     "https://github.com/worldwidewebvi/ev-releases/releases/download/v0.5.4/jarvis-0.5.4.zip",
  "notes":   "what changed"
}
```

If `version` is newer than the one installed, an UPDATE button appears. One
click downloads the archive, **verifies it against the SHA-256 in the manifest**
and refuses to install anything that doesn't match, unpacks it beside the
current version, and moves a pointer file. The old version stays on disk, so a
bad release can be rolled back by moving the pointer back.

## What it does not do

The check is a plain HTTPS GET. It sends nothing about the user — no identifier,
no telemetry, no version ping beyond the request itself. E.V. is an offline
companion: this is the only thing it ever fetches, and everything a person says
to it stays in `%APPDATA%\Jarvis` on their own machine.
