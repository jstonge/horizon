---
name: vault-review
description: Review one of the user's atomic notes at the bundle root (`kb/`) and give hints about what's missing — whether it stands on its own, makes a claim, holds one idea, and links to the other notes and the reading it rests on. Read-only; the hints go in chat, never into the note. Usually invoked as `/vault-review <note>`. Also use it whenever the user asks whether a note is atomic or evergreen, what a note is missing, how to sharpen or improve one of their own notes, or for feedback on their thinking notes. Not for reading notes in reading/ — those are summaries, and vault-note makes them.
---

# Vault review

The bundle root (`kb/`) holds the user's atomic notes: one idea each, theirs entirely.
This skill reads one and says what it's missing, so they can fix it. It doesn't
fix it. Per `AGENTS.md`, agents never edit or create notes at the root — the
hints go in chat, and the user decides what to do with them.

That's also why the output is hints and questions, not rewrites. A suggested
sentence or a ready-made title is the user's argument written by someone else;
it's easy to accept and then it isn't theirs. Ask the question that would get
them to the claim, and leave the claim to them.

## Invocation

Called as `/vault-review [note]`. The note is a filename at the bundle root, with or
without `.md`, or a link to it. If none is given, run `vaultkit index`,
list the root notes with their link counts, and ask which one.

## The loop

**1. Read the note, then the vault around it.**

```sh
vaultkit index
```

The `YOUR NOTES` block gives this note's links in, links out and reading notes
cited, and the one-line description of every other note — root and reading. That's
enough to see what it could connect to without opening every file.

**2. Check it against the principles below.** Not all of them will have
something to say. Pick the ones that matter most for this note.

**3. Before suggesting a link, open the note you'd suggest.** A reading note's
gist is a pointer, not its content, and a link proposed from a gist alone can
send the user somewhere that doesn't hold up. Reading notes are short; read the
ones you'd point to. If you couldn't, say so.

**4. Reply in chat.** See **What to say** below.

## The principles

These come from Zettelkasten and Andy Matuschak's evergreen notes. Each one is
a question to put to the note.

- **Does it hold one idea?** Two claims that could each be linked to separately
  should be two notes. The opposite failure is more common early on: the note
  isn't yet *an* idea at all, only a topic or a definition. Say which.
- **Does it stand on its own?** Someone landing on it from a link has no
  surrounding context. Words like "another", "this", "as above" or "the second
  one" point at something that isn't on the page — often a sign the note was
  cut out of a longer document.
- **Does the title make a claim?** A title that states the idea can be linked
  to like a sentence: "grant discovery is now a search over private funders"
  rather than "grant-discovery". A topic title is fine for a structure note,
  which gathers others, but an atomic note gets its title from its claim. If
  there is no claim yet, the question to ask is what the user thinks, not what
  to call it.
- **Is it linked?** Zero links in means it will get lost; zero out means it
  doesn't build on anything. Point to the specific root notes it continues,
  answers or contradicts.
- **Does it rest on what's been read?** If the idea leans on something in
  `reading/`, the note should link it. The most useful hint this skill gives is
  often a reading note that supports or cuts against the idea — nothing else
  in the vault connects the user's thinking back to the reading.
- **Is it in their own words?** A note that mostly restates a source is a
  reading note in the wrong place.
- **Is the frontmatter there?** An empty `addedBy` matters: it's what says
  whose note it is. So does an empty `description:` — it's the line `index.md`
  shows, and for an atomic note it should be the claim in one sentence. If
  `description:` and the title say different things, ask which one the user
  means. Empty `topic:` or `tags:` matter less, and only once there are
  enough notes to group.

If the note mostly links to others, it's a structure note. Ask different
questions: does it say why these notes belong together, why in this order, and
what's still missing from the group?

## What to say

- Open with one line on where the note stands — usually whether it's atomic
  and what the main gap is.
- Then the hints, most important first, three or four at most. A long list
  reads as "rewrite it", and the point is to sharpen it.
- For each: what's missing, why it matters, and the question that would get
  them there.
- Name specific notes to link, as relative markdown links the user can paste
  (`[title](reading/slug.md)` from the bundle root), with one line on why each.
  Say how sure you are — whether you read the note or only its gist.
- Say what already works, briefly, if something does.

Don't put words in the note, don't propose its claim, don't create a structure
note for a cluster you notice. Point it out; the gathering is theirs.
