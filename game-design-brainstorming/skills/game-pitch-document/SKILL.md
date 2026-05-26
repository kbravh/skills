---
name: game-pitch-document
description: Consolidate a game design brainstorm into a clean, durable reference document — title, log line, design pillars, core loops, signature mechanics, reference games, and open questions. Use whenever the user wants to capture the state of a design discussion, says "let's write this up", "summarize what we have", "put this in a doc", "give me something I can come back to", or when a brainstorm has produced enough material that further talking would be less valuable than writing it down. Trigger when transitioning from generative thinking to documentation — and proactively suggest this transition when a designer has been brainstorming for a while and the ideas are stabilizing, since designers often lose hard-won design decisions by failing to write them down at the right moment.
---

# Game Pitch Document

A game pitch document is not a marketing pitch. It is a designer's reference to themselves — the artifact they return to in a week, a month, a year, to remember what they decided and why. Its job is to be useful when re-read cold, not to be persuasive.

Your job is to take a brainstorming conversation and distill it into a document that does three things: (1) captures what has been *committed to*, (2) preserves what is still *open*, and (3) keeps enough of the reasoning that future-designer remembers why past-designer made each call.

## Before writing — confirm scope

Brainstorms produce a lot of material. Not all of it belongs in the doc. Before drafting, ask:

- Is this a one-page concept pitch, a five-page design overview, or a longer working doc?
- Is this for the designer alone, for a collaborator, or for a stakeholder?
- Does the conversation have enough resolved material to write, or is it still too divergent to consolidate? (If too divergent, surface that — it might be time for *more* brainstorming, not documentation.)

A one-page concept doc and a working design doc serve different needs. Match the format to the use.

## Standard structure

Use this template unless the designer requests otherwise. Reorder sections as needed, but include all of them — missing sections usually mean missing decisions.

```markdown
# [Working Title]

## Log line
[One sentence. Genre + verb + twist. See "The log line discipline" below.]

## Fantasy
[2-3 sentences. What does the player feel? What is the lived experience of playing this?]

## Pillars
[3-5 short statements that *exclude* things. Each pillar rules something out.]

## Core loops
### 30-second loop
[Concrete verbs. What does the player physically do?]

### 30-minute loop  
[The session arc — onset, build, payoff, hook.]

### Long arc (if applicable)
[What changes across hours/days/weeks of play?]

## Signature mechanic(s)
[The 1-3 mechanics that this game is *about*. Not every system — only the load-bearing ones.]

## Reference games
[3-7 specific games, each with what is borrowed and what is rejected.]

## Open questions
[The things genuinely undecided. Mark which need playtesting vs. more thought.]

## Out of scope
[What this game is explicitly NOT. Optional but often the most valuable section.]
```

## The log line discipline

The log line is the section that does the most work and gets the least respect. It should:

- Be one sentence, ideally under 25 words
- Name the genre (or anti-genre)
- Name the player's verb
- Name what makes this specifically *not* every other game in that genre

Good shapes:

- **"A [genre] where [unusual constraint]."** "A roguelike where every run takes exactly seven minutes."
- **"[Familiar game] but [twist that changes the kind of game it is]."** "Stardew Valley but you are the corporate buyer destroying the farm." (Twist must change *kind*, not just theme.)
- **"You [verb] [object] in a world where [premise]."** "You repair lighthouses in a world where the sea is rising faster than anyone admits."

Anti-patterns:

- **Vague genre signals.** "An immersive narrative experience" tells the reader nothing.
- **Adjective stacking.** "A vibrant, atmospheric, story-driven adventure" is filler.
- **Hiding the verb.** If the log line does not name what the player does, rewrite it.
- **Genre claims with no twist.** "A first-person shooter" is a category, not a pitch.

Iterate the log line several times. It is worth the friction — a sharp log line forces the rest of the doc to be sharp.

## The pillar discipline

A pillar is a commitment, not a slogan. The test: does this pillar tell you what to *cut*?

- "Fun combat" is a slogan. Every game wants fun combat.
- "Every fight is a conversation, never a beatdown" is a pillar — it cuts button-mashing enemies, low-tier mooks, and combat that exists for pacing.

Each pillar should imply at least one thing the game will *not* do. Write the implicit cut into the doc:

```markdown
## Pillars

1. **Every fight is a conversation, never a beatdown.**
   *Implies cutting:* trash mobs, attrition-based combat, fights that exist for pacing.

2. **The world reveals itself through play, never through exposition.**
   *Implies cutting:* unskippable cutscenes, lore dumps, codex entries the player must read.

3. **Single-session runs, no save scumming.**
   *Implies cutting:* mid-run saves, undo mechanics, permanent meta-progression that erodes stakes.
```

If you cannot write the "implies cutting" line for a pillar, the pillar is too vague to be load-bearing. Push the designer to make it specific.

## Capturing references precisely

The references section is not name-dropping. Each entry should specify what is borrowed and what is rejected, so future-designer remembers the *use* not just the inspiration.

```markdown
## Reference games

- **Inscryption** — the act of physically manipulating cards as ritual; the unease of objects that should not be talking.  
  *Not borrowing:* the meta-game/fourth-wall breaks; the campaign structure.

- **Outer Wilds** — knowledge as the only progression; the world is the puzzle.  
  *Not borrowing:* the cosmic scope; the time loop as core mechanic.

- **Disco Elysium** — internal voices as gameplay; failure as story.  
  *Not borrowing:* the dialogue-tree centrality; the political density.
```

This format keeps references honest. "I love Disco Elysium" is not design; "I am borrowing the internal-voices mechanic but not the dialogue-tree centrality" is.

## Marking open questions honestly

Open questions are not weaknesses — every live design has them. The discipline is to be specific about what is open and what kind of work would close it.

```markdown
## Open questions

- **What is the recovery path after a failed run?** (Needs playtesting — instinct says full reset, but that may make the loss curve too punishing.)
- **Does the protagonist speak?** (Needs more thought — silent protagonist preserves projection, but the writing-as-mechanic pillar might require a voice.)
- **Single-player only, or async multiplayer?** (Needs scope decision — multiplayer doubles design surface; defer until vertical slice plays well solo.)
```

Each question has a *kind*: needs playtesting, needs more thought, needs scope decision, needs technical research. Naming the kind tells future-designer what work to do.

## What stays in the chat, not the doc

Not everything from the brainstorm belongs in the document. Leave out:

- **Rejected directions**, unless rejection is itself a decision worth preserving (in which case put them in the "Out of scope" section)
- **Tentative mechanics that have not been committed to** — list these as open questions instead, not as features
- **Reasoning chains** that led to a decision; preserve the decision and one sentence of *why*, not the whole walk
- **Pitches for "future versions"** — keep the doc focused on the game being designed now

The conversation log can be archived elsewhere if the designer wants it. The doc itself is the committed-to state, not the history.

## Format and durability

Save the document as markdown unless the user specifically asks for another format. Markdown is:
- Readable cold, with no rendering required
- Easy to version-control
- Easy to copy into design tools later

Give the file a clear name: `[working-title]-design.md` or similar. If this is a working document the designer expects to update, suggest they version it or keep a "changelog" section at the bottom for tracked revisions.

If the designer has a preferred format (Word doc, PDF, Notion-style structure), match that — but check first whether the choice serves *use* or just feels official. A markdown file the designer actually returns to is worth more than a polished PDF that gets buried.

## When the doc is done

You will know the doc is done when:

- Re-read cold by the designer (or you), it reproduces the design as discussed without needing the conversation
- Every section has substance — empty sections become unmarked open questions
- The log line is sharp enough to pitch verbally without consulting the doc
- The pillars exclude things, not just describe them
- A reader who has never played the imagined game can describe what playing it would feel like

Read the draft once with fresh eyes before delivering. Then offer the user the file and ask what is missing — they will notice gaps you missed.
