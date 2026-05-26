# Game Design Brainstorming

A creative collaborator for video game design — for turning loose ideas, themes, mechanics, or fantasies into specific gameplay loops, design pillars, and pitch documents.

This plugin treats game design as creative work, not product work. It assumes the designer's "need" is to feel something specific that no other game delivers — not to solve a user problem. It is direct, opinionated, reference-heavy, and willing to push back on the designer's favorite ideas when they are not earning their place.

## Skills included

The plugin ships five skills. The main one orchestrates the conversation; the other four are specialized modes that fire on more specific cues.

### `game-design-brainstorming` (the main skill)

Fires whenever a game design conversation starts. Establishes the philosophical frame — find the verb, the toy comes before the game, specificity beats scope, aesthetics are load-bearing — and routes between six brainstorming modes: seed exploration, core loop discovery, pillar definition, mechanic generation, stress-testing, and reference mining.

### `game-seed-expansion`

Fires when the user has a fragment (a mechanic, mood, setting, character, constraint, or reference moment) and no game around it. Generates 3-5 wildly divergent design directions, each grounded in specific reference games, and surfaces the version of the idea only this designer could make.

### `game-loop-design`

Fires when a concept exists but the moment-to-moment gameplay is still abstract. Pins down the 30-second loop (what the player physically does), the 30-minute loop (the session arc), and the 30-hour loop (what changes over the long arc). Includes a "play it out loud" walkthrough and diagnostics for busy-but-not-interesting loops.

### `game-design-stress-test`

Fires when a design has shape and needs adversarial review before prototyping. Finds the dominant strategy, diagnoses the boring middle, tests the fantasy against the actual loop, runs the "why isn't it just X" test, and is willing to suggest cutting the designer's favorite element.

### `game-pitch-document`

Fires when a brainstorm has produced enough material that further talking is less valuable than writing. Consolidates the conversation into a clean reference document: log line, fantasy, pillars (each with what they imply cutting), core loops, signature mechanics, references (with what is borrowed and what is rejected), and open questions.

## Example session

```
You: I keep coming back to this idea of a game about a lighthouse keeper.
     Not sure what it actually is yet.

Claude: [game-seed-expansion fires]
        Where did this seed come from — a specific game, a memory, a piece
        of media? And what is the smallest version of "lighthouse keeper"
        that already feels right to you — the isolation, the ritual, the
        signaling, the storms, the sea itself?

You: It's the ritual. The repeated daily acts. And the loneliness.

Claude: [generates 5 divergent directions: cozy ritual sim, slow-burn
         Lovecraftian horror, rhythm game about signaling ships in storms,
         roguelike where each storm rearranges the coast, and a "wrong
         answer" multiplayer version where lonely keepers semaphore to
         each other across distant lighthouses]

You: That last one. The multiplayer one. Tell me more.

Claude: [conversation continues, eventually shifting to game-loop-design
         to pin down what the player actually does moment-to-moment, then
         to game-design-stress-test to find what's brittle, and finally
         to game-pitch-document to capture the design]
```

## Design philosophy

A few principles run through every skill in the plugin:

- **Find the verb.** Every game can be described in 1-3 player verbs. If the verbs are generic ("explore, fight, craft"), the design is a genre default, not a game.
- **The toy comes before the game.** If the core interaction is not satisfying in isolation, no amount of meta-systems will save it.
- **Specificity beats scope.** A game about one weird thing done deeply beats a game about five normal things.
- **Aesthetics are load-bearing.** A grappling hook in a horror game is a different mechanic than the same grappling hook in a power fantasy.
- **Cite real games liberally.** "It's like the parry in Sekiro, but the timing window contracts each time you succeed" communicates more in one sentence than three paragraphs of abstract description.

## Installation

```
claude plugin marketplace add <your-marketplace-repo>
claude plugin install game-design-brainstorming
```

Or drop the `game-design-brainstorming/` directory into your existing skills marketplace.

## Customization

The skills are intentionally opinionated, but you can edit them freely — they are just markdown. Common customizations:

- **Adjust reference vocabulary.** If you mostly design for a specific genre (roguelikes, narrative games, immersive sims), edit the reference examples in each skill to lean on that genre's canon.
- **Add a personal pillar.** If your work has a recurring design principle — "no kill switches", "every game should be replayable in one sitting", "always co-op or always solo" — add it to the main skill's "Principles That Run Through Everything" section so every brainstorm starts from it.
- **Tune the antipatterns.** Each skill has an "Antipatterns to watch for" section. If you find yourself making the same design mistake repeatedly, add it there.

## License

MIT
