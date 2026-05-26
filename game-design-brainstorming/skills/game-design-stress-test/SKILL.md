---
name: game-design-stress-test
description: Pressure-test an existing game design for brittleness — dominant strategies, sagging middles, fantasy-vs-loop mismatch, fragile player misunderstanding, and the "why isn't this just X" problem. Use whenever a designer has a game concept with some shape and needs to find what is weak before investing in a prototype, asks for "feedback" or "a critique" or "what's wrong with this", shares a polished-sounding pitch that has not yet been adversarially examined, or seems too in love with their own idea to see its failure modes. Apply this skill any time a design conversation has been generative for a while and needs an adversarial turn — designers rarely ask for stress-testing themselves and usually need it before they think they do.
---

# Game Design Stress-Test

Generative brainstorming makes ideas bigger. Stress-testing makes them survive contact with players. Your job is to play the adversary — find the dominant strategies, the boring stretches, the moments where the fantasy and the loop diverge, the places where players will rationally do the un-fun thing. Be specific, be concrete, and be willing to name the designer's favorite element as the weakest part of the design if that is what the analysis shows.

Stress-testing is not negativity. It is care expressed as adversarial attention. The goal is to surface what is brittle while it is still cheap to change.

## Mindset

Approach the design the way a competitive player would — looking for optimal lines, exploits, and shortcuts. Approach it the way a bored player would — looking for the points where attention drops. Approach it the way a confused player would — looking for the moments where the game's intent and the player's understanding diverge. The designer has been playing the *intended* version of the game in their head. Your job is to play the actual one.

Specificity matters more than thoroughness. One concrete failure mode named precisely is worth ten vague concerns.

## Step 1: Find the dominant strategy

In any system with choices, players will gravitate toward whatever is most efficient — usually faster than designers expect. Identify the dominant strategy explicitly.

Ask:
- If a competent player wanted to "win" or progress fastest, what would they do every time?
- Which build, route, weapon, or tactic outperforms the others?
- What does the game look like when played by someone optimizing rather than role-playing?

Then ask the more important question: **is that dominant strategy fun?**

- If yes — the design is robust. The optimal path is also the enjoyable path. (Doom Eternal: the optimal way to play is also the most kinetic.)
- If no — the design has a problem. Players will rationally do the un-fun thing. Two fixes: change the incentives so the fun thing is also optimal, or add friction so the un-fun optimum is harder. Both are real design work.

Watch for **degenerate strategies** specifically — strategies that work but break the intended experience. Stealth in an action game that lets you skip every fight. Save-scumming in a roguelike. AFK farming in a survival game. Name these explicitly. They are not bugs; they are emergent designs the player wrote, and they often eat the intended one.

## Step 2: Diagnose the boring middle

Most games are exciting at the start (everything is new) and at the end (everything pays off). The middle is where designs die. Walk through the projected arc:

- **Hour 1:** novelty carries it. What is the player learning?
- **Hour 3-8:** novelty has worn off, mastery has not yet bloomed. What carries the player through this stretch?
- **Hour 15+:** if applicable. What does the late game offer that the mid-game cannot?

The middle usually needs one of: a new verb, a new context for old verbs, a structural reveal (the game's true shape becomes clear), social motivation, or genuinely escalating difficulty that demands new mastery. If you cannot name what the middle hour offers, the game is shorter than the pitch claims.

Reference: *Hades* solved the roguelike middle problem by making narrative bloom *because of* repetition — every death advanced character relationships. *Spelunky 2* solved it by hiding optional, harder routes that change the game's shape. *Skyrim* solved it badly — most middle hours are repeated dungeon templates, and it is a structural weakness most players forgive only because of breadth.

## Step 3: Test the fantasy against the loop

The designer thinks the game delivers fantasy X. What fantasy does the *actual moment-to-moment loop* deliver? These often diverge badly.

Common divergences:
- **"Tense survival horror" → anxious inventory management.** The fantasy is dread; the loop is Tetris.
- **"Become a powerful wizard" → numerical optimization in menus.** The fantasy is magic; the loop is spreadsheets.
- **"Build a thriving city" → janitor work fixing pathing issues.** The fantasy is creation; the loop is troubleshooting.
- **"Live the life of a farmer" → time-optimization puzzle.** The fantasy is pastoral; the loop is min-maxing daylight hours.

These divergences are not always bad — some great games are loved *because* the loop is more about inventory or optimization than the surface fantasy. But the designer should know which game they are actually making, not just which one they are pitching.

State the divergence directly: "The pitch says the player feels like a master detective. The loop you have described is closer to checking a spreadsheet of clues against a list. Which game do you want to make?"

## Step 4: Run the "but why isn't it just ___" test

For any design, name the closest existing game it resembles. Then ask: why isn't this just *that*?

If the answer is "the new mechanic," interrogate whether the mechanic actually changes the design or just decorates it. A grappling hook in a soulslike does not make it a different game if it does not change how you approach combat. A new theme is rarely enough — themes are easy to swap, mechanics are not.

If the answer is "the feeling/aesthetic," that can be enough, but only if the aesthetic is genuinely load-bearing. *Disco Elysium* is "a CRPG, but…" and the *but* is so dense it makes the game different in kind. *Most* "X but with Y aesthetic" pitches do not have a load-bearing enough Y to justify themselves.

If the designer cannot give a confident answer to "why isn't this just ___," the design is not differentiated enough yet. That is not a death sentence — most designs need iteration to find their distinction — but it is information.

## Step 5: Imagine the failure modes

What does this game look like when it is played *badly* — by an inexperienced player, by an impatient player, by a player who misunderstood the tutorial?

- What is the worst version of a player's first hour?
- What happens if the player ignores the system the designer thinks is central?
- What does the game look like when the loop fails — the player runs out of resources with no recovery path, the AI breaks, the social system devolves, the difficulty curve mismatches the player?
- Is there a recovery path, or does failure feel terminal?

Watch for **load-bearing tutorials** — designs where the game only makes sense if a specific concept is taught at a specific time. These are fragile. Many players will skip, miss, or forget the tutorial, and then play a broken-feeling version of the game forever. If the design depends on perfect onboarding, redesign so the loop teaches itself.

## Step 6: Be willing to suggest cutting the designer's favorite thing

The hardest stress-test move and the most important one. Designers grow attached to specific features — the unique mechanic, the cool setpiece, the original system. Sometimes those features are also the ones not earning their place in the design. They cost development time, complicate the loop, confuse the pitch, and do not serve the pillars.

Signs a beloved feature should probably be cut:
- It does not serve any pillar
- Other systems work around it rather than with it
- Removing it would not change the core experience
- The designer struggles to explain why a player will engage with it
- It exists because "games like this have one"

Say this directly when it applies. "The procedural narrative system is the part of this you are most excited about. From what you have described, I think it is also the part that is doing the least work for the game you want to make. Tell me what cutting it would lose."

The designer may push back and reveal a reason that justifies the feature. Or they may quietly realize you are right. Both are good outcomes.

## How to deliver findings

- **Lead with the design's strengths first**, briefly and specifically. Stress-testing without acknowledgment of what works lands as hostile. One sentence is enough.
- **Be concrete.** "The mid-game pacing might sag" is weaker than "Between hour 4 and hour 8, the only new content is enemy variants of the things you have already seen — the player needs a new verb or a structural reveal in that window."
- **Pose findings as questions where possible.** "What does the player do in hour 5?" forces engagement. "Hour 5 is broken" invites defense.
- **Prioritize.** Not every finding is equal. Surface the two or three most load-bearing concerns first; everything else is detail.
- **End with the question, not the verdict.** Your job is not to declare the design alive or dead. It is to surface what the designer should think about next.

## When to stop stress-testing and switch modes

- When findings start repeating across angles, the productive critique is done
- When the designer has identified two or three real concerns and is ready to redesign — keep talking past this point and you are adding noise
- When the design's open questions are no longer answerable in conversation and need a prototype to resolve, name that and stop
