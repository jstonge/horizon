---
name: vault-synthesis
description: Build a synthesis note in `synthesis/` that surveys what's in reading/ — a landscape across the vault's topic values, a note comparing two approaches, or everything on one theme pulled together. Use this whenever the user asks what the vault covers, wants a landscape or overview or state-of-play, asks to compare several things they've read about, or wants to pull scattered reading notes together into one place. Distinct from vault-note, which turns a single URL into a single reading note.
---

# Vault synthesis

A reading note answers "what does this source say". A synthesis note answers
"what's out there, and what is it". This builds the second from the first.

These land in `kb/synthesis/`, not the bundle root. The root is for the
user's own thinking — written, not compiled — and a generated survey sitting
there would blur the one distinction the layout exists to keep.

`vaultkit synth` assembles the skeleton; you write the prose. The tool can
group notes deterministically, but only you can read them and say what a thing
actually is.

## The loop

**1. See what you have.**

```sh
vaultkit index
```

The `BY Topic` block is the shape of the answer. A row covered by one
`repo` note is a thinner claim than one covered by `docs, repo, video` — say so
rather than presenting both with equal confidence.

**2. Scaffold.**

```sh
vaultkit synth --slug <slug> --model <your model id> \
    --description "One sentence: what the survey finds."
vaultkit synth --slug <slug> --topic a,b,c --model <your model id>
```

With no `--topic` it takes every value in the vault. Pass a subset when the
note is an argument about particular ones rather than a survey of all.

You get a `## By topic` section under a `> [!agent] <name> survey` banner:
one table row per value, with the name and `Covered by` filled in and the other
columns (set in `vault.toml` under `[synth]`) left blank. The note's
`generated:` records you as the author of the survey.

**3. Read the notes, then fill each row.** Open the reading notes listed under
`Covered by` — the links are a pointer, not a source. Each cell is one clause;
the distinctive-move column is the one thing that differs from the rest, which
is what makes a landscape read as contrasts rather than a list of blurbs.

When a row rests on specific detail — a number, a named mechanism — keep a
prose paragraph below the table rather than losing it to a cell.

**4. Say what the survey doesn't cover.** Name things you know exist but have
no reading note for, and rows whose coverage is thin.

**5. Run `vaultkit check`.**

## Attribution

Everything `vaultkit synth` generates sits under a `> [!agent] <name> survey`
banner, and anything you add to those sections stays under one. See
`AGENTS.md`. Never run `vaultkit verify` — that's the user saying they've
checked your survey.

## What stays yours

`## Thinking` is the user's, under their `### <name>` heading with dated
entries. Never write there. If the survey makes a conclusion obvious, put it in
the rows as observation, or say it in chat. Don't put it under their name.

## Keeping it current

Re-running `vaultkit synth --slug <slug> --force` rebuilds the table from the
current notes, which drops the cells you wrote. Prefer editing the note in
place when new reading notes land: add a row, append a link under `Covered by`.
Compare `vaultkit index` against the note to see what's drifted.
