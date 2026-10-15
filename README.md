# horizon

## How to get started

Open this folder as a vault in [Obsidian](https://obsidian.md), then enable
**Settings → Appearance → CSS snippets → `agent-callout`**. It tints
agent-written blocks orange, so you can tell whose words are whose.

The notes are written with [vaultkit](https://github.com/jstonge/vaultkit):

```sh
uv tool install git+https://github.com/jstonge/vaultkit
```

### Add what you read

Hand your agent the link:

```
/vault-note https://example.com/some-post
```

It archives the page to `kb/sources/`, makes a note in
`kb/reading/`, and drafts a summary marked as the agent's.
`## Why do I bring this up?` is left for you, under your own name. Once you've
checked the summary against the source, record it:

```sh
vaultkit verify reading/some-post
```

Once there are a few notes, `/vault-synthesis` surveys across them.

### Write your own notes

Your own thinking goes at the root of `kb/` as atomic notes — one idea
each, linked to each other and to the reading. Make a new note, run
**Templates: Insert template** (Cmd+P) → `atomic`, and fill in `description:`
with the claim in one sentence. For hints on what a note is missing:

```
/vault-review <note>
```

It answers in chat and never edits the note.

### Where things live

| | |
| --- | --- |
| `kb/` | the knowledge itself, an [OKF](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) bundle |
| `kb/` root | our own thinking, one atomic note per idea |
| `kb/reading/` | one note per thing read |
| `kb/synthesis/` | surveys compiled across the reading notes |
| `kb/who/` | the people and organisations behind them |
| `kb/sources/` | archived full text, so nothing depends on a live URL |
| `my-templates/` | note templates, shared by `vaultkit` and Obsidian's Templates plugin |
| `vault.toml` | which folders and fields this vault uses |

`index.md` and `log.md` in the bundle are written by `vaultkit`; don't edit
them. `vaultkit check` says whether the bundle conforms to OKF.

The conventions agents follow are in [AGENTS.md](AGENTS.md).
