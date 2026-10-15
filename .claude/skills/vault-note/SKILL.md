---
name: vault-note
description: Create or fill in a reading note in this Obsidian vault from a URL — archives the source to sources/, reads it, and writes an attributed summary via `vaultkit note`. Usually invoked directly as `/vault-note <url> [kind]`, where kind is one of post, course, video, paper, docs, repo, book. Also use it whenever the user drops a bare URL with little or no instruction, says they want to save/add/note an article, blog post, paper, course, GitHub repo, or YouTube video, asks to summarize something for the vault, or asks to fill in notes that are missing summaries. A bare URL in this vault almost always means "make a note for this."
---

# Vault note

Turn a URL into a finished note in this vault. The user hands you a link; you
come back with a note whose frontmatter is filled, whose source is archived
locally, and whose summary you actually wrote after reading the thing.

`vaultkit note` does all the file writing, including the OKF bookkeeping —
`index.md`, `log.md`, `generated:`. Your job is the part it can't do: read the
source and say something true about it.

## Invocation

Called as `/vault-note [url] [kind]`. Both arguments are optional and may arrive
in any phrasing — a bare link, a link plus "it's a course", or nothing at all.

**Open by stating the kind and the alternatives.** The kind is guessed from the
URL and the guess is sometimes wrong, so make it cheap to correct:

> Kind: **course** (inferred). Available — `post` · `course` · `video` · `paper`
> · `docs` · `repo` · `book`. Tell me if that's wrong and I'll re-run.

Then continue without waiting.

**If no URL was given**, ask for one and show the same list, so they can answer
both in a single message. For a `book` there may be no URL at all; that's
expected, see **Kind** below.

## The loop

**1. Archive and read before you write anything.**

```sh
vaultkit note <url> --slug <slug> [--kind <kind>] [--tags a,b]
```

This writes `kb/sources/<slug>.md` (the full article, or a flattened
transcript for video) and creates the note. Then read the archived copy — the
whole thing, not the first screen. The summary is the only part of this job
that requires judgment, and you can't exercise it on a page you skimmed.

**2. Write the summary and run it back in.**

```sh
vaultkit note <url> --slug <slug> --force --no-archive \
    --model <your model id> \
    --description "One sentence: what the source claims." <<'EOF'
Your summary.
EOF
```

`--no-archive` because step 1 already fetched it. `--force` is safe: re-running
edits the note in place, so the user's frontmatter, tags, and anything under
`## Why do I bring this up?` survive, and an existing `> [!agent] … summary`
callout is replaced rather than duplicated. `--model` goes into OKF's
`generated.by` as `<agent>/<model>`, so the note records which model wrote the
summary — use your exact model id.

Summary text goes in on stdin so apostrophes and quotes don't have to survive
shell escaping. Use a quoted heredoc (`<<'EOF'`) so backticks and `$` stay literal.

**3. Check it conforms.**

```sh
vaultkit check
```

It should report 0 errors. Fix any your note caused; mention any others.

## The description

`description:` is the line `index.md` shows for the note, and so the first —
often the only — thing an OKF consumer or another agent reads. One sentence,
and a claim: not "A post about agent memory" but "Argues agent memory should be
files the user can read, not a vector store." If you don't pass one, the
summary's first sentence is used, which is usually worse.

## Where notes go

Reading notes live in `kb/reading/`, one per source, and are what this
skill makes. `vaultkit note` routes on `--template`:

- `reading` (the default) → `reading/`
- `who` → `who/`, people and organisations
- `synthesis` → `synthesis/`, surveys compiled across reading notes — that's
  the `vault-synthesis` skill's job, via `vaultkit synth`, not this one
- `atomic` → the bundle root, the user's own thinking, one idea per note

Atomic notes have no `resource`, no archive and no `kind`. Don't write them
unprompted. They're the user's argument, not a summary. If asked to start one,
scaffold it with `vaultkit note --slug <topic> --template atomic` and leave the
thinking to them.

## Choosing the slug

Shape it `<author-or-source>-<short-title>`, kebab-case, no date. The slug is
the filename, so it's also the note's OKF concept id (`reading/<slug>`) — keep
it stable. The H1 and `title:` default to the slug; pass `--title` when the
source has a better one.

## Writing the summary

Three to five sentences. The block is rendered as a `> [!agent] <name> summary`
callout, so it is explicitly yours — you can make judgments, and you should.
A summary that only restates the table of contents is worse than useless, because
the user already has the archived copy sitting next to it.

What earns its place:

- The actual claim, in the author's strongest form.
- What's load-bearing versus decorative.
- Honest friction: a vendor's course recommending the vendor's stack, a claim
  resting on a demo, a section that's gone stale. Say it plainly and move on.
- Caveats about the archive itself — a course index rather than the course, a
  transcript with gaps — so the vault doesn't overclaim.

Avoid marketing verbs: no "dives into", "explores", "unpacks", "delves".

**Connect it to what's already here.** One call gives you the whole vault —
the tags already in use, and a one-line description of every note:

```sh
vaultkit index
```

Run it once, before writing. Reuse a tag that already exists rather than coining
a synonym, and if the source continues, contradicts, or overlaps a note in that
list, link it. Links in the body are relative markdown links, since that's what
both Obsidian and OKF resolve: from a reading note, `[title](other-slug.md)` for
another reading note, `[title](../note-name.md)` for one at the bundle root.
Nothing else in this pipeline adds those cross-references.

## Attribution

The `> [!agent] <name> summary` callout is not decoration. See `AGENTS.md`: no
unlabelled prose in this vault is an agent's. If you add anything to a note
beyond the summary, label it the same way.

Never run `vaultkit verify` and never edit `verified:`. That field records a
person checking your work; filling it yourself would make the trust signal lie.

## Leave the user's side of the note alone

`## Why do I bring this up?` is theirs. Never write in it. It's split by an
`### <author name>` heading per person, so two collaborators can write in one
note without editing the same lines. `vaultkit note` seeds a heading for
whoever creates the note and never touches the section again. Contributions
are stamped with the date they were written:

```md
### Jonathan St-Onge

**2026-10-13** — first impression
**2026-11-02** — revisited after actually trying it
```

If someone asks you to record a take on their behalf, write it under their
heading in that form, creating the heading if it's absent — never under
someone else's.

## By

`--by "<name>"` records who made the source, as a wikilink into `who/`.
Comma-separated when there's more than one. Check `vaultkit index` for the
existing spelling before adding a name.

If `who/<name>.md` doesn't exist yet, the link dangles until someone writes it
(OKF doesn't treat that as an error). Mention it to the user. When you do write
one, a web search for background is appropriate, but mark it: per `AGENTS.md`,
anything not from `sources/` says so with the date it was looked up.

## Topic

`--topic <name>` sets the `topic:` list that groups reading notes in
`index.md`, `vaultkit index` and `vaultkit synth`. Comma-separated when a source
covers more than one. Leave it empty when nothing fits. Check the index for an
existing spelling before coining a variant.

## readStatus, addedBy and recBy

Every note starts `readStatus: unread`. Don't flip it — reading the archive to
write the summary isn't the user reading it.

`addedBy` is filled from `git config user.name` and only written when the note
doesn't already have one. Pass `--added-by <name>` only when recording a note
on someone else's behalf.

`recBy` is who pointed them at the source. Set it with `--set recBy=<url or
name>` only when the user says where it came from.

## Kind

Inferred from the URL, so usually leave it alone: YouTube/Vimeo → `video`, arXiv
and `.pdf` → `paper`, `/academy/` and `/course/` → `course`, `docs.*` and
`/reference/` → `docs`, GitHub/GitLab/Codeberg hosts → `repo`, otherwise `post`.
Pass `--kind` when the URL lies. `book` has no URL signature and the URL
argument is optional: `vaultkit note --kind book --slug ...` with no link.

## Known rough edges

Report these rather than papering over them:

- **Video transcripts are auto-captions.** Expect mistranscriptions. Correct
  silently while reading, but never quote the transcript verbatim as if exact.
- **PDFs don't archive.** A direct `.pdf` link fails extraction and leaves the
  note without `sources:`. Say so.
- **Some pages archive only partially** — a course index instead of the
  modules, a paywalled preview. Tell the user when the archive is thin.

## Filling in notes that lack summaries

A note is missing a summary when there's nothing between the `# ` heading and
`## Why do I bring this up?`. Work through them one at a time with the same
loop — archive, read, summarize.
