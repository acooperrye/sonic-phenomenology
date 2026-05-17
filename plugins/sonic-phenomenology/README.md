# sonic-phenomenology — Cowork plugin

A Claude plugin that lets you run a phenomenological reading of any piece
of music: audio in one ear, artist-and-title context in the other,
cultural conventions as a third independent axis, and your own felt
response as the final ground truth.

**Convergence across independent axes is the signal.**

## What this plugin gives you

One skill, `sonic-phenomenology`, that activates when you ask Claude to
analyse a track, read what a song is doing to your body, or run the
phenomenology pipeline by name. The skill is a thin loader — it fetches
the current engine specs live from
[github.com/acooperrye/sonic-phenomenology](https://github.com/acooperrye/sonic-phenomenology)
on each session and applies them to whatever input you bring. Updates to
the dictionaries, baselines, or engine modules propagate automatically;
you don't need to reinstall the plugin to inherit them.

## Install

If you're already using a Cowork or Claude Code marketplace, add this
repo as a marketplace and install the plugin:

```
/plugin marketplace add acooperrye/sonic-phenomenology
/plugin install sonic-phenomenology@sonic-phenomenology
```

Then trigger it by saying something like "do a sonic-phenomenology read
of [track]" or "what is this song doing to me."

## What you can bring to a reading

- An audio file or recording link
- An artist and title
- Both
- Only your somatic response — "this hit my chest at the bridge"

The skill routes through the appropriate engines based on what you
provide. The pipeline is:

```
audio  ─▶ 1a Binary Engine ─┐
                            ▼
artist+title ─▶ 1b Web Engine ─▶ 2 Activation Layer ─▶ 3 Cultural Engine ─▶ 4 Interpretive Engine ─▶ Your body
```

Your felt response overrules computed inference where they disagree.
That's the load-bearing rule: never overwrite what was felt together
with what was computed alone.

## Network requirements

The skill calls `WebFetch` against `raw.githubusercontent.com` to pull
engine specs at runtime. Any Cowork or Claude Code session with default
network access will work. No GitHub account or auth needed — the repo is
public.

If the Web Engine path is in use, `WebSearch` is also called to retrieve
genre and production context for the artist+title input. Most Claude
products allow this by default.

## What it doesn't do

This plugin doesn't run the Python suppression-audio encoder (the part
of the framework that turns a genre's complete analysis into actual
stereo audio with FSK-encoded JSON metadata). That's a maintainer-side
tool. The plugin gives you access to the *analytical* surface — Claude
reading the engine specs and applying them to your track. The
audio-as-data piece lives at the source repo if you want to run it
yourself.

## Versioning

The plugin is pinned at version 0.1.1 in `plugin.json`. The engine
content the skill fetches is *not* pinned — it tracks `main` of the
source repo, so dictionary and baseline updates land in your sessions
without a plugin reinstall. To freeze a reading against a specific
version of the engines, edit the URLs in the skill's SKILL.md to
substitute a tag or commit SHA for `main`.

The framework versions the engines independently of the plugin. Breaking
protocol changes are governed by the protocol version in
`0-shared/shared-protocol.md` at the source repo.

## Project links

- Public brief: [atcooper.net/tools/sonic-phenomenology](https://atcooper.net/tools/sonic-phenomenology)
- Source repo: [github.com/acooperrye/sonic-phenomenology](https://github.com/acooperrye/sonic-phenomenology)

> *You feel. The engine reads. Convergence across independent axes is the signal.*
