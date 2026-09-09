# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **template** for Valheim modpacks published to [Thunderstore](https://thunderstore.io/).
There is no source code, no build, no test suite, and no package manager. The artifact is a zip
that `publish.yml` assembles from a subset of this directory when a GitHub release is published.

A modpack does not bundle mod DLLs. `manifest.json` `dependencies` list mods by
`Author-ModName-Version`, and the mod manager (r2modman / Thunderstore Mod Manager) resolves and
downloads them. The repo only carries metadata, an icon, config overrides, and any custom plugins.

## Layout

| Path | Role |
|---|---|
| `manifest.json` | Package metadata + dependency list; the one file most edits touch |
| `icon.png` | Required; PNG, **exactly** 256×256, under 6 MiB |
| `config/` | Flat `*.cfg` overrides shipped with the pack |
| `plugins/` | Custom DLLs / asset bundles, if any |
| `.github/workflows/publish.yml` | Publishes to Thunderstore on GitHub release |
| `CUSTOMIZATION.md` | End-user guide for forking this template |

`config/custom_mod_settings.json` and the two `README.txt` files are illustrative placeholders,
not consumed by anything.

## Thunderstore rules that bite

Verified against the server source
([consts.py](https://github.com/thunderstore-io/Thunderstore/blob/master/django/thunderstore/repository/consts.py),
[icon.py](https://github.com/thunderstore-io/Thunderstore/blob/master/django/thunderstore/repository/validation/icon.py),
`package_upload.py`), not from docs:

- **`name` must match `^[a-zA-Z0-9_]+$`.** Dashes are rejected. The repo slug may keep dashes;
  the package name may not. Several template docs used to claim dashes were allowed — they were
  wrong, don't reintroduce it.
- **`icon.png` must be exactly 256×256.** Not a minimum. A 512×512 icon fails the upload.
- **`version_number` must be exactly `N.N.N`**, 16 chars max, no `v` prefix, no suffixes.
- **`description` is capped at 256 characters** and is not markdown.
- **`README.md` is required** in the package or the upload errors out. `CHANGELOG.md` is
  optional and gets its own rendered tab.
- Dependency **order is cosmetic**. Thunderstore treats `dependencies` as a set; BepInEx load
  order comes from each plugin's `[BepInDependency]` attributes.

## Config override placement

r2modman matches a package directory by *basename* against Valheim's install rules
(`BepInEx/config`, `BepInEx/plugins`, …). So top-level `config/` works and maps to the profile's
`BepInEx/config/`. Configs use `trackingMethod: "none"` — they install flat, with no per-mod
subfolder, overwriting same-named files. `plugins/` is `subdir`-tracked and does get a per-mod
folder.

Never nest `config/BepInEx/config/`. That extra layer is copied verbatim and lands at
`BepInEx/config/BepInEx/config/*.cfg`, where nothing reads it.

## Release flow

Publishing is release-triggered only — pushing to `main` does not publish.

1. Bump `version_number` in `manifest.json`.
2. Commit and push.
3. Create a GitHub release tagged `vX.Y.Z`.
4. `publish.yml` verifies the tag matches `manifest.json`, stages `dist/`, and uploads.

Thunderstore rejects a version that has already been published, so every change needs a new
version. Requires a repo secret `THUNDERSTORE_TOKEN` with "Upload Packages" permission.

`manifest.json` is the single source of truth. The workflow reads `name`, `description`,
`website_url` and `dependencies` out of it with `jq` and passes them to the action, because
`GreenTF/upload-thunderstore-package` does **not** read `manifest.json` — its `cfg_edit.js`
generates a fresh `thunderstore.toml` from action inputs. Renaming the pack therefore means
editing `manifest.json` `name` and `website_url` only.

The `repo: https://thunderstore.io` input is load-bearing. The action's `entrypoint.sh` falls
through to an empty `repo` when it is unset, and the unquoted `--repository ${repo}` then drops
the argument so `--repository` swallows `--file`. Don't remove it.

`pip install tcli` was in an earlier version of this workflow. PyPI `tcli` is unrelated
abandoned software from 2019 with no `publish` command; the real Thunderstore CLI is a .NET
tool. Never reintroduce that line.

## Verifying changes

There is no build, lint, or test command. The global pre-commit rule to "run the full test
suite" has nothing to run here. These substitute for it:

```bash
python3 - <<'PY'
import json, re
m = json.load(open('manifest.json'))
assert re.fullmatch(r'[a-zA-Z0-9_]+', m['name']), m['name']
assert re.fullmatch(r'\d+\.\d+\.\d+', m['version_number'])
assert len(m['description']) <= 256
for d in m['dependencies']:
    assert re.fullmatch(r'[^-]+-[^-]+-\d+\.\d+\.\d+', d), d
print("manifest OK")
PY

python3 - <<'PY'
import struct, os
d = open('icon.png','rb').read()
assert d[:8] == b'\x89PNG\r\n\x1a\n'
w, h = struct.unpack('>II', d[16:24])
assert (w, h) == (256, 256), (w, h)
assert os.path.getsize('icon.png') < 6*1024*1024
print("icon OK", w, h)
PY

git check-ignore -v config/BepInEx/config/mod1.cfg   # must print nothing
ruby -ryaml -e "YAML.safe_load(File.read('.github/workflows/publish.yml'), aliases: true); puts 'yaml OK'"
```

PIL and PyYAML are not installed on this machine; the stdlib PNG parse and `ruby -ryaml` above
are the working substitutes. Don't swap them back.

To rehearse a release without burning a version number, set `dev: true` on the publish action
to target `thunderstore.dev`.

## Known gaps

- No release has ever run from this repo, so the workflow is untested end to end.
- `GreenTF/upload-thunderstore-package@v4.3` is an unofficial community action (the Thunderstore
  wiki recommends it; there is no official one). Its last release was Dec 2023.
- Dependency pins go stale fast. They were current on 2026-09-09, the day Valheim 1.0
  "Deep North" shipped. `denikson-BepInExPack_Valheim-5.4.2350` was published that same day and
  is presumed to be the 1.0 compatibility release, but maintainers have not confirmed that in
  writing. Re-check both pins before relying on them.

## Editing conventions

`publish.yml` and the `.txt` files are heavily commented to teach the reader — keep that tone
when editing template files. Documentation lives in overlapping places (`README.md`,
`CUSTOMIZATION.md`, the `README.txt` files); a change to structure or the release flow usually
needs updating more than one.
