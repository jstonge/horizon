# Working conventions

## Mark what an agent wrote

Anything an agent produces in this vault sits in an `[!agent]` callout, so a
reader can tell at a glance whose words they're reading — orange, with a bot
icon, styled by `.obsidian/snippets/agent-callout.css`. The title names the
agent that actually wrote it: `> [!agent] Claude summary` on a reading note,
`> [!agent] Claude survey` as a banner on a generated section. `vaultkit` takes
the name from `$VAULT_AGENT`, defaulting to `Claude`, so set it once if you work
with something else:

```sh
export VAULT_AGENT=Codex
```

The rule is not the exact wording — it's that no unlabelled prose in this vault
is an agent's, and that the label doesn't claim the wrong one.

The frontmatter says the same thing in a form other tools can read. When an
agent writes a summary or a survey, `vaultkit` sets OKF's `generated: {by, at}`,
with `by` as `<agent>/<model>` — pass `--model <your model id>` so it names the
model and not only the agent. When a person has checked that text against the
source, they run `vaultkit verify <note>`, which appends `{by: human:<id>, at}`
to `verified`. That is the human's attestation: an agent never runs `verify`,
not even when asked to tidy up, and never edits `verified` by hand.
`vaultkit index` lists what an agent wrote that no one has verified since.

Two people are writing here, and an unattributed paragraph silently becomes
something "we think". If a section is assembled rather than argued, say so and
let it be argued with.

Sections under an `### <author name>` heading are that person's, always. Agents
never write there, and never flip `readStatus:`.

Atomic notes at the bundle root go further: one idea, one author, the whole
note theirs — so there's no heading, and `addedBy` says whose it is. Agents
don't edit them at all. If one should be split, retitled or linked, say so in
chat. Nor do they create notes at the root — structure notes included.
Deciding which notes belong together, and in what order, is the thinking; when
a cluster shows up in `vaultkit index`, point it out and leave the gathering to
them.

## This vault is an OKF bundle

`kb/` follows the [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2, so anything that reads OKF can read it without knowing about Obsidian.
What that asks of you:

- **Every `.md` in `kb/` has frontmatter with a `type:`.** The templates
  set it; a note made by hand without a template doesn't conform. Run
  `vaultkit check` after anything you write.
- **`description:` is one sentence** saying what the note claims, not what it's
  about. It's what `index.md` shows, so it's the first thing a reader or an
  agent meets. Pass it with `--description`.
- **`index.md` and `log.md` are reserved**, and `vaultkit` writes them. Don't
  edit them, and don't name a note `index` or `log`.
- **`resource:` is the URL a reading note is about**; `sources:` lists where its
  content came from — the archived copy first.
- **`status:` means OKF lifecycle** (`draft`, `stable`, `deprecated`). Whether
  someone has read a source is `readStatus:`.
- **Links in a note body are relative markdown links**, `[title](../reading/slug.md)`,
  which both Obsidian and OKF resolve. Obsidian is set to write them that way.
  Wikilinks stay only in frontmatter, where Obsidian needs them to be clickable.

## Say where a fact came from

The archives in `kb/sources/` are checkable — anyone can open the file
and see whether a summary is fair. A fact from a web search isn't, and it ages
differently: funding rounds, star counts and release dates go stale in a way
that an archived article doesn't.

So when a note carries background that isn't in `sources/`, say so in the block,
with the date it was looked up. It costs one line and tells a future reader
which claims to re-check.

## Tables for surveys

When a synthesis note surveys several things side by side, default to a table —
it's for scanning and comparing, which is what a landscape is for. Name the
columns in the note so they can be challenged.

Tables compress, and the evidence is usually what gets squeezed out. When a
claim is load-bearing — a benchmark number, a specific mechanism — keep a prose
section below the table rather than losing it to a cell.

## Where notes live

All of them inside `kb/`:

- the bundle root — your own thinking as atomic notes, written rather than compiled
- `synthesis/` — surveys compiled across the reading notes, made by `vaultkit synth`
- `reading/` — one note per source, made by `vaultkit note`
- `who/` — people and organisations: who they are and what they've made
- `sources/` — archived full text of what was read; nobody's words but the author's

`vaultkit note` routes on `--template`, so a new shape is a template file in
`my-templates/` plus a `[templates.<name>]` table in `vault.toml`. Entity notes
get no `### <author>` heading — they're shared reference, not anyone's take.
