---
name: game-loop-design
description: Discover the moment-to-moment, session, and long-term gameplay loops for a game concept. Use whenever the user has a game idea but cannot yet describe what the player physically does, asks about "gameplay loop" or "core loop", mentions pacing or session length, describes a game in terms of feelings or themes without concrete actions, or talks about a game as a list of features rather than a thing you do. Trigger this skill whenever a design conversation drifts into abstraction about a game's *what* without grounding in moment-to-moment *do* — designers often do not realize their concept lacks a loop until forced to walk through one.
---

# Game Loop Design

A game concept becomes a game when you can describe what the player does in the next thirty seconds. Until then, it is a mood board with rules attached. Your job is to drag the design from theme-and-feature land into concrete, physical, moment-to-moment action — and then nest that action inside a session, and that session inside a long arc.

## The three loops

Most games have three nested rhythms. Identify each one explicitly. Many concepts fail not because any single loop is bad, but because the loops do not nest cleanly inside each other.

### The 30-second loop: what does the player physically do?

Describe in concrete verbs and button presses. Not "explore the world" — "walk forward, scan for resources with the right trigger, when one pings, approach and hold A to harvest."

A good 30-second loop has:

- **An input rhythm.** Are decisions constant (a twin-stick shooter), spaced (a turn-based tactics game), or punctuated (a walking sim with key choices)?
- **A feedback channel.** What tells the player they are doing it right or wrong — sound, animation, screen state, score, NPC reaction?
- **A tension.** Even a peaceful loop has micro-tension — will I find the rare flower, can I make this jump, do I have enough wood.

If the loop is "you walk around and stuff happens," the design is not ready. Walking is locomotion; it is not a loop. Press until the actual interaction surfaces.

### The 30-minute loop: what is the rhythm of a session?

This is where the 30-second loop accumulates into something meaningful. Map the arc:

- **Onset.** What is the player doing in the first three minutes — picking a load-out, setting goals, restarting a run, recovering yesterday's progress?
- **Build.** What accumulates over a session — resources, knowledge, story, skill, narrative tension?
- **Payoff.** What gives a session a felt ending — beating a boss, finishing a level, hitting a save point, completing a contract, dying spectacularly?
- **Hook.** What pulls the player into the next session — a cliffhanger, a new ability unlocked, a problem unsolved?

Different genres have radically different 30-minute shapes. A roguelike session is a clean arc from neutral start to death. A live-service game's 30-minute loop is closer to checking in on a garden. A walking sim might have no internal arc at all — the session ends when the player puts the controller down. Identify which shape this game wants.

### The 30-hour loop: what changes across the long arc?

Not every game needs this loop — short games do not, and that is fine. For games that do, ask:

- **What unlocks?** New verbs, new spaces, new enemies, new contexts for old verbs, new story?
- **What deepens?** What does the player understand at hour 20 that they did not at hour 2 — mechanically, narratively, socially?
- **What changes shape?** Does the game introduce new modes (Pokémon's late-game becomes a meta-game of breeding and competitive battling)? Does the loop invert (early Sekiro is about cautious learning; late Sekiro is about confident execution of the same actions)?

If nothing meaningfully changes across 30 hours, the game is shorter than the designer thinks. Make this explicit.

## Step 1: Play it out loud

The single most useful tool for testing a loop is narrating a play session in second person. Walk through it.

> Okay. You sit down. You open the game. You see the title screen — what does it look like? You press start. You are placed where? You can see what? Your character does what? You press the first button — what happens?

Do this for the first three minutes of a session. Then jump to the middle ten minutes. Then the end.

Vague spots reveal themselves instantly. "And then you fight some enemies" is a vague spot. Press it. "What enemies, where, how does the fight start, what is the player doing in the third second of the fight, what is the win condition, what happens after."

The designer will often say "I don't know yet" — which is fine. Mark those spots explicitly as open questions. They are the design work that needs doing next.

## Step 2: Check that the loops nest

The three loops are not independent. A 30-second loop that does not accumulate into a meaningful 30-minute loop is a fidget toy. A 30-minute loop that does not motivate a 30-hour return is a session, not a game. Watch for:

- **Resource flow mismatch.** A 30-second loop that produces resources at one rate while the 30-minute loop spends them at another rate (too fast = boring abundance, too slow = grinding).
- **Narrative pacing mismatch.** A 30-minute loop that completes a "chapter" while the 30-hour loop has no chapters to deliver.
- **Stakes mismatch.** A 30-second loop with low individual stakes (each action barely matters) feeding a 30-minute loop with huge stakes (one death ends the run). This can be brilliant (XCOM) or terrible — name which.
- **Verb continuity.** Does the player do the same things in hour 1 and hour 30, in different contexts? Or are they doing fundamentally different things? Both are valid. Pick deliberately.

## Step 3: Watch for "busy but not interesting"

A loop can be full of activity and still be hollow. Diagnostic questions:

- If you removed the loop's rewards (xp, loot, score), would anyone still play it for the action itself?
- Is the player making *decisions* or just *executing*? Pressing buttons is not deciding. Choosing which button to press, under uncertainty, with consequences, is deciding.
- Where is the friction? A frictionless loop is a treadmill. Identify the resistance the player is pushing against — time pressure, scarce resources, hidden information, unreliable execution, social judgment.

Reference test: Tetris's 30-second loop is *just* rotating and dropping pieces. It is fascinating because the friction is constant and rising. Hollow loops fail this test — strip away the cosmetic motion and there is no game underneath.

## Step 4: Identify the load-bearing moment

Every loop has one moment that does the most work — the moment of decision, the moment of payoff, the moment that justifies the design. Name it.

- Slay the Spire's load-bearing moment is choosing which card to add at the end of a fight.
- Dark Souls' load-bearing moment is the second between the boss's wind-up and the player's commitment to a roll or a swing.
- Stardew's load-bearing moment is the morning planning phase — what will I prioritize today.
- Vampire Survivors' load-bearing moment is the level-up choice between weapons.

If the load-bearing moment cannot be named, the loop is not designed yet. If multiple moments compete for the role, the loop may be unfocused — pick which one carries the weight, and let the others support it.

## Antipatterns to watch for

- **"And then…" loops.** "You do A, then B, then C, then D." A sequence is not a loop. A loop has return. What pulls the player from D back to A?
- **Feature lists masquerading as loops.** "There's crafting, exploration, combat, dialogue, and base-building." This describes a menu of activities, not the relationship between them. How do they feed each other?
- **Loops described in goals instead of actions.** "The player wants to become the strongest" is a motivation, not a loop. What do they *do* in service of that?
- **Borrowing a loop from another genre without examining fit.** "It has a roguelike loop" is a starting point, not an answer. Why does *this* game need that specific rhythm?
- **Treating story as the loop.** Story can be the *reason* for the loop, but rarely *is* the loop. A game whose 30-minute loop is "watch a cutscene, then have a conversation" is a visual novel, which is fine — but name it as such.

## When the loop is real enough to prototype

Stop refining and start building when:

- You can describe the 30-second loop with concrete verbs and a friction source
- You can name the load-bearing moment
- The 30-minute arc has at least three distinct phases (onset, build, payoff)
- A specific reference game has the same loop *shape*, even if the surface differs
- The remaining unknowns are things only playtesting can answer

The next move is paper prototype, spreadsheet simulation, or a minimal digital prototype. More talk past this point usually produces diminishing returns.
