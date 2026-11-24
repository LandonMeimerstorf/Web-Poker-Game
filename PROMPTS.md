# Prompt log

How this project evolved, one prompt at a time. Same approach as the FSB logo: I wrote what I wanted, looked at what came back, and described what I saw to steer the next step.

## 1. The scene (poker-scene)

> Imagine you're in the seat at a high stakes poker table and you are "peeking" at your cards sitting right in front of you. The table has chips sitting in a sizable stack in the middle of the table in the distance. When you move your cursor to the cards you are able to drag them up and down to reveal or hide your cards. There are other visible players sitting around the table but no faces can be seen... The cards you're revealing are the Ace and King of Spades.

**Result:** a realistic 3D table with six faceless players and a dealer, all moving, and cards you drag up to peek at.
**Lesson:** "no faces" got solved with framing. The camera's top edge sits just above eye level, so heads are never in shot. Describing what the viewer should never see is as useful as describing what they should.

## 2. Card assets in ASCII (ascii-cards)

> I want to start generating ascii art assets we can use for this project, can we first start with generating a render for what the cards will look like in ascii art form using three.js?

**Result:** A♠ and K♠ on a turntable, rendered through the same ASCII pipeline as the FSB logo.
**Lesson:** ASCII eats detail. The card art had to be redrawn bold (huge index letters, a big spade, thick lines) and the lighting kept low so the white faces didn't blow out into solid `@`.

## 3. Chips that don't look like wafers (ascii-chips)

> I want white, black, green, and red poker chips. Generic design but they need to differentiate themselves from regular wafers, the viewer needs to know that they are poker chips.

**Result:** chips with 8 edge spots that wrap over the rim, a value inlay, molded ticks and real thickness.
**Lesson:** naming the failure ("not wafers") pushed the build toward the features that make something recognizable. The black chip turned out to be the clearest, because its white spots read as bright characters on empty space.

## 4. Table and a pile of chips

> Generate a round poker table with dark green, olive, felt covering the top of it, nothing else, keep it simple. Add the chips to the center of the table, they look scattered yet collected. A sizable pile of chips.

**Result:** 320 chips in a messy heap covering most of the table.
**Lesson:** "scattered yet collected" and "sizable" are feel words, so the AI's taste filled in the rest. I didn't like it, which led to the next prompt.

## 5. Counts and a camera position

> Build 5 large stacks that vary in size and shape, 1 stack is small and only has a few chips, the stack sitting next to it has more, etc... make more room for other things to go on the table. Use a 16mm lens as your reference. Imagine there's a camera sitting flat on the edge of the table looking toward the center and seeing the mountain of chips in front of it.

**Result:** five neat stacks (5, 12, 21, 30, 42 chips) in a tight cluster, seen from a lens resting on the felt.
**Lesson:** numbers and a physical camera position fixed it in one pass. "Camera sitting flat on the edge of the table" said more than any amount of description of the chips.

## 6. 35mm instead of 16mm

> Can you do 35mm lens instead of 16?

**Lesson:** a lens change isn't just a zoom. At 16mm the chips had to be oversized to read as a mountain; at 35mm they went back to near real scale and the perspective flattened out.

## 7. Lock the camera, add the cards

> Make this POV static, I don't want you to be able to spin the table around... I next want you to add in our generated cards asset and place them face down on the table in front of you.

**Lesson:** taking controls away is a design decision too. One fixed shot is easier to compose and to reuse.

## 8. Peeling

> Add the ability to view your cards as if you were "peeling" them up off the table, only revealing the card's suit, and other imagery that separates your ace king from any other card.

**Result:** the near half of each card curls up and shows only the top of the face.
**Lesson:** "only revealing the suit" set a limit on how far the peel goes, which is what made it feel like a peek and not a flip. The faces were in their own shadow, so a small light behind the lens was needed for them to show up in ASCII at all.

## 9. Real card physics

> Make the cards closer together, so you can peel the corner of one card and both cards lift and react to that gesture. When you release the mouse button I want the cards to return to their idle state.

**Result:** the cards stack with an offset and bend together around one diagonal fold, then spring back flat with a small bounce on release.
**Lesson:** describing the behavior (both react, release returns) instead of the mechanics left room for a better solution: one shared fold line instead of two separate cards.

## 10. The casino

> Add a background that simulates what it would be like to sit at a table in Vegas, in the middle of a booming casino, flashing lights bounce around the room as slot machines buzz and light up as if they were hitting the jackpot... No humans are visible.

**Result:** about 50 slot machines, random jackpots, a progressive meter, neon, moving lights, and optional synthesized sound.

> Remove the three moving lights that sweep color beams across the floor.

**Lesson:** sometimes the fix is subtraction. The sweeping beams fought with the table for attention.

## 11. A photo reference with Seedream 5.0 Lite

My first prompt:

> Can you generate an image viewed through a 35mm cinema lens that captures what a camera would see if it were mounted flat on a poker table in a bustling casino in Vegas. Don't generate any humans, just chips and cards on a poker table with dark green "olive" felt... Make the atmosphere and environment immersive... Go, make no mistakes. I want to render something exactly the same.

Gaps: Seedream can't see this chat, so "exactly the same" meant nothing. No lens height, aim, depth of field or aspect ratio. "Immersive" and "little details" were feel words. Filler at the end.

The revised prompt described every object, the lighting and the background signs, and came back as [this](reference/seedream-attempt-1.jpg): a close-up product shot of the chips, square, with the cards off to the side.

**Lesson:** listing objects isn't enough. The image model treats the most detailed thing as the hero and moves the camera to it. What fixed the plan:

- Distances and frame proportions: cards one foot from the lens and centered, chips 4.5 feet away taking about 15% of the frame width, far rail as a thin line across the middle.
- Aspect ratio set in the generator's settings, not written in the prompt.
- A screenshot of my own scene attached as a layout reference, labeled "layout only, don't copy the ASCII style."
- Small fixes as edits on the last image, not new prompts.

## Takeaways

- Put numbers on everything: chip counts, lens focal length, distances, how much of the frame something fills.
- Place the camera physically ("resting on the felt at the edge, aimed at the center") instead of describing the view.
- Describe behavior, not implementation ("both cards react, release returns them").
- Say what to leave out: no faces, no humans, no moving lights.
- For image models, spatial relationships matter more than object detail, and a reference image beats a paragraph.
