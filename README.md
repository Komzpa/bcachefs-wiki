# bcachefs wiki mirror

This is a mirror of the bcachefs wiki source from:

- https://evilpiepirate.org/git/bcachefs-wiki.git/
- https://bcachefs.org/

## Local preview

Use the repository helper instead of calling `ikiwiki . out` directly:

```sh
scripts/build-preview --serve
```

The default output directory is `${TMPDIR:-/tmp}/bcachefs-wiki-site`. Pass
`--out DIR` to choose a different destination.

The output directory is recreated on each build. The helper refuses obviously
dangerous destinations such as `/`, `$HOME`, or the repository root.

`--serve` starts a local preview at:

```text
http://127.0.0.1:8777/
```

Use `--port PORT` to change the preview port.

## Why the helper exists

The live site is not produced by IkiWiki's bare defaults. A plain command like:

```sh
ikiwiki . /tmp/bcachefs-wiki-site
```

builds something that looks different: the title becomes `wiki`, sidebar/actions
and feed links are missing, and newer IkiWiki versions emit HTML5 template tags
such as `article`, `section`, and `nav`.

The live site is closer to this setup:

- `wikiname => "bcachefs"`
- `rcs => "git"` so page dates come from Git metadata
- `sidebar`, `recentchanges`, `inline`, `meta`, `htmlscrubber`, and `goodstuff`
  plugins enabled
- RSS and Atom enabled
- `html5 => 0` for the older `div.pageheader`, `div.actions`, and
  `div.sidebar` template shape used by the current live CSS/layout

IkiWiki `3.20250221` and newer default `html5` to enabled, so this helper writes
a temporary setup file with `html5 => 0`. Current IkiWiki warns that option is
deprecated, but it is still needed to reproduce the live markup shape.

One remaining known difference: the live server has at least one extra static
asset linked from the sidebar (`bcachefs_talk_2022_10.mpv`) that is not present
in this git clone. Until that asset is mirrored too, local builds may show that
link as missing/createlink.
