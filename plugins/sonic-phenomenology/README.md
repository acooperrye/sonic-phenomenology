# Sonic Phenomenology — Plugin Release v0.1.4

A Claude plugin that lets you run a phenomenological reading of any piece of
music. Audio in one ear, artist-and-title context in the other, cultural
conventions as a third independent axis, your felt response as the final
ground truth. Convergence across independent axes is the signal.

## Install (this archive)

This zip is a complete Claude plugin. To install:

- **In Cowork** — open Plugins → Personal plugins → "+" → upload this
  `.plugin` file. Cowork extracts it and registers the skill automatically.
- **In Claude Code** — drop the extracted folder into your plugins directory,
  or use `/plugin install` against the extracted path.

After install, trigger it in any session with:
- *"do a sonic-phenomenology read of [track]"*
- *"what is this song doing to me"*
- *"run a phenomenological analysis on [artist — title]"*

## Install (recommended — marketplace, auto-updates)

For automatic version tracking, add the source repo as a plugin marketplace
instead:

```
/plugin marketplace add acooperrye/sonic-phenomenology
/plugin install sonic-phenomenology@sonic-phenomenology
```

The skill live-fetches the engine specs from the repo's `main` branch on
each session, so dictionary and baseline updates propagate automatically.

## What's in this zip

Standard Claude-plugin layout:

```
sonic-phenomenology-0.1.4/
├── .claude-plugin/plugin.json        ← plugin manifest
├── README.md                         ← this file
└── skills/
    └── sonic-phenomenology/
        └── SKILL.md                  ← the skill — read this to see what it does
```

The heart of the plugin is `skills/sonic-phenomenology/SKILL.md`. Open it
directly if you want to see how the framework is invoked before installing.

## What the plugin does

You bring one of: an audio file, an artist+title, both, or just your felt
response. The skill routes through the appropriate stages of the framework:

```
audio  ─▶ 1a Binary Engine ─┐
                            ▼
artist+title ─▶ 1b Web Engine ─▶ 2 Activation Layer ─▶ 3 Cultural Engine ─▶ 4 Interpretive Engine ─▶ Your body
```

Your felt response overrules computed inference where they disagree. That's
the framework's load-bearing rule: never overwrite what was felt together
with what was computed alone.

## Network

The skill issues HTTP GETs against `raw.githubusercontent.com` for engine
specs at runtime, and uses web search for the Web Engine path. The fetch
tool name depends on the runtime: `mcp__workspace__web_fetch` in Cowork,
`WebFetch` in Claude Code CLI. SKILL.md's Step 0 picks the right one, and
Step 3 lists every sub-spec URL in full so the runtime's URL-provenance
filter doesn't block them. Any session with default network access works —
no GitHub account or auth needed, the repo is public.

## Changelog

- **0.1.4** — SKILL.md Step 0 added so the skill picks the right HTTP fetch
  tool per runtime (Cowork's `mcp__workspace__web_fetch` vs. Claude Code's
  `WebFetch`). Step 3 sub-spec table converted to fully-qualified URLs so
  runtimes with URL-provenance filtering (e.g. Cowork) can fetch each
  engine spec without prior mention in conversation. Fixes silent
  failure-to-orient that hit Cowork sessions in 0.1.3.
- **0.1.3** — initial public release.

## Links

- Source repo and full framework: [github.com/acooperrye/sonic-phenomenology](https://github.com/acooperrye/sonic-phenomenology)
- Public brief: [atcooper.net/tools/sonic-phenomenology](https://atcooper.net/tools/sonic-phenomenology)

## License

MIT.
