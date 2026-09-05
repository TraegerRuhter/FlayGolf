# Claude Code config

## Matt Pocock's skills (`mattpocock-skills`)

[Matt Pocock's "Skills for Real Engineers"](https://www.aihero.dev/skills) are enabled for this
repo through `settings.json`, which declares Anthropic's official plugin marketplace and turns the
plugin on for everyone who opens the repo (including Claude Code web sessions).

- Source: <https://github.com/mattpocock/skills> (listed in `claude-plugins-official`)
- Contains 25 skills, e.g. `/mattpocock-skills:grill-with-docs`, `:to-spec`, `:to-tickets`,
  `:implement`, `:tdd`, `:code-review`, `:triage`, `:handoff`
- Skills are namespaced by plugin name, so they never collide with this repo's own skills

### First-time setup

Run once per repo, inside Claude Code:

```
/setup-matt-pocock-skills
```

It asks which issue tracker to use (GitHub, Linear, or local files), which triage labels you use,
and where generated docs should be saved.

### Updates

Automatic. The official marketplace has auto-update enabled by default: Claude Code refreshes it
shortly after a session starts and pulls new plugin versions as Matt ships them. Claude Code either
prompts for `/reload-plugins` or loads the new version on the next launch. To force a check:

```
/plugin marketplace update claude-plugins-official
```

### Installing it outside this repo

To have the same skills in every repo on your machine, install it at user scope instead:

```bash
claude plugin install mattpocock-skills
```

Don't also copy the skills in with `npx skills add mattpocock/skills` — the plugin and the
file-copy installer are alternatives, and running both gives you every skill twice.

### Removing it

Delete the `enabledPlugins` entry from `settings.json`, or run
`/plugin uninstall mattpocock-skills@claude-plugins-official`.
