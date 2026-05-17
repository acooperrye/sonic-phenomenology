# Change Orders Inbox

This is the inbox the `sonic-phenomenology-maintainer` skill watches.

When a musicology / analysis session in any Claude surface notices a problem with
the engine architecture itself (not the track being analyzed), it writes a
**change order** here as a structured markdown file. The analysis session does
not modify the engine. The change order sits in this folder until a dedicated
maintainer session picks it up.

## Filename convention

`YYYY-MM-DD-short-slug.md` — e.g. `2026-05-17-percussion-slope-flat-bias.md`.

If two orders land on the same day, append `-a`, `-b`, etc.

## Lifecycle

1. **pending** — written by an analysis session, sitting here unprocessed.
2. **in-progress** — a maintainer session is currently working on it (the
   frontmatter `status` field is flipped during processing).
3. **processed** — done. The maintainer skill moves the file to
   `processed/<original-filename>` and records the commit SHA in frontmatter.

## Folder lockdown — non-negotiable

The maintainer skill will refuse to apply a change order that renames or
reorders the canonical stage folders. The stage names and their order are
public-facing — end users live-fetch against them, and downstream plugin users
hard-code these paths. The locked names are:

```
0-shared/
1a-binary-engine/
1b-web-engine/
2-activation-layer/
3-cultural-engine/
4-interpretive-engine/
dictionary/
internal/
```

Adding files inside these folders is fine. Renaming or reordering them is not.
A change order that proposes such a rename must be rejected by the maintainer
or rewritten to preserve the folder names.

## Local-first edit rule

Every change order processed against this repo modifies the **local copy at
`/Users/acr/Documents/Sonic Phenomenology/` first**. The maintainer skill never
edits via the GitHub web UI, never proposes changes through a PR fork, and
never pushes before validating the local edits. The flow is always:

1. `git pull` the local clone.
2. Edit files locally.
3. Validate locally.
4. Stage + commit locally.
5. Delegate the push to `repo-maintainer`.

If anything fails between steps 2 and 4, no push happens.

## See also

- `EXAMPLE-*.md` in this folder — a worked example showing the change-order format.
- `../../_maintainer-skill/SKILL.md` — the skill that processes this inbox.
- `../../_maintainer-skill/templates/change-order-template.md` — the empty template.
