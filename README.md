# The Long Road

A prototype of a mastery-based learning game: one idea at a time, learned by doing it in a world you walk through.
You are a seven-inch-tall elf child in Thistledown Hollow. Old Bramblewick teaches you to count, add and subtract
by gathering acorns; Fen Dewberry sells you jars; Marigold Fernsby plants in rows; Aldric Bough sorts shapes.
Past the village gate the Long Road leads to the Mallow Orchards, where Hollis Quern and Pip Willowby teach
multiplying and dividing at the mill.

Everything is one self-contained HTML file built on [three.js](https://threejs.org/) (r128, loaded from cdnjs).
No build step, no server.

## Play it

Open `index.html` in a modern browser (Chrome, Edge, Firefox or Safari). For the best experience serve it rather
than double-clicking the file, so the fonts and the 3D library load without warnings:

```
npx serve .
```

then visit the address it prints. The game opens on a region map; Thistledown Hollow (ten lessons) and the
Mallow Orchards (nine lessons) and Fernwater Crossing (nine lessons) are playable. Any lesson can be started directly, which is handy for testing.

## Units

- Everything is at real scale and measured in real units. The elf stands about 7 inches tall, and one unit in the code is about 2¼ real inches. The game shows lengths in inches, feet and miles, and lessons use real units (yards, feet, inches, chains, stone).

## One continuous world

- The land is being joined into one continent you can walk across, with no loading screens between places. All eight towns are now one world: seven joined by real land from Thistledown Hollow to Lanternreach, and Skyhaven floating in the real sky above the sea off Lanternreach. Each leg of the road is about a third of a mile for a 7-inch elf (roughly ten minutes on foot), over rolling hills with full-size trees.
- Fernwater is an open town with no fence: you can walk in from any side. The Fern runs through it and on across the country in both directions, and you can swim it anywhere (the current and drowning rules apply out there too).
- Each town keeps its own local coordinates and sits on the continent as a tile with a position and a turn. The game draws the open land and any nearby town together each frame and swaps the player between frames at the town gates, so nothing pops.
- The open land is built in 64-unit chunks around the player, a few per frame, and thrown away behind them, the way Minecraft does it. The same seed always grows the same hills, trees and stones. The Distance setting controls how many chunks are kept.
- Cairnhold stands on a highland 26 units above the lowlands; the road from Fernwater climbs to it. A town tile can sit at any height, and the land rises to meet it. Like Fernwater it is open on every side.
- Cairnhold's gorge is a real cut in the land, about 0.4 miles long, deepest by the town and running out to nothing at each end. Falling in kills you anywhere along it. You can walk around either end, or walk in at an end and follow the floor under the town. The bridge from the last Cairnhold lesson is the short way across.
- Cogsworth sits at the foot of a real crag. The cliff behind the town is the crag's face; its sides are too steep to climb, but the back is a long walkable slope, so you can reach the top and look down on the town. Stepping off the edge kills you. The bike hill east of town carries on into the open land.
- Emberdeep is a hall cut through a real mountain ridge, about 0.4 miles long and 140 units high. The hall is a pocket interior the same size as the rock it sits in: you walk into the west door and fade into the hall, and its far door comes out on the other side of the ridge. The ridge's faces are too steep to climb; its ends are gentle, so you can walk round it or up onto the crest.
- The east coast is real: a level plain behind a sea cliff, the cove by Lanternreach, and open sea beyond. You can swim the sea anywhere along the coast (the current and drowning rules apply), and the cliffs cannot be climbed from the water; the cove is the way back up.
- It is always night at Lanternreach. The sky, sun and fog darken as you come within about 1,500 units of the town and are fully night by 600, so the stars, the moon and the lighthouse beam belong to the place rather than to a separate scene.
- The sky-lift is a real cable from the end of Lanternreach's pier, over the Pillar Rocks, to Skyhaven, which floats 300 units up and about 230 out to sea. On the ride you see the real town and sea fall away below. Boarding and stepping off still use a short fade. Skyhaven is in the air, not on land: the only way up is the gondola, and stepping off an island is a fall you do not survive.
- Above the cloud it is always bright day, so the gondola climbs out of Lanternreach's night into sunlight. The stars and the moon belong to the night below.
- The flight home at the very end is still a storybook scene: a small model of the whole road passing under the glider. Flying the real 6 miles in under a minute would outrun the chunk loading.
- The Hollow's fence and the Orchards' hedge are solid from outside; the gates are the way in. The six old road scenes (the Long Road, River Road, High Road, Cliff Road, Mountain Road and Sea Road) are deleted from the code. Old saves and map links that point at them open at the matching spot in the open land.
- The open land has its animals: three deer, a fox, a badger, two pheasants, four rabbits and a flock of birds. They are one small company that keeps pace with you; anything left more than about 340 units behind turns up again somewhere ahead, clear of roads, towns, water and cliffs. Three squirrels live on the wild oaks near you, running up and down the trunks and foraging at the foot; each moves to a new oak when you leave its tree about 300 units behind.
- These animals exist only on each player's own screen. For multiplayer they need to move to a shared wildlife layer on the server (a Supabase table plus a realtime channel), so that two players standing together see the same animals.
- A bike can be granted to an account from the database (`save_blob.bike` in `game_saves`); the game keeps a granted bike whichever save wins at sign-in.

## Falling, hurt and death

- Cliffs and holes are real: the Cairnhold gorge, the bridges between Skyhaven's islands, the sea cliffs at Lanternreach. Step off and you fall.
- A fall of more than about a foot hurts (the elf is 7 inches tall); a health bar appears under the coins and refills slowly. A fall of about 3 feet or more kills you, and so does the gorge floor or the open sky, whatever you fell from. Water never hurts.
- Death is Skyrim's: the screen goes dark, a low note plays, and you wake at your last save, which the game writes every couple of seconds while you're standing on solid ground. You never save mid-air or at the bottom of something.
- Rope fences along the island rims and the rope across the gorge lane (until the bridge is built) still hold you; bridges have no rails that stop you.

## Stamina, swimming and drowning

- Stamina grows only from your own effort: running, swimming and climbing hills on foot make the bar slowly longer (up to three times its starting size). Riding never does. The game tells you each time it grows by ten.
- Swimming uses stamina. Run out in the water and you start to drown: your health drains until you reach the shore, a pier ladder or the beach, or you die and wake at your last place on dry land.
- Lanternreach's pier has ladders on both sides near its land end and at its far end, and a plank path runs from the pier up the cove beach to the town. The sea is open to swim anywhere; the camera follows you at the waterline.

## Movement feel

- **Momentum.** You take a few steps to get up to speed and a few to stop, so a sharp turn at a run slides a little. The bike is heavier: slower to wind up, longer to stop, wider turns. Water answers slowly.
- **Slopes push.** Downhill speeds you up and is harder to stop; uphill drags. Standing still on gentle ground holds you, but stand on something steep and you slide. Too steep still blocks you outright.
- **Currents.** The Fernwater river carries you downstream while you swim; the Lanternreach sea pushes you gently toward the cove. Swim against them or use them.
- **Jump** stays generous, and carries a little of your run with it. Hit a wall, a tree or a fence and you stop: whatever the world pushes back against is lost.

## Doors, fences and stairs

- Every building's door is a real leaf on a hinge. It swings open as you come within about 8 units and shuts behind you. Inside a room the door stands shut until you walk up to it, so you only see outside through an open door.
- Rooms in Lanternreach have night outside their doors, to match the town.
- In the Hollow, straight-sided things (garden beds, rail fences, the potting shed) are box colliders in `hollowBoxes`: you slide along them and cannot pass through. A box with an `id` is a building entered at its door.
- Fernsby's nursery is a 32 by 28 unit yard with its beds, cold frame, barrel and bench spread apart, a solid rail fence on three sides, and a potting shed you can walk into.
- The spiral stair round Aldric's tree is climbable: 28 steps wind once round outside the platform and land on its rim, 21 units up. The stair passes over itself, so the game picks the step nearest your own height. Above 2.6 units you are clear of furniture and fences; the trunk still blocks. Stepping off is a real fall.
- Thistledown Hollow is 354 by 208 units inside its fence (it was 128 by 176). The main street still runs 138 units from the yard to the gate, and the gate stands where it always did in the wider world; the Hollow grew east for Lantern Lane and a little north. Everything about the fence, the ground, the grass and the map inset is worked out from `FENCE`.
- Each of the four who work in the Hollow has a house on Lantern Lane, away from the workplaces (see **Lantern Lane** below). They are listed in `HOMES`. Nobody is at home: they are all at work.
- The Village Stores works as a room: the door is shut until you come near, walking through it fades you in or out, and it shuts behind you. The inside is still built at the same spot as the stump, so the ten Hollow lessons did not have to move.
- Doors inside rooms now swing outward, so the leaf never lands on furniture. The potting shed is 24 by 18 units inside.
- Every building is a pocket room again, Bough's House included. The door of a pocket building unlatches a crack with lamplight in the gap, then you fade in. `realHome()` (a room built inside its own walls, no fade) is still in the file, unused, if a see-through house is wanted later.
- You can sit indoors. Any room can call `k.seat(x, z, yaw, top, label)` from its decorate function; the stools, armchair and rocking chair in the homes, the stools in Crumb's Bakery and the Surveyors' Lodge, and the waiting bench in the Ferry Office are all seats.
- When a villager stops you as you walk up, the first thing they say has a "Not now" button. The talk keeps: walk away and come back, and it starts again from the top.
- The map is drawn from the land itself: terrain shading from `worldH`, the real road lines from `ROADS`, the Fern and the Cairn Gorge, and every town at its true position. The red marker is your real position. Skyhaven is drawn above Lanternreach because it hangs in the air over the far end of the sky-lift.
- Fernwater: the bridge rails are solid. The dock has a ramp, and you can walk along it, across a gangplank and onto the deck of the barge (`fernDeck`). The Ferry Office is a real building beside the dock with a room inside. The spare door knob on every Fernwater shop is gone, and the cakes and buns sit on the bakery counter.
- The Old Oak's knothole is sunk into the bark. The crows are crow-shaped, sit with folded wings, lift off in a widening circle when you come close and only land again once you have gone.
- Sound is all made in the browser; there are no sound files. Settings has Music (on/off and volume), "Voices and the sounds of each place" (on/off and volume) and Birdsong.
  - Music: a harp and a flute. Each town has its own key, pace and tune (`TUNES`), built once from the town's name so it is the same tune every visit; the open road gets a sparser one.
  - Voices: each line of talk gets a murmur in the speaker's own pitch, and the first line starts with a small "oh", "ah" or "hm" (`VOICES`, `voiceLine`). Nobody speaks words yet.
  - Places: wind out of doors (more on the road, in the highlands and in the sky), the Fern near its banks, the sea and gulls near the Lanternreach shore, a hum, drips and a distant hammer under the mountain at Emberdeep, and quiet indoors. Birdsong is only heard outdoors.
  - Fire: every hearth, the Stores stove, the bakery oven and the Emberdeep furnaces burn with moving flames, embers and sparks (`makeFire`), and crackle only when you stand near them.
- The bike: hold Run while pedalling to pedal hard. It is faster, drains stamina, and builds stamina over time like running does. Coasting with Run held costs nothing.
- Lanternreach pier: six ladders (three each side), all climbable. The boardwalk down to the cove can be walked back up. The sky-lift gateway is two posts and a beam across the pier; the old cable and the second set of Pillar Rocks are gone.
- The hint is a small note in the top right corner, under the task list. It shows for a few seconds and then folds away to a "?" button; tap that to read it again.
- "How fast you look around" in Settings is one sensitivity for the mouse, a finger on the screen and a controller stick, sideways and up-and-down alike, from 0.4x to 4x. (Before, it only changed up-and-down.)
- The tree-house in the Hollow has a handrail all the way up the stair and round the platform, open only where the stair arrives. From up there, no step may drop you more than a stair or two.
- Skyhaven's ropes hold: you cannot walk or jump off the side of a bridge or the rim of an island.
- Hearthmoor, the capital (first slice). It is built straight into the open land, not as a town in its own frame, so walking in and out needs no gate switch. It stands on the south shore of Hearthmere, a lake made by widening the Fern (`riverHW`), about 3,400 units east of Fernwater. A branch road leaves the Orchards road just south of Fernwater and runs to the West Gate. The city's own frame is `frameH` (x east along the river, z south from the middle of the water); everything about it is in `CITY` and `buildCity()`.
  - Inside the walls it is 1,080 by 558 units (it was 900 by 437), with three gates (West, East, South). The layout is a grid, and every measurement of it is in `CITY.st` and `CITY.sq`:
    - Five streets run east and west: **Wharf Road** (24 wide) along the quay, **North Street** (24), the **High Street** (36) from the West Gate to the East Gate, **South Street** (24) and **Garden Lane** (16) under the south wall.
    - Three run north and south: **Lake Avenue** (36) from the quay to the South Gate, and a cross street 300 units either side of it (24).
    - The **Market Square** is 244 by 156 where the High Street and Lake Avenue meet.
    - Every building's door opens onto a street or the square. The Guildhall is on the High Street; the Bank stands behind it and opens onto South Street.
    - Houses stand in terraces of three or four with a passage through to the gardens behind; the two rows of a block face their own streets and share the gardens between them.
  - Built so far: walls, towers and gates; the quay and five piers on the lake, two barges, four warehouses on Wharf Road; the Market Square with its fountain, eight stalls and eight trade halls (one named for each town); the University's four colleges on North Street with the quad behind and the Library Tower; Lakeside park and its three big houses; the Guildhall and Bank; Old Town in the west, Makers' Row in the east; three houses of **Lodgings** on South Street (Rowan, Hazel and Alder House: these are where players' rooms will be); eighteen fenced, numbered **Garden Lots** for sale, each with its gate on a street; a few townsfolk.
  - How it looks: houses have gabled roofs, stone footings, timber frames, chimneys, door frames and steps, and framed windows with shutters front and back. Trees line the High Street and Lake Avenue and stand in the gardens and the park. Iron lamps (about ninety) light at dusk and the windows shine at night, as in the Hollow.
  - The fountain runs: water falls down two tiers, ripples spread across the basin and a spray goes up from the top (`CITY.tick`).
  - Nothing flickers underfoot: every piece of ground is its own rectangle and no two overlap (`patch`). The streets used to be laid across one another at the same height, which is what shimmered at the crossings.
  - Buildings are drawn in a dozen batches (`batch`), and everything else in the city is welded into one mesh for each colour (`bake`).
  - Scale: Hearthmoor's buildings are sized for grown elves, who stand about 7.5 units tall (the player's elf is 3). A storey is about 9 units, a house door 8.6, houses are 20 to 38 wide and two or three storeys, the trade halls stand 25 to the eaves, the walls 24, the Library Tower 80. `HS` in `buildCity()` scales every building's height.
  - The eight trade halls and the eight market stalls trade (see **Things, coins and trade**).
  - Not built yet: the townsfolk outdoors do not talk, nobody in the city keeps hours, the Garden Lots cannot be bought, the colleges are shut, and no road leaves the East or South Gate.
  - The in-game map shows Hearthmoor; tapping it there takes you to the West Gate.
- The world is built for grown elves; the player's 7-inch elf is a small child in it. Grown elves stand about 7.5 units tall.
- Character creator: besides name and colours you now choose a face (Cheerful, Wide-eyed, Sleepy, Determined) and a height, from 7 inches (a little one) to 17 inches (grown). Height is `look.height`, a scale from 1 to 2.45 applied to the whole elf, the bike included; the camera sits higher and further back for taller elves, and they walk a little faster. Face is `look.face`.
- Hats on the townsfolk (`makeElfNPC`) are pulled down onto the head, and Marigold's sun hat has a crown.
- The four homes in the Hollow are grown-elf houses. Outside they are 52 by 32 units and two storeys (`buildHouse`). Inside each is 76 by 40 and 20 high (`familyHome`), on two floors:
  - **The hall** (east, 54 by 40, open to the roof): the hearth with a stone chimney breast, the owner's things, two roof trusses, a wheel of candles, a settle facing the fire (seats two), a round game table with three stools, a tall clock, cloaks on pegs, pictures and wall hangings. Eight or ten players can stand about in it without crowding.
  - **Kitchen** (the whole ground floor of the west wing, 22 by 36): a range with a fire, a long counter, a table with four chairs and room to walk right round it, and a pantry corner under the stair. Two doorways lead in from the hall.
  - **Stair** along the south wall of the wing: fourteen steps up to a landing in the south-west corner. It is closed off from the kitchen by a panel, which carries the handrail upstairs.
  - **Upstairs** (10 up): the child's room at the head of the stair (small bed, small table and stool, toy chest, toys) and through a doorway the parents' room (big bed, wardrobe, chest, chair).
  - How it works: a room can now give its own floor height (`floorAt`) and say which floor a wall, rail or piece of furniture belongs to (`lv`). You are on the upper floor once you have climbed to it; rails and furniture only block on their own floor, and a seat can only be picked from its own floor. Other buildings have no upstairs yet.
- The scale pass: every walk-in building in the other towns, the potting shed and the Ferry Office are now sized for grown elves (`growBuilding`). Each is about 1.7 times as tall, a little wider, and deeper away from the street; the front door stays where it was, so streets and lessons are untouched. Doors are 8 to 9.5 units tall. The way out of every room is as tall as its ceiling allows, up to 9.
- Still at the old scale: the furniture and room sizes inside the shops (the lessons are set out around them), and the Village Stores, which is a hollow stump 22 across and 13 high with a 6-unit door.
- **The river sits in the lowest ground.** The Fern used to be drawn at one level whatever the land did, so wherever the country dipped it ran along a raised causeway with a sheer drop (15 to 27 units) ninety units back from each bank; drawn on the 4-unit land grid that drop looked like terraces, and trees on its edge hung in the air. Now land within 330 units of the river is never lower than the bank and comes up to it gently (steepest slope in the whole corridor: 0.46). The bank top is 1.2 above the bed's datum instead of 0.35, so the waterline falls on the steep part of the bank and is a clean line instead of a sawtooth. Hearthmoor's own shore stays level with the quay.
- Paths in the Hollow each lie a hair higher than the last, so where two cross nothing flickers.
- The two barrels outside the Village Stores stand clear of the stump's wall.
- Animals that get stuck pick a new direction. They are only ever moved to a clear spot when more than 140 units from you, so you never see one jump.

## Lantern Lane, the gardens, and night

- **Homes are apart from work.** Lantern Lane leaves the main street just south of the Stores and runs 262 units east to a round with a tall lamp in a ring of flowers. Two houses stand on each side, every front door facing the lane: 1 Bramblewick and 3 Bough on the north side, 2 Fernsby and 4 Dewberry on the south. Marigold no longer lives behind her own potting shed.
- **Each house has a plot** 80 wide and 72 deep inside a white picket fence. In front: a gate under a rose arch with a lantern in it, a letterbox with the house number, a path to a porch, flower beds under the windows, two round bushes and a blossom tree. Behind: a big shade tree, a vegetable patch, a washing line, a bench you can sit on, and one thing of the family's own (Bramblewick's beehive, the Fernsbys' cold frame, Bough's woodpile and chopping block, the Dewberrys' berry bushes). A gate at each side of the house leads through to the back garden. The fences are solid.
- **The green.** Between the Stores and the lane stand The Sleepy Beetle (the inn), a well with a roof and two benches, and trees. More trees stand along the north fence and in the meadow south of the gardens. The lessons, shops and teachers are where they were, with the old house plots opened up around them.
- **Day and night.** The clock is the real one (see **The clock, closing time and the night** below). Night is dim and blue, never black. It reaches every outdoor town and the open land, and shows through the windows of every room; Skyhaven above the cloud stays bright, and Lanternreach is still always night.
- **Lamps.** Ten iron street lamps stand along the lane, three on the main street and one tall one at the round. By day each is only a post. After dark the lantern glows and throws a pool of light on the ground, the lanterns in the garden arches and by the front doors come on, and every window on the lane shines. A line of text tells you when night is falling and when the sun comes up.
- The paths have their grain at any length (the dirt used to be one picture stretched the whole way down the street).
- How it works: `LANE` and `HOMES` place everything; a house that faces north is the same house turned half round (`h.sg` turns a spot on its own plan into a spot in the Hollow). `bake(group)` welds a finished group into one mesh for each material, so a house with its garden is about ten draws instead of five hundred. `clockNight()` gives the night from the clock, `setNight` now also dims each town's own lights, and anything that should light up after dark adds a function to `NIGHT_GLOW`. The street lamps add no real lights (those are costly on a phone): the glow is a picture laid on the ground.

## The clock, closing time and the night

- **The clock is real.** A day in the game is 24 hours. This continent keeps Central time whatever zone the player is in, so everyone standing together sees the same sun; it is one line (`ZONE`), and each continent can have its own later. The sun is up by 7 in the morning and down by 7:30 in the evening. The map panel says what time it is.
- **Hours in the Hollow.** Shops open at 7 and shut at 9 in the evening. At 9 Bramblewick, Marigold, Aldric and Fen leave their work and walk home down the main street and along Lantern Lane, each to their own front door. They are by their own fire until 10, then upstairs in bed, and they set off for work again a little after 6:30.
- **Learning never stops.** If any lesson is under way, all four stay at their places however late it is. With no lesson under way and the teacher gone home, go to their house and knock (or walk in): Bramblewick, Marigold and Fen will give you their lessons at any hour, and the lesson starts at its usual place with the teacher there. Aldric teaches nothing yet, so he tells you the shop opens at seven.
- **Honesty boxes.** A shop that is shut and sells food (the Stores, Dewberry's stall) still lets you buy the food from an honesty box; nothing else, and nobody is there to buy from you. A shop with no food (the potting shed, the woodshop) says it is shut till seven.
- **The Sleepy Beetle** is the inn on the green, and it never shuts. Posy Underbough keeps it from 7 in the morning to 7 in the evening and her brother Moss through the night. It sells hot cocoa and barley stew (both made here), and bread, pie, cider and nut mix. Inside there is a great hearth with the carved beetle asleep over it, two settles by the fire, four tables with stools, and the bar; ring the bell on the barrel. Three travellers sit in from 5 in the evening until 11: a carter, a road surveyor and a pedlar.
- **Anyone out at night has a reason,** and will tell you it. Wick Tallow the lamplighter walks the lane from dusk to dawn. Bryn Ashlock minds the gate from 9 until the Hollow wakes. Both sleep by day at the inn.
- **Not yet:** every town keeps hours. The inn has no upstairs you can visit, so the travellers, the lamplighter and the watchman simply are not there when they are asleep. Nobody sits down: people at home stand by the fire, then lie in bed.
- How it works: `daySecs()` reads the clock in `ZONE`; `hollowAbed()` and `shopsShut()` are the hours. `FOLK` holds the four with their post, their home and the way between; `nightLifeTick` walks them, puts their indoor figures by the fire or in bed, and shows or hides everyone in `folk` (each has an `on(hour)` test). `shopShut(id)` and `shopRing(id)` decide between the shop, the honesty box and a shut door. `roomNight(room)` darkens a room's windows.

## Fixes in build 107

- **The Hollow Stables.** The tree that grew through the roof is gone. Flowers, grass and stones no longer come up through the stable floor. The stable's sign sits on the edge of the eave where the roof does not cut it, and Bracken's name board hangs from the beam on two chains.
- **Bracken stays one size.** He used to grow when a taller elf got on, because he took on the rider's size. He is now the same size ridden or standing, whoever rides him.
- **Cogsworth.** The "To Emberdeep, over the hill" sign stands beside the path on two solid posts instead of in the middle of it. The arch at the top of the hill is twice as tall and wider, for grown elves.

## Lanternreach at full size

- **Windward Row.** A new street runs west from the lane, between the chandlery and the Tide Office, with two full-size family houses on each side: Vell, Reef, Ebb and Wick. Each has its fenced garden, gate, letterbox and a walk-in home inside, the same as the houses in the Hollow. The town's ground now reaches 180 units further west, and the plain is levelled under the row so the houses sit square. It meets the open land with no edge.
- **Three new people.** Rowan Reef keeps the chandlery, Marnie Ebb the Tide Office, and Jory Wick the light. They talk about fathoms and compass bearings, the tide as a wave, and the beam's thirty degrees a second.
- **Hours.** It is always night in Lanternreach, so people go by the clock: home along the row at nine, by the fire, then bed, and back at seven. The chandlery bell is shut at night. Knock on Astra's door and she will still teach.
- **Taller lamps** down the lane, to suit grown elves.
- **Every building is grown-elf size (build 107).** The lighthouse and the observatory are twice the size they were, and the observatory's door now faces the lane. The keeper's cottage, the chandlery and the Tide Office are nearly twice as wide and deep, with doors a grown elf walks through. Inside, all four rooms and everything in them are 1.6 times bigger. The chandlery bell sits on a low shelf on the front of the counter so a small elf can still reach it.
- **The cove path** now lies on the ground from the lane to the head of the boardwalk. The old flat strip that stuck out over the cliff is gone.
- **The sky-lift** comes down to the pier. The gondola waits with its floor level with the deck at the end of the pier, and you walk straight in.
- **Not yet:** you cannot look through the observatory telescope. The two painted hills that stood west of the town are gone, because the row stands where they were.

## The Ice Cave on Frostpeak: frost goblins, and Old Rimeclaw who throws snow (build 132)

**Where**
- A little over half way up the Frostpeak trail, on the uphill side: a mouth in the rock with icicles hanging over it, a sign that says "The Ice Cave", and snowballs lying on the path in front. Walk into the mouth to go in.
- Sister Linden Hush at the monastery will tell you where it is.

**Inside**
- A long cave of blue ice: pillars of ice from the floor, icicles from the roof, snow drifted against the walls, a frozen pool, and clumps of crystal that give the light.
- **Five frost goblins.** They are pale blue, in white rags, and a little tougher than the goblins in the Hollow: each takes four points of harm. A white belt needs four hits for one; a black belt needs one.
- **Old Rimeclaw** at the far end, nearly twice their size, with a crown of ice. He takes fourteen points of harm.
  - He sees you from most of the way down the cave, and there is no talking to him.
  - While you are out of his reach he throws snowballs, one every two or three seconds. Each is aimed at where you were standing when he threw it, so keep moving and it misses. One that lands costs five health.
  - Up close he clouts like the goblin chief does.
- **The heap** behind him: once every frost goblin has been seen off, look through it for 45 acorn coins. This pays once.
- They are all back the next time you come in, the same as in the Goblin Hole.

**Under the bonnet**
- The game now keeps each cave's creatures apart, so more caves can be added without their goblins getting mixed up.
- The cast sheet has the frost goblin and Old Rimeclaw on it.

**Not yet**
- This is the only cave on the mountain so far.
- Frost goblins are yours alone, like the Hollow's: another player in the cave sees their own.

## Frostpeak: a mountain of snow, a monastery on top of it, and a dojo with three belts to earn (build 131)

**The mountain**
- Frostpeak is a new mountain north-west of Fernwater Crossing, off the road that runs north from Fernwater to Cairnhold. It is 320 high and 1,400 across, which makes it the biggest thing in the land by a long way.
- It is under snow from a little above its foot to its top, with grey rock showing on the steep faces, pines on the lower and middle slopes, and snow falling whenever you are on it.
- **From far off** you can see it white against the sky, before the haze would normally let anything that far away show.
- **Finding it:** on the road north of Fernwater there is a signpost, "Frostpeak Monastery, up the mountain". A line of stone cairns with yellow flags runs from the signpost to the foot.
- **The climb:** a trail winds most of the way round the mountain from the foot to the top, cut level so it can be walked, with a red pole and flags every little way. It is a long walk on purpose. The top is flat.
- Once you have reached the top, Frostpeak is on your map and you can travel straight back to it.

**The monastery**
- **The gate** where the trail comes over the lip, with a stone way running from it to the hall, stone lanterns that light at night, and prayer flags strung across the yard.
- **The hall:** open at the front, under a two-tier roof with snow on it. Inside are mats, a rack of staves and a scroll with one brushed circle.
- **The yard:** three striking posts. Any of them can be punched and will rock on its base.
- **Three cells** for the monks, each open to the yard, with a sleeping mat, a blanket and a lamp. You can walk into them.
- **The bell,** which you can ring.
- **A fire bowl** with three stones to sit on round it, and **a frozen pond** you can walk across.
- **Five people live here:** Brother Sorrel Deepsnow at the gate, Brother Tobin Quietfoot sweeping the yard, Sister Linden Hush at the bell, Pim Snowberry (the newest, and a child) by the fire, and Master Oren Frostmantle in the hall.

**The dojo: belts**
- Speak to Master Oren. He teaches one belt at a time, and each belt is earned in a drill, not by counting hits.
- **The drill:** stand at the middle post. Master Oren lights the lantern over it at moments of his choosing. Strike while it is lit. A strike before the light counts against you, and so does a light you miss.
  - **Yellow belt:** 6 lights, land 4.
  - **Green belt:** 8 lights, land 6, and the light is quicker.
  - **Black belt:** 10 lights, land 8, and the light is quicker again.
- Walk out of the yard in the middle of a drill and he puts the lantern out. You can try as often as you like.
- Your belt is worn: the belt on your elf turns yellow, green or black.

**The four punches**
- **White belt (no belt):** the flap. Leans away, looks away, both arms going. One point of harm, one goblin.
- **Yellow belt:** the straight punch. Weight back, then one straight arm with the body behind it. Two points, and it can catch two.
- **Green belt:** the one-two. Both fists up, a quick jab with the left, then the right comes through. Two blows from one press, two points each.
- **Black belt:** the open palm. Both hands drawn back to the hips, then both palms out together, with a ring of air off the blow. Five points, a long reach, up to four at once, and whatever it lands on goes a long way back.
- Twelve landed punches in the Goblin Hole still teach you the straight punch by yourself, but not the belt.
- The Punch button shows on a touch screen at the monastery as well as in the cave.

**Not yet**
- The caves on the mountain. Sister Linden mentions them; none can be entered.
- Other players see your elf at the monastery but not the colour of your belt, and each player has their own drill.
- The monks stay at their posts day and night. They do not sleep, eat or train yet.

## Hands clear of the coat, necks for everyone, and a fan that turns the right way (build 130)

**Your elf's arms and hands**
- The arms now lie along the outside of the coat with the hands resting at the hip. They used to hang straight down inside the flare of the coat, so the sleeves vanished into it and the hands came out under the hem.
- A rounded shoulder joins each arm to the body.
- This is the same for every other player's elf, at every height from child to grown.

**Necks**
- Every elf has a neck you can see: yours, other players', all the townsfolk and travellers, and the eight hand-made folk of the Hollow. Heads sit a little higher to make room.
- The plain townsfolk also get a closed top to the coat under the neck.
- The goblins keep their heads on their shoulders, as goblins do.

**Hair**
- Your elf's hair now comes down the back of the head. It used to stop in a band, with bare skin below it.

**The wind tunnel at Skyhaven**
- The fan turned end over end. It now spins on the drum's own axle like a wheel, with a hub in the middle and a pitch to the blades.

## Shoulders for the eight hand-made folk of the Hollow (build 129)

- Old Bramblewick, Marigold Fernsby, Aldric Bough, Fen Dewberry, Hollis Quern, Pip Willowby, Captain Oriel Tideknot and Maudie Crumb were each modelled by hand, so build 128's shoulder fix missed them. They have it now.
- For each one the game finds where the body really is at shoulder height, brings the arm in to meet it, and rounds the join with a shoulder in the colour of the sleeve.
- Your own elf and the goblins are built differently again and were not changed.

## Nine fixes from the first walk round Tinderfold: the book, a swing, a rocking chair, shoulders, and signs you can read (build 128)

**The book in your hands**
- The panel that sat over the pages has gone. Nothing is in front of the book now except two small buttons in the top corner: **Close** and **Plain text**.
- **Turning pages:** tap or click the right-hand page to go on and the left-hand page to go back. On a keyboard the right and left arrows do the same (so do Page Down and Page Up), and Esc closes the book. On a gamepad it is D-pad right and left, A to turn on, and B to close.
- **Walking with it:** you can walk, run and jump with the book open. While you move, your elf lowers it and you see the world as usual; when you stop, the book comes back up and you are looking at the pages again. You can sit down with it too.
- **Out of the bookcase:** when a book comes off a shelf or a table your elf turns away from it to read, and anything between your eye and the pages is left out of the picture, so a shelf no longer cuts across the page.
- The pages now fill the screen instead of the top half of it.
- The book shuts itself if somebody starts talking to you, or you open your satchel or the map.

**A swing you can swing on (the Cinders' yard, Tinderfold)**
- The ropes hang from the bough at each end of the seat, the right way round.
- Sit on it and it starts to swing, gently at first and then higher, and your elf goes with it. Get up and it runs down by itself.
- Tilly Cinder now stands beside the swing, not in it.

**A rocking chair you can rock in (Odwin Flint's porch, Tinderfold)**
- The chair has been rebuilt: curved rockers on the boards, four legs, a seat, a back and arms. Nothing is sunk into the porch.
- Sit in it and it rocks.

**Everybody's shoulders, heads and hats**
- Arms are set into the body with a rounded shoulder, where they used to hang a little way off it.
- Children and the short mountain folk get arms to their own size. A child's arms used to reach the ground.
- Hair and hoods sit back off the face. They used to come down level with the eyes, which is what made Tilly's hat look like a mask.
- Children get a slightly smaller head.

**Sitting properly**
- Gorse Tallow sits on the driving box with his knees bent and his hands out for the reins. He used to stand straight up through the seat.
- The four passengers sit on the side benches.
- The travellers at the inn's tables sit on their stools with their knees under the table. They used to stand sunk into the floor.

**Green grass on Low graphics**
- On Low and Medium, the land far from the Hollow was being left out of the picture by mistake, so the pale under-layer showed through and the ground looked like sand. The land is drawn everywhere now.

**Signs and roofs in Tinderfold**
- Flint: the name board stands on the front edge of the porch roof, facing the street. The top logs no longer poke through the roof.
- Cinder: the name board is on the wall beside the door, clear of the little roof over it.

**Not yet**
- Putting a book back has no reach animation.
- Only these two seats move. The rocking chairs indoors do not rock yet.
- The swing and the rocking chair are not shared: another player sees you sitting, but not swinging.
- The people of the Hollow who were made by hand (Bramblewick, Marigold, Aldric, Fen, Hollis, Pip, Oriel, Maudie) keep the shoulders they had.

## The Goblin Hole: the first cave, goblins, a side quest, and a punch that gets better (build 127)

**Where**
- There is a mound of rock with a timbered mouth against the north fence of the Hollow, signed "Goblin Hole", north-east of the Village Stores. Walk into the mouth to go down.
- Beside it is a post with a note and a tin. Read the note to take on the quest.

**The quest: the pinched things**
- Old Bramblewick's note says things have gone missing from the Hollow, and asks you to get them back from the goblins.
- The pinched things are in the chest at the goblins' camp, at the far end of the cave. There are two ways to get at it:
  - **Riddles.** Speak to Grizzlegob, the goblin chief, before any fighting starts. He asks three number riddles, each with three answers to pick from. Get all three and he keeps his word: the chest is yours and his goblins leave you alone from then on. Get one wrong and they all come for you.
  - **Thumping.** See off all six goblins and the chief.
- Carry the sack back up and leave it by the tin. There are 30 acorn coins in the tin.

**The cave**
- A long cave: rock underfoot, boulders along the walls, stalagmites and stalactites, a still pool, and clumps of glowing toadstools that give most of the light. The goblins' camp has a fire, crates, bedding and the chest.
- Two goblins watch the way in, two are in the middle, two are at the fire, and the chief stands by the chest. A goblin that sees you runs at you and thumps you, which costs a little health. Health comes back by itself.
- A goblin takes three weak hits. The chief takes six. A beaten goblin shrinks and scampers off. They are all back the next time you come down, so there is always somebody to practise on.

**Punching**
- Punch with the **F** key (it can be changed in Settings), the **right trigger** on a gamepad, or the **Punch** button, which shows on a touch screen while you are in the cave.
- **A beginner's punch** is what you start with: your elf leans back away from it, turns its face aside and flaps both arms. It is slow, it reaches a short way, it does one point of harm, and it can only catch one goblin.
- **After twelve punches that land**, the game tells you your punch is better. Now your elf puts its weight back, then drives forward behind a straight arm with a closed fist. It is faster, reaches farther, does two points, knocks a goblin well back, and can catch two standing together.
- Punches landed are counted and saved, so the skill is yours to keep. This is the first step of a skill that a dojo can teach further.
- You can throw a punch anywhere. It only lands on goblins.

**Not yet**
- Goblins are yours alone: another player in the cave sees their own goblins, not yours.
- Goblins are only in this one cave. There are no other creatures and no dojo yet.
- The punch cannot be thrown while sitting, riding or reading.

## A book in your hands: reach for it, hold it open, turn the pages (build 126)

**Reaching for a book**
- When you choose to read, borrow, buy or take a book from a shelf, the shelf picture steps aside, your elf turns to that exact place on the shelf, the arm goes up to it, and the book comes away into the hand. The place on the shelf is empty from that moment.
- A book you are only reading at the shelf leaves its gap while it is in your hands and goes back when you close it.
- The same happens with a book lying on a table.

**Reading**
- You now hold the book open in both hands and look down at it as your own eyes would. The words are printed on the pages themselves: the title page first, then two pages to a spread.
- "Turn the page" lifts the right-hand page and swings it over to the left, with the next pages under it. "Back a page" turns it back the other way.
- On a tall narrow screen, such as a phone held upright, you look at one page at a time so the print stays large, and the view slides across before a page turns.
- While you read you see the book and your two hands on it, and nothing else of yourself.
- The buttons under the book work as before on every way of playing. The words of the open pages are also there, unseen, for a screen reader.
- **Plain text** switches to the old parchment panel, for anyone who finds that easier to read. "Hold the book" on that panel switches back. The choice is remembered.
- This is how every book is read now: from a shelf, from a table, from your satchel, the keeper's log, and the recipes at the forge.

**For VR later**
- The book is a real object with real pages and a hinge, held between the two hands, so in a headset it can be gripped and its pages turned by hand. That step is not built: the game has no VR mode yet.

**Not yet**
- Putting a book back on a shelf does not have a reach; the book drops into its place as before.
- Checkers: looking down at the table while you play is still to do.

## The keeper's log is kept, and the forge's recipe book can be read (build 125)

**The Log of the Lanternreach Light**
- The log book open on the table in the Keeper's Cottage can be read, and it is being kept. Jory Wick, who keeps the light, has written an entry for each of the last seven real days, and the last page is always today's. Come back tomorrow and there is a new one.
- Each entry has the day and date, the wind and the sea, the Moon, the tides, the ships that passed, the oil burned, and a line about the keeper's day.
- **The Moon and the tides are real.** The Moon's phase is worked out from the date, and the tides follow it: spring tides at new and full moon, neap tides at the quarters.
- The oil count goes up by 2 pints a day through the week, as the sign on the wall says. The wind, the ships and the keeper's remarks are made up, but a given day always reads the same for everybody.

**Recipes of Ore and Flux**
- The book on the lectern in Emberdeep stands beside the balance scale in the forge and is Master Durna Ironvein's recipe book, so that is what it now is: six pages on recipes written in parts, scaling a recipe up, and finding a missing amount, in the forge's own words about every recipe being a balance.
- It stays on its lectern. It can be read there but not carried off.

**Also**
- The book on the desk in the Observatory can be read: A First Book of the Night Sky, by Astra Vell.

## A frame-rate counter, graphics settings that change what is drawn, and the last of the books (build 124)

**The frame-rate counter**
- Settings, under Graphics, has a new tick box: "Show the frame rate". It puts a small line in the bottom-left corner with three numbers: frames a second, draws, and triangles.
- **Frames a second** is how smooth the game is. About 60 is smooth; under 30 feels choppy.
- **Draws** is how many separate things the device was asked to draw for that picture. This is the number that makes a phone or tablet work hard and get warm.
- It works on a phone and a tablet as well as a computer.

**What each quality setting does now**
- **Low**
  - at most 30 pictures a second, which is the largest single saving of heat and battery
  - no shadows, and one screen pixel per game pixel
  - a third of the grass
  - hands are round mittens, with no fingers
  - animals lose the small shaping on their legs, birds overhead lose their legs, books lose the band on the spine
  - things too small to matter at their distance are not drawn at all
- **Medium**
  - shadows, most of the grass
  - fingers only when close
  - only the tiniest far-off things are left out
- **High**: everything, with the sharpest shadows.
- Measured standing on the green in the Hollow: High asked for about 5,000 draws, Medium about 3,700, Low about 1,000. Before this build Low was about 1,400 and Medium about 5,000.
- The frame rate itself could not be measured here, because the test machine has no graphics chip. The counter is there so it can be read on the real devices.

**The last of the books**
- The four books on the Library's reading tables and the three on the librarian's desk are real books now. Walk up and choose "Pick up": read it there, borrow it for a coin, or buy it. When the place is empty you can put one of your own books down there.
- The high shelves in the Village Stores hold real books, two shelves at each of five places. Reading is free and a book costs 8 coins. There is no borrowing from a shop.
- Old Bramblewick's cottage was already done in build 123: its books are the bookcase in his study.
- **Still only to look at:** the book on the lectern in Emberdeep and the keeper's log at Lanternreach. Each needs something written in it first.

## Real books on real shelves: read, borrow, buy and put back (build 123)

**Every book is its own book**
- A bookcase is now rows of places, and every book stands in one place as its own object with its own cover colour and band. Take a book down and it leaves a gap. Put a book into an empty place and it drops in and stays there, in that place, on every later visit.
- Walk up to a bookcase and choose "Look along…". The shelf you are looking at is shown as a row of spines in the same order as on the wall. Pick a book or an empty place. It works by tap, mouse, keyboard (arrows along the shelf, up and down for the shelf above or below, Enter, Esc) and gamepad (stick or D-pad, A, B).
- The tall library walls are split into three parts each, and you can look along any of the seven shelves from the floor, which is what the ladder is for.

**Reading, borrowing and buying**
- **Reading at the shelf is always free.**
- **The Library Tower in Hearthmoor:** borrow a book for 1 acorn coin for three real days, or buy it for 8 and keep it. Sage Inkwell explains this when you speak to her.
- **In somebody's house:** you may read any book there. To take one away, ask to borrow it (free, three days) or ask to buy it (6 coins). Whoever lives there has to be at home to say yes; if they are out the game says so.
- **Your own bookshelf**, in your room at a lodging house, starts empty. Books you own can stand on it, in whatever order you put them.
- A satchel holds six books. They are listed in the satchel with how long each is due back in, and a Read button, so you can read a carried book anywhere.
- **Bringing one back:** put a borrowed book into any empty place in the building it came from. If it is not back on its day it is taken home to its shelf for you, with no fine.
- A book you own can go on any shelf anywhere. On somebody else's shelf, that is giving it to them.

**The books**
- Twenty-nine titles. The twelve that were already written, six new ones (Shapes a Builder Uses, A First Book of the Night Sky, Acorn Coins, Telling the Time, Halves and Quarters, Games for a Round Table), and a times-table book for each number from 2 to 12 with a help for learning it.
- About 1,460 books stand on 1,860 places in 24 bookcases: the Library (three walls and four low shelves), the bookcase in the hall of the houses that have one, the bookcase in the study of the houses that have one, and your own.
- With 29 titles and that many places, the Library holds many copies of each. More titles are the thing to add next.

**How it is stored**
- A case's books are drawn as one batch for speed, but each is separate and can be taken or placed on its own.
- Only what you have changed is saved: which places you emptied or filled, and the books you carry. It is saved with your game and goes with an account.
- Shelves are per player for now. If you take a book, another player still sees it on their shelf.

**Not yet**
- The four books lying on the Library tables can be read but not carried.
- The stacks of books in the Village Stores and in Old Bramblewick's cottage are still only to look at, and so are the three on the librarian's desk.
- Checkers: looking down at the table while you play, and pieces that glide on the screen board, are still to do.

**Also fixed**
- On the lowest graphics setting, the code that thins out grass was thinning the books too. Books are left whole.
- Opening a game board or a bookshelf now gives you the mouse pointer back if the game had hold of it.

## Five more table games: chess, pairs, pig, stones and the sliding puzzle (build 122)

The round table in each family hall now has a game you can play, the same way as checkers: walk up and choose it, or sit on one of the two side stools. A board opens on the screen, and the real pieces on the table move as the game goes. Tap, mouse, keyboard (arrows or walking keys, then Enter or Space; N for a new game; Esc to stand up) and gamepad (stick or D-pad, A, Y, B) all work.

A second elf who sits on the other stool takes the other side, and everyone in the room sees the pieces move. While the other stool is empty, the board plays the other side. Anyone else can open the board to watch.

- **Chess** (Old Bramblewick's and Astra Vell's). The full rules: check, checkmate and stalemate, castling, taking in passing, and a pawn that reaches the far row becomes a queen. Pieces you may move have a gold edge, and the squares a picked piece can go to have a gold spot. The board's own play looks one move and one reply ahead, so it takes what is free and is easy to beat.
- **Pairs**, the card game (the Fernsbys', Rowan Reef's, Clementine Verge's, Pell Reedmace's). Sixteen cards face down, eight pairs of pictures. Turn over two; a match is yours and you go again. Most pairs wins.
- **Pig**, the dice game (the Dewberrys', Jory Wick's, Corwin Sedge's). Roll to add to the pot, hold to bank it; a one loses the pot and your turn. First to 50. It is all adding up, and deciding when to stop.
- **Stones** (Odette Spindle's, Wren Ashcombe's). The old sowing game, also called mancala: six pits each with four stones, and a store. Pick a pit and its stones are dropped one at a time round the board. End in your store and you go again; end in an empty pit of your own and you take the stones across from it. Most stones in the store wins.
- **The sliding puzzle** (the Boughs', Tamsin Gearwright's). Eight numbered tiles and a gap; slide them into order. There are no sides: anyone sitting at the table can move a tile, so two can do it together. The tiles on the table show their numbers as dots.

**Not yet**
- Knucklebones, the map table and the tea table are still only to look at, and so is the long-running chess game at Fen and Pip's.
- Chess has no draw by repeated moves and no choice of piece when a pawn is crowned.
- The pieces on the table jump to their new squares; they do not slide as the checkers do.

## Every clock tells the time, and a telescope you can look through (build 121)

**Clocks**
- Every clock face in the land now shows the real time, with an hour hand and a minute hand that move. That is the tall clocks in the house halls, the wall of clocks at Gearwright House, the six clocks in the Clockworks, the great clocks over the doors on Cliff Row, and the Cogsworth town clock, which already did.
- They all show the game's own time, which is Central time for every player, the same clock the sun and the shops go by.
- A clock in a house built as the mirror image of another has its hands turn the other way inside, so it still reads right.
- Since build 120, some halls have a bookcase or a dresser where the clock used to stand, so not every hall has one.

**The observatory telescope**
- In the Observatory at Lanternreach, walk up to the eyepiece and choose "Look through the telescope". It works by tap, mouse, keyboard and gamepad, like any conversation.
- There are ten things to look at, each with a picture and a few sentences about it: the Moon, Jupiter and its four big moons, Saturn and its rings, Mars, Venus, the Seven Sisters, the Great Nebula in Orion, the Andromeda Galaxy, the double star Albireo, and the Pole Star with the sky turning round it.
- **The Moon is real.** It shows tonight's true phase, worked out from the date, and the words name it (waxing crescent, full moon and so on).
- Jupiter's moons go round at their true speeds, so they are in different places each night, but where they start is made up. The planets are always on show, whatever is really up tonight.
- The first time you have looked at all ten, you get 10 acorn coins.

## Every hall furnished its own way, birds with legs, and a map that fills in as you walk (build 120)

**The hall in each house**
- The big living room used to be the same in every family home. Now six things change from house to house:
  - the lights: a wheel of candles, or three hanging lanterns
  - the seats by the fire: a settle, two armchairs with a little table, or two benches facing across a low table
  - the tall piece by the door: a clock, a bookcase, or a dresser with the best plates
  - beside the chimney: two pictures, three small portraits, or a shelf of jugs
  - on the long wall: cloth hangings, a shelf of pot plants, or a patchwork quilt
  - the far corner: a fern, a basket of logs, or a tall vase of flowers
- Each house's choices come from its name, so a house looks the same every visit and to every player. All thirty homes come out different from each other.
- Every new seat can be sat on, and all the new furniture is solid.

**Birds overhead**
- The birds that circle over the towns now have a head, a beak, a tail and two legs tucked back under them as they fly.

**The map fills in as you walk**
- The road, the River Fern and Cairn Gorge are now drawn only where you have been. The map marks the land around you as seen while you walk, ride or fly, and the lines grow as you go.
- A game saved before this build starts with the land round each town it has found, and the road between any two found towns that are next to each other.
- What you have seen is saved with your game, and on an account it goes with you to other devices.
- The shape of the land (hills, valleys, the sea) still shows across the whole map, so a river valley can be made out faintly before you have walked it.

**A fix found on the way**
- The list of towns you have found and the list of things you have picked up were saved under the same name, and each could wipe the other. They are kept apart now. Before, a thing you picked up could be back in its place too soon.

## Checkers for two, Bracken goes round things, and legs for the small animals (build 119)

**Checkers you can play**
- There is a real checkers board on a round table out on the green in the Hollow, just west of the Village Stores, with a stool each side. The Willowbys' house, Barnaby Ratchet's house and two town houses have the same board on their round tables.
- Walk up and choose "Play checkers", or sit on one of the two stools. You take Red or White, whichever stool is free, and a board opens on the screen turned so your own pieces are nearest you.
- A piece you may move has a gold ring. Pick it, then pick a gold spot. It works by tap, mouse, keyboard (arrows or the walking keys, then Enter or Space) and gamepad (stick or D-pad, then A).
- The rules are the usual ones: pieces move one square on the diagonal, a jump must be taken when there is one, a piece that can jump again goes on, and a piece that reaches the far row is crowned and can move both ways. You win when the other side has no pieces or no move left.
- **Two players:** when another elf sits on the other stool they take that side, and each move goes to everyone in that place. Anyone else who walks up sees the pieces move on the table and can open the board to watch.
- **On your own:** while the other stool is empty, the board plays the other side. Its play is simple.
- New game sets the pieces back. Stand up (or Esc, or B on a gamepad) leaves the game and frees your stool. If an elf shuts the game without standing up, their stool frees itself after about nine seconds.
- **Not yet:** the chess, cards, dice and other table games in the houses are still only to look at. There is no draw rule, so a game of kings chasing each other goes on until someone presses New game.

**Bracken goes round things**
- When you whistle, the dog now looks a few steps ahead and runs round houses, trees, tables and the shop instead of through them, in every town and out on the road.
- If he cannot reach you (you are indoors behind a wall, say), he stops as near as he can and waits, and the game tells you so.

**Legs for the small animals**
- Mice and hedgehogs have four little legs that step when they run, and the hedgehog has eyes.
- Ducks have orange legs and webbed feet that paddle under them as they swim, and eyes.

## Other elves on the map, which way you face, voice on iPhone and iPad, and Bracken's tail (build 118)

**Other elves on the map**
- Every other elf who is playing shows on the big map as a blue dot with their name, wherever they are in the land, so you can find a friend or your family and walk to them.
- Each game tells the others where it is on the map every 3 seconds. A dot goes away about 15 seconds after that elf stops playing.
- If "See other elves" is off in Settings, you are not on anyone's map and you see no dots.

**Which way you face**
- Your red "You are here" mark now has a small arrow that points the way your elf is facing. It turns as you turn, in every town and out on the road. Indoors the arrow hides.

**Voice on iPhone and iPad**
- The Voice button now always shows next to Say once you are playing. Before, it could stay hidden on a phone or tablet.
- If voice cannot work yet, the button says "Voice (off)". Tap it and it tells you why. Most often the device does not know your birth year: open Settings, pick your birth year under parental controls, and voice works.
- Sound from other elves now plays through the iPhone and iPad speaker properly. Apple devices do not play this kind of sound through the game's normal sound path, so they now use a plain audio player for each elf. If the phone blocks the sound, tapping Voice again starts it.
- The birth-year box in Settings now shows on a device that has no birth year and no parent passcode yet.

**Bracken's tail**
- The dog's tail now joins the rump, with a rounded root, and wags side to side from that point instead of swinging around its middle.

## A bigger Village Stores, and a whistle for Bracken (build 117)

**The Village Stores**
- Old Bramblewick's stump is much bigger: 30 across and 21 high instead of 22 and 13. The doorway is tall enough for a grown elf to walk through, under a wooden arch.
- There are two rings of round windows, a broader mossy roof with its curl, and a taller chimney.
- The lantern post, barrels, crate and bench now stand out in front, clear of the bigger walls. The yard and the start of the street move forward with them.
- Inside, the old counter, shelves, workbench and stove are all still there, with more around them:
  - two rows of high shelves full of books and boxes up the back wall
  - a reading corner with two armchairs you can sit in, a little table and a rug
  - a long display table of the week's new toys
  - barrels of nuts by the right-hand window
  - more lamps to light the bigger room
- Bramblewick walks out through the new doorway when he goes home at night.

**A whistle for Bracken**
- If Bracken is your dog, whistle and he comes running to you from wherever he is, his stall in the Hollow included. If he is far off, or in another place, he comes running up the way you came.
- He stops beside you, wagging, ready to ride.
- To whistle:
  - **Keyboard:** G (it can be changed in Settings)
  - **Touch:** the Whistle button, which appears once he is yours
  - **Controller:** the left trigger
- You can't whistle from indoors, on the sky-lift, or on the glider. If Bracken belongs to someone else, or to nobody yet, the whistle tells you so.

## One gondola for everyone, riding the ram carriage, Tinderfold on the map (build 116)

**The gondola is shared**
- **Everyone sees the same car.** The pier, the sky-lift and Skyhaven are now one meeting place online. Players there see each other even across the three: from the pier you can watch a friend ride up in the gondola, or stand on Skyhaven's dock waiting for it.
- **How it stays in step.** Each trip is announced to everyone: when it set off (on the clock every game shares), where from, and where to. Every game works out the car's position from that alone, so everyone sees it in the same place on the cable.
- **Riding together.** Whoever walks in or rings the bell sets the trip going for everyone. If you walk into the car while a friend is in it, you ride together. Someone who arrives later picks up where the car is from the last trip.

**Riding the ram carriage**
- **Getting on and off.** Walk up to the tailboard of Gorse Tallow's carriage when it stands still, at the shed in Tinderfold or at Emberdeep's west door, and press the pick-up key to climb on. You sit on the left bench by the driver's box. A moment later Gorse shakes the reins and Tuff and Biscuit pull you to the other end. You can climb down before it sets off; once it stops, move to step down beside it.
- **When it runs.** Gorse takes you whenever he's awake (from about 6:20 in the morning to 9:20 at night), not just on the workers' morning and evening runs. On those runs you ride with the miners.
- **It's shared too.** A trip anyone starts is shared with the other players on the road, just like the gondola.

**Tinderfold on the map**
- The first time you come within sight of Tinderfold, it's added to your map with its own marker. Click the marker to travel back there any time, to the street by the well, with the carriage shed at the far end.

## The sky-lift is a real car, fast travel, spinning your elf, talking mouths (build 115)

**The sky-lift**
- **It rides the cable.** The cable now hangs between a steel tower beside the pier end and a gantry on Skyhaven's dock, sagging a little in the middle. A haul rope runs above it. The gondola hangs from a wheel that rolls along the cable, so it is always on the line, and the wheel turns as it goes.
- **One car, and you walk on.** There's no fade any more:
  - **Car at your end:** walk off the end of the pier, or the dock, through the gap in its rail. A moment after you're standing inside, it sets off.
  - **On the way:** you can walk about in it while it travels.
  - **At the other end:** it waits, and you walk out.
  - **Car at the other end:** walking to the edge rings the bell, and you watch the car come to you.
  - **Workers' runs:** the people who work in Skyhaven use the same car on their morning and evening runs.
- **Bigger and taller.** Wide enough for several to stand in, and tall enough for a grown elf, with a lamp under its roof.

**Fast travel to places you have found**
- The map now shows only the towns you have been to. A new player sees only Thistledown Hollow.
- The first time you reach a town, it appears on your map. From then on, the map has a **Travel to …** button for it. Hearthmoor works the same way.
- Everywhere new, you still walk to first.
- Existing players start with the Hollow, plus every town where they've finished a lesson, plus wherever they are now.

**Turning your elf in the creator**
- Your elf now stands still, facing you. Turn it yourself:
  - drag it with the mouse, or swipe it with a finger
  - hold the arrow keys or Q and E
  - push the right stick
- A flick keeps it turning for a moment after you let go.
- A VR hand will be able to turn it the same way (pinch and twist) once VR is in.

**Talking mouths**
- With voice chat on, your elf's mouth opens and closes with how loud you are speaking, and so does everyone else's whose voice you can hear. It follows loudness, not the actual words, as in most online games. When nobody is talking, each face goes back to its own expression.

## The satchel, held things, and Cliff Row (build 114)

- **The satchel is a real shoulder bag.** It used to sit inside your elf's body, with only two straps sticking out of the shoulders. Now it rides on the right hip, a little behind, where a swinging arm clears it. Its strap runs up across the chest, over the left shoulder and down the back. The bag has a flap, a brass buckle and rings for the strap.
- **The satchel button and picture show a satchel**, a flat shoulder bag with a flap and a long strap, instead of a backpack. The traveller's pack is still a backpack, because it is one.
- **Things people hold stay in their hands.** Since elbows went in, anything held (Durna's hammer, Marigold's trowel, Captain Tideknot's rope and the rest) was fixed to the upper arm while the forearm bent away from it. Each held thing now rides on the forearm with the hand, and the hand closes round it.
- **Cliff Row in Cogsworth: four different fronts.**
  - Gearwright's keeps its timber grid and turning gear.
  - Spindle's has a long timber balcony along the upper floor, with tall doors opening onto it and pots of flowers on the rail.
  - Ratchet's is dressed stone with a round-arched door and keystone, arched windows, brass pipes and ivy.
  - Verge's has a bay window, a brass-ringed porthole, and a great clock face over the door.

## Every house its own (build 113)

The twenty family houses in Thistledown Hollow, the Orchards, Fernwater, Cairnhold and Lanternreach used to be one house in different paint. Now no two in a town are alike, outside or in.

**Outside, six kinds of house, each its own size:**
- **Timber-framed:** the old two-storey house with dark beams (the Boughs').
- **Stone cottage:** stone walls, a steep roof with two dormer windows, and a chimney at each end.
- **Thatched cottage:** long and low, under a deep straw roof with a rounded ridge, and the chimney up through the middle.
- **Barn-roofed:** clapboard walls under a gambrel roof (two slopes each side), with a dormer.
- **Hip-roofed:** a square two-storey house with stone corners and a roof sloping on all four sides.
- **Saltbox:** two storeys at the front and one at the back, under one long back slope.
- **Towered:** the timber house with a round tower at the back corner and a pointed roof on it (Captain Tideknot's and Astra Vell's).

**Inside:**
- **Half the houses are laid out the other way round**, so the kitchen, stair and bedrooms sit on the other side of the hall. The people who live there sit by their own fire and sleep in their own beds either way.
- **A child's room only where a child lives** (Pip Willowby's house and the Ashcombes', where the other Pip stays). Everywhere else the second room upstairs is what that household would use it for:
  - a study with a bookcase, desk, globe and armchair
  - a sewing room with bolts of cloth, a spinning wheel and baskets of yarn
  - a guest room
  - a music room with a harp, a drum, a lute and a music stand
  - a storeroom of jars, sacks, barrels and crates
  - a painter's room with an easel, canvases and a workbench
- **A different game on every hall table:** draughts, chess, cards, dice, a jigsaw, a stones-and-pits board, knucklebones, a sea map with markers, a tea set, or just a vase of flowers.
- **What each house's description promises is really there now:**
  - Astra's star chart and sextant
  - rope on the pegs at Reef Cottage
  - the tide table with today circled at Ebb Cottage
  - Jory Wick's lamp glasses
  - Tamsin's walls of clocks, none agreeing
  - the wheel over Odette's hearth
  - Barnaby's labelled levers
  - the town clock's spare hands at Clementine's
  - Wren's plumb line
  - Una's fleeces and crook
  - Garth's mallet and stone dust
  - Tobias's map rack and chain
  - the half-mended net at Corwin's
  - Pell's river marks by the door
  - flour sacks at the mill house
  - Pip's crates
  - honey jars at the Honeywells'
  - pear baskets at the Applegarths'
  - bread rising at Maudie's

**Not yet:**
- Each household has its own second room, game and things about the hall, but the hall's built-in pieces are the same in every house: the candle wheel, settle, clock and wall hangings.

## Small animals get joints too (build 112)

- **Rabbits** have haunches, long hind feet and front paws. Each hop now runs the way a real one does: the hind feet push off together and straighten out behind, the front paws reach forward in the air, the front paws land first, and the hind feet swing past them to land in front, ready to push again. A rabbit sits with its hind feet folded flat under it, and a frightened rabbit hops higher.
- **Squirrels** have hind legs that fold under them and small arms held to the chest. They hop the same way across the ground and scrabble with all four feet as they dash up and down a trunk.
- **Crows** stand on legs that bend backward at the ankle, as a bird's do. The ankles dip a little as a crow shifts on its perch, and it tucks its feet back when it flies.
- **The heron** at Fernwater has long legs with the same backward-bending ankle. When it steps, it lifts one foot with the toes swinging up under it.
- **Frogs** sit on folded hind legs and kick them straight out behind when they jump.
- **Pheasants** tuck their feet up while they fly.
- **The beetle** has a knee in each of its six legs. It walks on two tripods (front and back legs on one side with the middle leg on the other), each leg sweeping back along the ground, then lifting and swinging forward.
- Other players see the same hops, perches and tucks.
- Not yet: the mouse, the hedgehog, the ducks and the birds that only fly overhead.

## Riding, hands, gaits and faces (build 111)

- **Sitting on Bracken.** The rider now sits down into the saddle blanket instead of hovering above it. Because Bracken is broad and an elf child is small, the thighs go out sideways across the blanket and the shins lie along its flaps, the way a small child sits a pony. This works the same at any height the player has grown to, and for other players seen riding him.
- **A bridle and reins.** Bracken wears a noseband, a strap behind his ears, cheek straps and a brass ring at each corner of his mouth. Leather reins run from the rings, under his jaw, along the sides of his neck and back to the rider's hands, with a little sag. They follow every step, and they are hidden when nobody is riding.
- **Hands with fingers.** Every person (the player, other players, every elf in every town and the Hollow's own folk) now has a palm, four fingers and a thumb. Each finger bends in two places, so a hand can relax, half-close or make a fist. Riders close their hands round the reins. Further than about 20 to 30 paces away, a hand turns back into the simple round mitten, so the extra detail costs nothing at a distance. This is the groundwork for grabbing things with your hands in VR.
- **Animal legs join the body.** Each leg of the deer, dogs (Bracken too), fox, badger and rams now starts with a rounded shoulder or haunch that sits inside the body, so the top of the leg no longer shows as a cut-off post.
- **Walk, trot and gallop.** Four-legged animals choose a gait by speed. A walk sets the feet down one at a time (hind, fore, other hind, other fore). A trot moves diagonal pairs together. A gallop lands both hind feet close together and then both front feet, and the body rises and falls once a stride. Gaits blend into each other as speed changes. Legs fold as they swing forward and straighten as they land, and the stride slows down and lengthens at speed rather than going faster and faster.
- **A startled deer bounds.** When a deer takes fright it first springs away in a few stiff-legged bounds, all four feet leaving the ground together and tucked up, before it breaks into a gallop.
- **Faces match their names.** The eyebrows were tilted the wrong way round, so Determined and Grumpy looked worried. Brows now tilt down toward the nose for Determined, Grumpy and Mischief. They sit a touch lower and are drawn darker so they show under the hair.

## Knees and elbows (build 110)

- **Your elf and other players** have a thigh, knee and shin in each leg, and an upper arm, elbow and forearm in each arm. Knees bend as each leg swings back and fold square when you sit or ride Bracken. Elbows bend more as the arm swings forward.
- **Every townsperson has legs now.** The long robes and dresses that went to the ground stop at the knee, with trousers or stockings and boots below. The Hollow's own people (Old Bramblewick, Fen, Marigold, Aldric, Hollis, Pip, Orrin and Maud) were made by hand, so they were given the same legs and elbows one by one.
- **People stride when they walk.** Anyone who walks anywhere, such as going home at nine, swings their legs and arms in step with how far they go. People carried along in the carriage or the lift do not.
- **Animals.** The deer, Bracken and the other dogs, the foxes, the badgers and the Tinderfold rams have jointed legs. Front legs fold back at the knee as the foot lifts. Hind legs are in three parts with the hock bent back, the way a real deer's or dog's are. The pheasants' legs bend that way too.
- **Not yet:** squirrels, rabbits, birds other than pheasants, and the beetles are unchanged.

## Skyhaven at full size, and the sky-lift as a commute (build 109)

- **Everything on the islands is 1.8 times bigger:** the islands themselves, the rope bridges, the Glider Guild, the Balloon Works, the Engine House, the wind tunnel, the launch rail, the balloons and the lift station. The three buildings have grown-elf doors and rooms 1.6 times bigger inside. The Guild's bell is on a low shelf at the front of its counter.
- **The gondola is grown-elf size too.** It still waits with its floor level with the pier at the bottom and with the deck at the top.
- **Two new people, both invented:** Corvin Gale keeps the Guild's counter, and Mabel Spoke sews the balloons.
- **Hours.** At nine Professor Quill, Fen, Pip, Corvin and Mabel walk to the lift station. The car goes up empty, waits, and brings the five of them down; you can see them in it. At 6:36 it takes them back up, and they are at their places a few seconds after it arrives. Overnight Skyhaven is empty. The Guild is shut at night.
- **Their homes are in Lanternreach.** Windward Row runs on west, and its four new houses are each different. Professor Quill's is a round stone house two floors high with a glider weathervane. The Gales' is shingled, with a barn-shaped roof, dormers, a porch, a kite on a pole and a sandbox. Fen and Pip share a workshop with big doors, their rooms above and a glider frame in the yard. Mabel's is a long whitewashed cottage under a thatched roof. Inside, each is laid out differently, and only the Gales have a child's room.
- **Lark Gale**, Corvin's daughter, plays outside their house by day. She is Lanternreach's only child.
- **Knock on Professor Quill's door** in Lanternreach at night and she will still teach.
- **Not yet:** you can't ride down with them on the evening run (if you take the lift yourself, it is yours). The lesson that flies you home is unchanged.

## Emberdeep and Tinderfold (build 108)

Emberdeep is a place people work, not a place they live, so its people now live in a village of their own and ride to work.

- **Tinderfold** stands on level ground down the hill from Emberdeep's west door, at the end of a lane off the Long Road (a signpost marks the turning). It has one street, a well with a bench either side, lamps, and a ram shed.
- **Four houses, none alike.** Durna's is low and stone, with a steep slate roof, a chimney like a tower and her own smithy under a lean-to. Brokk's is brick below and plaster above, jutting over the street, with a red tile roof, a kitchen wing and a fenced yard with a swing. Hilda's is tall and narrow, three floors of pale stone with a hipped roof, a bay window and a rock garden of labelled ores. Odwin's is a long log cabin with a deep porch and a lantern between every post.
- **Inside, each is laid out differently.** Durna has one great room with her bed in an alcove and her anvil for a table. Brokk has two bedrooms, his and his daughter's, a kitchen and a half-played game of dominoes. Hilda has shelves of ore samples to the ceiling, a desk with scales, a ledger and an abacus, and her bed behind a curtain. Odwin has the fire at one end and his bed at the other, a lamp bench, a cat and a game of patience laid out for one. Only Brokk's house has a child's room, because only Brokk has a child.
- **Tilly Cinder**, Brokk's daughter, plays in their yard by day and goes home at nine. She is the only child in Tinderfold, and you can see her.
- **The carriage.** Gorse Tallow drives a long open cart pulled by two big rams, Tuff and Biscuit. A little after half past six it takes everyone up the road to Emberdeep's door, where it waits all day. Just after nine it brings them home. You can see the four of them sitting in it. Gorse will talk to you, but it is workers only for now.
- **At night the cave is empty.** At nine Durna, Brokk, Hilda and Odwin walk out of the west gate to the carriage. They are back at their places a few minutes after it arrives in the morning. Knock on Durna's door in Tinderfold and she will still teach.
- **Emberdeep's west gallery** has no houses now. Along its north wall are four ore bins, one for each seam.
- **Not yet:** you cannot ride the carriage. Tinderfold is not on the map. Inside, the houses are one floor each.

## Cogsworth at full size

- **The town now runs 170 units further west** along the foot of the cliff (380 wide, was 210). The gate, the lane, the square and water clock, the lift, both works, the thermometer, the hill and every lesson spot are where they were.
- **Cliff Row:** four houses built into the cliff, as the two works are. Each is a full two-storey timber front 34 wide and 24 tall with a grown-elf door in the rock, a turning brass gear over it and a brass flue up the cliff, and the same full house inside as everywhere else: no. 1 Gearwright (Tamsin), no. 2 Spindle (Odette, who builds the bicycles), no. 3 Ratchet (Barnaby, who keeps the lift), no. 4 Verge (Clementine, who keeps the town clock).
- **The town clock** stands in the new square: a stone tower 45 high with four faces. Its hands show the real time, the same clock the whole game runs on.
- **Also new:** plane trees in stone tubs, benches you can sit on, full-height lamps, and the grown-elf gate with the big gear turning above it.
- **Hours.** Both works shut at 9 and open at 7. Tamsin, Odette, Barnaby and Clementine walk home to Cliff Row at 9, sit by the fire till 10, then bed. Knock and whoever is in answers; Tamsin will teach at any hour. A lesson under way keeps Tamsin at her place.
- **Not yet:** the two works are the size they were. The houses have no gardens, being in a cliff.
- How it works: `CGW` holds the west edge and the lane; `GHOMES` the houses (each "house" is a point inside the cliff, so the same walking-home code works); `cogHB` their doors. The crag in open country behind the town (`cogLand`) now stands tall as far west as the row does.

## Cairnhold at full size

- **The town is now 448 wide** (it was 120), still 150 long, still up on its plateau with the gorge across it. The lodge, the stone yard, the pasture, the odd field, the cairn ring and the cisterns are exactly where and what size they were, because the lessons measure them.
- **Rim Row** is a lane along the near side of the gorge, with two houses east of the old town and two west, all looking across the gorge: no. 1 Ashcombe (Wren, with Pip lodging while he learns the chain), no. 2 Fellwick (Una, who keeps the pasture), no. 3 Scree (Garth, the mason), no. 4 Marl (Tobias, who carries the other end of the surveyor's chain). A path leaves the main lane on each side and runs up to it. There is a low drystone wall along the rim in front of the houses; the rim by the old town is as open as it was.
- The houses are on the near side on purpose: the bridge is not there until a learner builds it, and nobody should have to cross a gorge to get home.
- **Both gates** are the grown-elf kind, and there are full-height lamps on the lane, the two paths and Rim Row.
- **Hours.** The Lodge shuts at 9 and opens at 7. Wren, Pip, Una, Garth and Tobias walk home at 9, sit by the fire till 10, then bed. Knock and whoever is in answers; Wren will teach at any hour. A lesson under way keeps Wren and Pip at their places.
- **Not yet:** the Lodge itself is the size it was.
- How it works: `CRN` holds the width, the lane and the two paths; `CHOMES` the houses; `cairnHB` their walls and fences. A person can now have their own way onto the lane (`gx` on a `FOLK` entry).

## Fernwater Crossing at full size

- **The town is now 220 wide and 320 long** (it was 92 by 120). The old town, the river, the bridge, the dock and every lesson are where they were. What is new is the far bank.
- **Heron Row** runs east and west across the far bank, with four houses on their own plots, the same houses as Lantern Lane: no. 1 Tideknot (Captain Oriel and his sister Hester, the rope-maker), no. 2 Crumb (Maudie the baker), no. 3 Sedge (Corwin, who makes the nets and catches the fish), no. 4 Reedmace (Pell, who keeps the bridge and reads the river gauge). You can go into all four. The path from the bridge runs on between them to the Cairnhold gate.
- **Both gates** are the grown-elf kind: stone pillars, a lintel and a board that reads one way going in and the other going out. The rail fence by the south gate is taller and runs the width of the town.
- **The Ferry Office** is rebuilt at the size of the room inside it and stands east of Nets & Lines, with a path from the dock. The five little huts on stilts are gone: nobody could go into them.
- **Lamps** on the lane, the bank and the far bank are full height and light at night. There are alders and willows on both banks.
- **Hours.** The bakery, the rope shop and the net shop shut at 9 and open at 7, with honesty boxes for food. At 9 Oriel, Hester and Corwin walk home over the bridge; Pell has the short walk; Maudie leaves the bakery once you are out of it. By the fire till 10, then bed. Knock and whoever is in answers; Oriel will teach at any hour. A lesson under way keeps Oriel and Maudie at their places.
- **Not yet:** the money-changer still has nobody at the counter. The four shops are the size they were (already grown-elf height, but smaller than the houses).
- How it works: `FRN` holds the width and the lane; `FHOMES` the houses; `fernHB` their walls and fences; `fernY` the height of the bridge and dock for anybody walking the paths. `FTOWN` tells `folkInit` each town's houses, lane and ground, so the next town is one more entry. `townGate` builds a gate in any town.

## Bracken, the riding dog, and the Hollow Stables

- **The Hollow Stables** stand south-west of the old oak: an open-fronted shed with three stalls. The middle stall is Bracken's, and the board over it says whose dog he is.
- **There is one riding dog in the whole game, for every player together, and he is owned, not hired.** The first player to walk up and say yes owns him from then on (kept on the server in `game_mounts`; one row, one owner). Everyone else can greet him and is told whose he is. How further dogs are won or change hands is not decided yet.
- **Riding.** Stand beside him and press the ride key (R, the Ride button, or up on a controller's D-pad), or press the pick-up key on "Ride Bracken". He does 21 at a trot and 32 at a gallop; the bicycle does 17.5 and 25, running on foot 14. Press ride again to get down.
- **He stays where you leave him.** Get down and he waits on that spot, in that town or out on the road, until you come back, even after you close the game. Go indoors or take the ferry while riding and he waits outside. The board at the stable has "Put Bracken in his stall", which brings him home from anywhere.
- **Other players** see you riding him, see him waiting where you left him if they are in the same place, and see his stall empty while he is out.
- **Not yet:** he does not follow you on foot or come when called; he cannot be fed, groomed or renamed; a bicycle rider still looks like a walker to other players; the two side stalls are empty.
- How it works: `mount` is `'bike'` or `'dog'` and `setRiding` handles both, so another kind of mount is one more value. `rdog` is the one dog; `dogTick` decides each frame whether he is under you, where you left him (`save.dog`), under another player, or in his stall. Server: `game_mounts_list`, `game_claim_mount`, `game_mount_home`.

## Mallow Orchards at full size, the Library Tower, and books

- **Mallow Orchards is built for grown elves now.** The town is about twice as wide and three times as long as it was (218 by 340 units), inside a tall hedge. The two gates are stone pillars seventeen units high with a lintel and a board that reads one way going in and the other going out. The apple and hazel rows, Hollis, Pip, the press and the crates are where they were, so every lesson starts where it always did.
- **Quern Mill** is a stone tower 50 units across and 46 to the eaves, with a porch and a door a grown elf walks through, and a treadwheel 28 units across on its west side with four mice in it. Inside it is one room 34 by 34: the stones in the middle, the big gear and the axle from the wheel overhead, the meal bin, grain and flour sacks, cider barrels, and a counter with the shop bell.
- **Windfall Lane** runs east and west beyond the mill, with four houses on their own fenced plots, the same houses as Lantern Lane: no. 1 Quern (Hollis), no. 2 Willowby (Pip and his mother Nell, the cooper), no. 3 Honeywell (Bram, the beekeeper), no. 4 Applegarth (Tansy, who picks the pears). You can go into all four.
- **New places to walk:** a pear orchard of four rows of six, the bee yard (six skeps in a strip of flowers, with bees over it by day), and the cooperage, an open shed where Nell makes the barrels and crates. Nell, Bram and Tansy each stand at their work by day and will talk.
- **Hours.** The mill shuts at 9 and opens at 7, with an honesty box for apples and cider in between. At 9 all five walk home along the lanes; they are by the fire till 10, then in bed (Pip in the small bed). Knock at a door and whoever is in answers; Hollis will teach at any hour. A lesson under way keeps Hollis and Pip at their places.
- **The Library Tower** in Hearthmoor is now 70 units across and 124 to the parapet, wide enough to hold the reading room you walk into, with buttresses, a porch and a spire.
- **Books can be read.** Twelve are out in the Library: four lying on the reading tables and eight on the two low shelves (stand at a shelf and choose). A book opens to its cover, then turns page by page, forwards or back. Each is five pages of true things written for a young reader: plants, knots, the water wheel, the Moon, bread, bees, rain, place value, the Long Road, rocks, seeds, and finding north. The game remembers which you have finished and ticks them on the shelf.
- **Not yet:** the other five towns are still at their old size. Books cannot be carried out of the Library, and there are none anywhere else. The tall shelves and the tower's upper floors are not open.
- How it works: `ORC` holds the town's bounds, `OHOMES` its houses (built by the same `buildHouse` as the Hollow, which now takes a scene and solid lists), `orchBoxes` the straight-sided things. `FOLK` now covers both towns; `folkInit` gives each person a route, an indoor figure and a bed. Books are `BOOKS`, read with `readBook(id)`; shelves call `shelfBrowse`.

## Inside Hearthmoor, your room, and storage

- **Every door in the city opens.** Walk straight at a door and you go in (walking along a wall past one does nothing). Coming out puts you back on the step you went in by.
  - **Rowan, Hazel and Alder House** on South Street are the lodging houses. See **A room of your own** below.
  - **The eight trade halls** each have a counter, the town's goods on the shelves, crates and sacks, benches, a keeper who will talk, and the bell on a barrel. The bell outside each hall still works too.
  - **The Guildhall** has the long table with a chair for each town (you can sit in any of them), their banners, and Odo Ledgerwood the clerk.
  - **The Bank** has a counter behind a brass grille, the vault door, and a wall of strongboxes; the brass one is yours and holds 40.
  - **The Library Tower** has books to the ceiling, four reading tables with stools, and Sage Inkwell the librarian. The books cannot be read yet, and she says so.
  - **The Hearthstone** is the city's inn, on South Street: the same room as the Sleepy Beetle, kept by Bram Kettleby, selling supper.
  - **The terrace houses** all open onto one of two shared house interiors (the Hollow's house plan, in two colourings), named for the street the door is on. Nobody is in.
  - The four colleges and the warehouses give a reason at the door and stay shut.
- **Storage.** A place to keep things is a `storeSpot`. Walk up to one and open it: the satchel panel shows what is in it on one side and what you carry on the other, with **Put in** and **Take**. What you leave stays there, on this device and in the cloud save.
  - Yours: the chest (20), wardrobe (12, things to wear only), bookshelf (10) and desk drawer (6) in your room, wherever that room is; the strongbox at the Bank (40); and a travellers' chest by the door of each inn, the Sleepy Beetle in the Hollow and the Hearthstone in the city (20 each). So there is somewhere to empty a full satchel at both ends of the road.
  - Each of yours has a lock you can turn on and off. It is locked unless you unlock it. With only one player in the world the lock keeps nobody out yet; it is there for when rooms can have visitors.
  - Other people's: the wardrobe and chest upstairs in each Hollow house and in the city houses belong to the family, and are locked. Looking at one tells you whose it is.
- How it works: `CITY.doors` lists every door with the room it leads to (`inside`) or the reason it does not (`msg`); `cityCollide` notices you walking at one and `cityDoor` takes you in. City rooms are ordinary rooms (`makeRoom`) whose way out is set to whichever door you used. `openStore`, `storeOf(id)` and the `take`, `put` and `lock` actions are the storage; the contents are in `save.stores`.
- Not yet: nothing can be put on a table or a shelf where you can see it (storage is a list, not objects in the room), and the letter racks are waiting for the post.

## Accounts

- **Nobody plays without an account.** Signed out, the title screen shows only the sign-in box. This is so every elf, room, chest and lot has an owner, and so progress lives in the account and not on one device.
- **An elf belongs to its account, not to the device.** The save kept on a device is stamped with the account it belongs to (`save.owner`).
  - **Signing out** sends the save to the account and then removes it from the device, along with which room is yours. If the cloud cannot be reached it refuses to sign out, and says why, so nothing is lost.
  - **Signing in** as a different account never picks up an elf left behind by another. This was the fault where a second account found "Jake" waiting on the device and offered to replace him.
  - **An elf from before saves had owners** (one made on a device before this change) is taken into the first account that signs in there, but only if that account has no elf of its own. If the account already has one, the account's elf is used and the leftover is dropped.
  - The same account on two devices still works as before: whichever save is further along, or newer, wins.
- **Making an account asks the year of birth first** (unless the device already knows it). Under 13, the form asks for **a parent's email** and says a parent must make the account; 13 and over use their own. The year, and whether it was a parent's address, are stored with the account, and a new device picks the year up from the account on sign-in.
- **Two things to check in the Supabase dashboard, under Authentication**, because the game cannot see or set them:
  1. **Confirm email is switched on.** With it off, an account works the moment it is made and the parent's link means nothing.
  2. **Site URL is the game's address** (`https://hardingroup.github.io/HGP/`), so the link in the email comes back to the game.
- **A parent's email with a link is the usual first step, not the whole of it.** US children's privacy law asks for more than that before a child under 13 can share anything with other people. That still needs proper advice before the game is offered to the public.
- **Testing without an account:** set `long-road-dev` to `1` in the browser's local storage for the game's address. Nothing in the game sets it.

## Other players, the age question, and parental controls

- **Saying things.** Press T (the Say button on a touch screen, right on the D-pad of a controller) for a list of sixteen ready-made phrases; "Gestures…" in the same list gives Wave, Cheer, Bow, Dance and Point. A phrase appears in a speech bubble over your elf for about five seconds, on your screen and everyone else's in that place, and as a line of text for anyone within 90 units. Only the number of the phrase is sent, never words, so nobody can be sent anything that is not on the list. The parental control "Ready-made phrases and gestures" turns sending, seeing and the Say button off. The lists are `PHRASES` and `EMOTES`.
- **Voice, for testing.** Press B (the Voice button on a touch screen, left on a controller's D-pad) and allow the microphone. Any elf in the same place who has also switched voice on can then hear you and be heard: full volume within 25 units, fading to silence at 150. A chip at the top of the screen shows it is on and how many are connected; tap it to turn voice off. The sound goes directly between the two games (WebRTC); the Supabase channel only introduces them, and nothing is recorded or stored.
  - **Who may use it:** only an elf whose voice rule is "Anyone nearby". That is the default for a birth year that makes an adult. For under 13 it is off and only a parent can change it behind the passcode. For 13 to 17 the default is "Friends only", which counts as off until there is a friends list. An elf who may not use voice has no Voice button and is never connected to anyone.
  - **Limits, known:** age is whatever year was typed, so this is for testers you know, not the public. There is no relay server, so two players on some networks (many phone networks, some offices) will not connect; two devices on ordinary home internet usually will. Each player can see the other's internet address, as with any direct call. There is no way yet to mute one person, and nothing shows who is speaking. It needs the real site (https); it will not work inside the Claude preview.
- **Everyone sees the same animals.** Every game makes the same 33 animals in the same places and colours (they are made from a fixed seed). In each place one game runs them: the one whose id sorts first, among everyone in that town, or among elves within about 400 units of each other in open country. It sends the animals that have moved, five times a second, inside its ordinary position message; the other games draw them a quarter of a second behind, gliding, with legs and wings moved to suit. If that game leaves, the next takes over from where the animals are. Animals react to whichever elf is nearest, not only the one on your own screen. Butterflies, the flocks overhead, the bees and the lamplighter keep time by the clock, so they match without anything being sent.
- **Their beetle too.** If another elf has a beetle walking with them, you see it beside them at its real size. Not yet: its name, an elf riding it, or a parked one.
- **Both games must be the same build** (the number on the title screen) for the animals to match; a different build's animals are ignored.
- **Other elves glide.** Each game sends its position ten times a second (and once more when it stops). Everyone else is drawn 180 milliseconds in the past, sliding between the two positions either side of that moment, so they move evenly instead of in steps; their legs swing by how fast they are really moving. `SEND` and `BEHIND` in `NET` are the two numbers.
- **Seeing each other.** Players in the same place see each other's elves walk about, with the elf's name over its head and its own colours, hair, hat, face and height. "The same place" is a town, the open land, or the inside of one building. It is on unless switched off in Settings.
  - How to try it: open the game on two devices (or two browser windows), go to the same town on both, and walk. Settings, under **Other players**, says whether it is connected and how many others are here; if it cannot connect it says why.
  - **This has not been run against the real server.** It was built and tested against a stand-in, because the place it was built in cannot reach Supabase. The first two-device try is the real test. If Settings says it could not connect, the likeliest cause is the Realtime service or public channels being switched off for the project in the Supabase dashboard.
  - What is shared is only where the elf is and what it looks like. Lessons, coins, the satchel, chests and the beetle stay your own. Other players' elves cannot be bumped into or spoken to yet.
  - Limits: it sends a position five times a second while moving, which Supabase's free plan will carry for a dozen or so players at once, not a crowd. Up to 24 others are drawn. The channels are open to anyone holding the game's public key, so a determined person could send false positions; a proper game server ends that. An elf's name is shown to whoever is nearby exactly as it was typed (letters, numbers and spaces only, 16 at most).
- **The year you were born.** A new game asks for it, with no hint of what any answer does. An elf made before the question existed is asked once on Continue. The answer is kept on the device apart from the elf's save, so starting a new elf does not ask again; after that only a parent's passcode can change it.
  - Under 13: voice chat off, and only a parent can turn it on. 13 to 17: friends and party only. 18 and over: anyone nearby. Age is counted as the youngest it could be (the birthday may not have come yet this year).
  - **Voice chat and ready-made phrases are not built.** These settings are stored now so every player is sorted from the start.
- **Parental controls** are at the bottom of Settings. A parent sets a passcode of 4 to 8 digits; from then on the page is locked, and the passcode is needed to see or change voice, phrases, seeing other players, or the birth year. It locks again when Settings is closed.
  - **Before this is offered to the public:** a passcode typed on the child's own device is not verified consent from a parent. US children's privacy law (COPPA) wants a method that shows it really was a parent before a child under 13 can talk to others. That needs deciding, with advice, before voice goes live.
- How it works: `fam` is the family record (`long-road-family-v1` on the device) and `renderFam` draws its page. `NET` is the connection: it speaks Supabase Realtime's own protocol over a WebSocket, joins a channel named for the place, and sends and hears `p` messages. `makeKid()` now builds an elf (yours, and each of the others), and `applyLookTo` dresses one.

## A room of your own, and a lot

- **Each account has its own room.** There are three lodging houses with four rooms each, twelve places in all. Signing in asks the Guild's register (the database) for your room; the first time, it gives you the first free one and that room is yours from then on. Jake's account already holds Room 2 of Rowan House. 
  - In your house your door stands open and has a "yours" plate; the other doors are shut. In the other two houses every door is shut and the hint tells you where your room is. Each house has its own warden, who will tell you the same.
  - If all twelve are taken, a new account is told so honestly and gets no room. More houses, or upper floors, are the fix.
- **The rooms are not all alike.**
  - Rooms are built in mirrored pairs: Rooms 1 and 2 share a wall, and so do 3 and 4. Each room's **washstand** stands against that shared wall, back to back with its neighbour's, with one lead pipe up the wall for the two. So Room 2 is Room 1 turned round: bed on the other side, wardrobe and shelf on the other side.
  - The **end rooms** (1 and 4) are wider, have a window in the outside wall, and an armchair.
  - In **Hazel House** the middle two are one long **shared room** for two: two beds, two desks, two of everything, and a table with two stools between. Either occupant's door opens it.
  - Every room has a bed, a desk under a window with a stool, a chest, a wardrobe, a bookshelf and the washstand (you can wash at it).
- **Your colours.** Stand on your rug and choose a colour for the quilt and for the rug, six of each. Signed in, the choice is kept with your room.
- **Chests are kept under your account.** As well as travelling in the cloud save, the contents of every store are written to a table of their own, with whether you have locked it. The database will show a store to its owner always, and to anyone else only if the owner has unlocked it. Nothing in the game lets one player open another's yet; this is the rule that will be enforced when it does.
- **Garden Lots can be claimed, one to each account.** Stand at the post by a lot's gate. If it is unclaimed and you are signed in and have no lot, you can claim it; the sign then carries your elf's name, in green for you and brown for everyone else. A lot cannot be swapped or given up yet, and nothing can be built on one. The signs are read from the database when the city comes into view and every two minutes after.
- In the database: tables `game_rooms`, `game_stores` and `game_lots`, all with row-level security, and the functions `game_my_room`, `game_room_colours`, `game_stores_put`, `game_store_peek`, `game_lots_list` and `game_claim_lot`. The functions decide everything from the signed-in account, never from what the game says about who is asking.
- In the game: `LODGE` and `LROOMS` are the houses and their rooms, `myRoom` is yours, `lodgeSync` opens and shuts the doors, `lodgeFetch` asks the register; `lotState`, `lotsFetch`, `lotsApply` and `lotPick` are the lots.
- Tested: the database functions were run as two different accounts inside a transaction that was then undone (rooms handed out in order, a second claim refused, a taken lot refused, a locked chest refused to a stranger and an unlocked one shown). The game side was tested with the answers fed in by hand, because the place it was built in cannot reach Supabase. **The two have not been run joined together.**

## Things, coins and trade

- **The satchel** (`I`, the Bag button, or a click on your coins) lists what you carry, with tabs for food and drink, clothes, tools and goods. Pockets hold 6; the satchel from lesson 1 holds 20; a traveller's pack adds 20; a beetle walking with you carries 20 more. Heavy things (flour, iron, coal, a clock) take the room of 3.
- **Things lie about.** Berries, mushrooms, flowers, herbs, feathers, pine cones and acorns sit in small clusters beside every road, about every hundred paces; shells near the Lanternreach coast; the odd ember crystal near the mountain doors. There are acorns under the Old Oak, berries by the bushes and windfall apples in the orchards. Walk up and press the pick-up key. A picked thing comes back after about twenty minutes. The wild ones are made as you come near and removed behind you, so the long roads cost nothing.
- **Shops.** A brass bell marks every counter (on a barrel where there is no counter). The pick-up key there opens the shop: what it sells on the left, what you carry on the right, and it will buy anything. Where nobody stands behind the counter it is an honesty dish.
  - The Hollow: the Village Stores (nut mix, acorn toys, the pack), Fen's stall (jam, tunic dyes), the potting shed (seeds, seedlings, straw hats), Bough's woodshop (walking stick, whistles).
  - Orchards: Quern Mill (apples, cider, flour). Fernwater: bakery, Nets & Lines, Tideknot Ropes. Cairnhold: the Lodge. Cogsworth: the Clockworks and the Bicycle Works. Emberdeep: Durna's ore bench. Lanternreach: the Chandlery. Skyhaven: the Glider Guild.
  - Hearthmoor: each trade hall sells everything its town makes; the market stalls sell food, hats, dyes, seeds and tools.
- **Prices are a rule, not a table.** Every thing has one worth. A shop in the town that makes it sells it for that; anywhere else (Hearthmoor included) it costs half as much again. Selling: wild things fetch their worth everywhere. Made things fetch 60% at home and 15% more for every town along the road you carried them, up to 150%; Hearthmoor pays a flat 120%. So buying where a thing is made and carrying it far is how you earn, and buying and selling in the same place always loses.
- **Food and drink** give back stamina (a pie and smoked fish mend a little health too). There is no hunger bar; you cannot eat when you are already full.
- **Clothes**: six hats (straw, wool, flat cap, miner's helmet, sou'wester, flying cap) that you wear from the satchel, and five dyes that recolour your tunic (one jar, one tunic).
- **Tools** work by being carried: trail boots make running cost a quarter less stamina, a walking stick brings stamina back 30% faster, the pack adds room, and the lantern is a real light you turn on and off.
- Lesson 2 no longer touches your purse: the jar money is Bramblewick's coin plus the two you earn at the stall, so coins you have made yourself are safe.
- Saved with everything else, locally and in the cloud. When the same lessons are done on both sides, the newer save now wins, so spent coins stay spent.
- On a short phone screen held sideways the control buttons used to be pushed off the bottom while the help panel showed; they now always stay on screen.
- On a controller: D-pad down opens the satchel; D-pad or left stick moves the highlight (up and down by row, left and right along a row), A presses, LB and RB change tab, B closes. X at a bell opens the shop the same way.
- Not built yet: seeds cannot be planted, there is no furniture, and shops never run out.

## Teachers, placement and travel

- **A lesson begins with its teacher.** Each town has one: Old Bramblewick (the Hollow), Hollis Quern (Orchards), Captain Oriel (Fernwater), Wren Ashcombe (Cairnhold), Tamsin Gearwright (Cogsworth), Master Durna (Emberdeep), Astra Vell (Lanternreach) and Professor Quill (Skyhaven). Walk up and press the pick-up key (the button says **Speak**). The teacher offers your next lesson there, any finished lesson again, or a test.
- **"Test me: I know this already."** Five multiple-choice questions on that town's arithmetic, drawn fresh each time from a bank of six to eight kinds with new numbers. Four right and every lesson in that town is marked known, and you are paid two coins for each lesson you did not need. Fewer than four and nothing changes; you can ask again whenever you like.
- **"I am working on..."** in the creator (counting and adding, up to calculus). Pick anything past the first and the game does not open with the acorn spill: Bramblewick offers the test at once, or lesson 1 if you would rather.
- **No fast travel.** The map shows where things are and which lessons are done, but its lesson list no longer starts anything, and a lesson name typed into the address bar is ignored. You get to a town by walking, cycling or the ferry. (The master account keeps its clickable list and Tester buttons. For testing, `localStorage['long-road-dev'] = '1'` lets address-bar lessons through.)
- Starting a lesson from its teacher still reloads the page and sets you at that lesson's starting spot inside the same town.
- The bridge at Cairnhold is built if you pass Wren's test, as it is when you finish her last lesson.

### Science in the Hollow

Marigold Fernsby teaches it, at the nursery. Speak to her between lessons. Each lesson sends you to real things standing about the village, then asks what you found out; a wrong answer gets a reason and another go. Two coins each. Lessons start where you stand, with no reload, and "Stop for now" leaves one cleanly.

1. **Alive, once alive, never alive.** Judge eight things round the village: the Old Oak, the berry bush, a stone, toadstools, a fire in a brazier, dry seeds on a tray, the barrel by the shop, the lantern post. (The fire and the seeds are the traps.)
2. **What a plant needs.** Four pots. You decide which gets sun and water, sun only, water only (under a crate), or neither. A week passes and each pot shows what its treatment did. Then: which pairs make a fair test.
3. **Which part do we eat?** Six plants in a kitchen bed: carrot, celery, lettuce, broccoli, tomato, peas. Root, stem, leaf, flower, fruit or seed.
4. **Egg to frog, egg to butterfly.** Four stones round a pond and four posts: touch each life's stages in order.
5. **Who lives here?** A burrow, a rotting log, a nest in the Oak, the pond. Then food chains, and herbivore, carnivore, omnivore.
6. **Reading the weather.** A thermometer, a rain gauge, a weather vane and a shadow stick, each with a reading to interpret. Then the seasons.
7. **What things are made of.** With Aldric: six things on a bench by a tub, float or sink, guessed before tested. Then density and choosing a material for a job.
8. **Pushes and pulls.** A handcart on a measured lane: the same push empty, loaded, and on long grass. Then friction and gravity.

- The props stand in the Hollow all the time (a pond in the south-east, a weather station by the south fence, a test bench south of the woodshop, a cart lane in the north-east corner, and so on). Each was placed by a search for clear ground, off the paths and away from houses.
- Not built yet: no placement test for science, no science past the Hollow, and the science list on the map only shows what is done.

### Reading in the Hollow

Fen Dewberry teaches it, at her stall. The eight lessons follow the order the science of reading sets out: hear the sounds, learn letters for them, blend, then read for meaning. Nothing asks a child to guess a word from a picture; the pictures are the answers, and the word is what gets read.

1. **The sounds at the start.** Ears only: hear a sound, pick the thing that starts with it. Then: do these two words start the same?
2. **Six letters: s a t p i n.** Meet each letter with its sound, then hear a sound and find its letter.
3. **Pushing sounds together.** Read jar labels (pin, tap, pan, tin, ant, nap) and pick the picture. "Sound it out" plays the sounds one by one. Then hear a word and find its label among near-misses (pin, pan, pit).
4. **More letters, more words.** Twelve more letters, then three-sound words with every short vowel.
5. **Two letters, one sound.** sh, ch, th.
6. **The silent e.** kit and kite, pin and pine, cub and cube.
7. **Letters to deliver.** Four unsigned letters. Read each and carry it to whoever it describes: Bramblewick, Marigold, Aldric, or the Old Oak.
8. **The fox and the jam.** A five-page story to read alone ("Read it to me" is there if needed), then four questions on what happened and why.

- **Everything is spoken.** A child who cannot read yet hears every instruction, every answer's name (the small speaker beside each picture), and what was wrong with a wrong answer.
- **Sound comes through one door.** The game plays `audio/<name>.mp3` if that file sits beside `index.html`, and otherwise uses the browser's own voice. So recordings, or an AI voice, can be added one file at a time with no code change. `audio/README.md` lists every name: 47 files covering the 44 sounds, the optional words, the story pages.
- Until the sound recordings exist, a single sound is spoken as "the first sound in sun". That is a stand-in, not good phonics; the lessons are not ready for a real child until at least the 25 sound files used here are recorded.
- Not built yet: the child is never asked to read aloud (no listening), there is no handwriting or spelling, and no reading past the Hollow.

### Things anyone can earn

- **The beetle.** A grub lies in the compost heap at the far corner of the nursery. Bring it 3 field mushrooms and 2 pine cones (both lie beside the paths and roads), and it pupates, hatches and is yours to name. Lesson 10 still gives one too.
- **The bicycle.** The Bicycle Works in Cogsworth builds one for 2 brass gears, 1 bicycle bell, 1 coil of rope and 30 coins. Gears and bells are made in Cogsworth; rope comes from Fernwater. Lesson "Your bicycle" still gives one too; passing Tamsin's test does not.

### The ferry

- A bell on the Fernwater ferry dock, and one on the western pier at Hearthmoor. Ring it, pay 4 coins, and the barge carries you down the Fern, across the lake and alongside the pier (or back). You stand on the deck and can look about; the beetle rides with you. A little over a minute.
- The ride itself is continuous through the open world. There is a fade when the barge casts off, and one when it comes back into Fernwater.

## Companions

- Walk up to your beetle and press the pick-up key (the big button says **Speak**) to give it an order: **Stay here**, **Go home**, **Open the baskets**, or, once it is waiting, **Follow me**.
- **Stay**: it waits exactly where it stands, in whatever town, room or stretch of road that is, for as long as you like. It is still there after you save and come back.
- **Go home**: in the Hollow it walks to the compost heap where you found the grub. Anywhere else it walks off, and is at the heap the next time you are in the Hollow.
- **Carry**: its baskets hold 20, but only while it walks with you. Leave it behind and your satchel is 20 smaller until you fetch it.
- **Whistle** (in the satchel, when it is not with you) calls it if it is waiting in the same place as you. From anywhere else it cannot hear; the satchel says where it is.
- Inside the code a companion that follows is moved by everything that moves you; one that waits is parked in its place and nothing touches it. Only the beetle exists so far, but the orders are not written for it alone.

## Title screen and your elf

- The game opens on a title screen with the live Hollow turning slowly behind it: Continue (when there is a save), New game, Lessons and map, Settings, and cloud sign-in.
- New game opens the creator: a name (up to 16 characters, "Elfling" if left blank) and colours for skin, hair, hat and tunic, shown on your elf as it turns. Begin starts the first lesson in Bramblewick's shop.
- One elf per player. If there is already progress, Begin warns what will be lost and must be pressed a second time; it then wipes the save (and the cloud copy when signed in) and reloads into the first lesson.
- The name and colours live in the save as `name` and `look`, and in the cloud inside `save_blob`. The `display_name` column in `game_players` is not written yet; multiplayer will need it.

### Your elf, in more detail

- **Growing up is not scaling up.** A taller elf is slimmer for its height (width grows at two thirds the rate of height), has longer legs, arms and body, and a head that is smaller for its body. The shoulders close the top of the tunic and there is a neck, so you can no longer see down into the collar.
- **Boy or girl**: a tunic, or a dress that is fuller in the skirt and drawn in at the waist. Either can have any hair, face or colour.
- **Hair**: short, long, pigtails, braid, bun, curly, or none.
- **Faces**: eight (cheerful, wide-eyed, sleepy, determined, gentle, mischief, grumpy, freckled).
- **Colours**: 10 skins, 12 hair colours, 12 hat colours, 14 tunic colours.
- **The pointed cap** is pulled down onto the head, with a brim band just above the eyebrows; it can also be left off.
- **Change how my elf looks** on the title screen reopens the creator for the elf you already have. Saving changes only the look and the name; lessons, coins and the satchel are untouched.
- While the creator is open, animals near the elf are hidden so nothing walks through the picture.

### The master account

- One account is flagged in the database table `game_masters` (row-level security on; players can read only their own row and cannot write). `game_load_progress` returns `master: true` for it. The flag lives only in memory for the signed-in session and is never written to the device, so signing out removes it.
- For that account the map panel shows a **Tester** block: go to the selected town with no lesson started, go to Hearthmoor, fly the glider, "bicycle, 200 coins and one of everything", and five clock buttons: **8:58 in the evening** (stand in the Hollow and watch closing time), **midnight**, **6:35 in the morning**, **noon**, and **the real time**. A clock button jumps to that hour and the clock runs on from there. The satchel has no limit.
- Everyone else reaches a town by walking, cycling or the ferry, and starts lessons with the teacher there.

## Phones and tablets

- Walking uses a thumb stick: a fixed round pad at the bottom left. The knob follows your thumb, and how far you push sets how fast you go. Drag the picture to look.
- The round buttons are Map, Bag, Ride (once you own the bicycle), Run, View, Jump and Pick up. The big button says Trade or Sit when that is what it will do.
- A small line above the stamina bar says what the pick-up key will do when you are next to something: pick up, trade or sit.
- The round buttons answer the touch itself. The game stops the browser's own touch handling on the control pad so the page cannot scroll or zoom, and that also stops phones sending a click afterwards, so nothing there may rely on click.
- The picture is always drawn at the size of the box it is shown in, checked every frame, so it cannot be stretched when a phone's address bar shows or hides.

## The bike

- The bike goes where it points. W pedals, A and D steer, S brakes, and letting go coasts. It turns more slowly the faster it goes, and the camera swings round behind it.
- The elf sits on the saddle with hands on the bars; each foot follows its pedal, the wheels turn with the ground speed, and the bike leans into turns.

## Settings

**Settings** on the map screen (or `P` in play, Select on a controller) opens the settings screen, saved per device:

- **How you play**: mouse & keyboard, controller, or touch (VR is listed for later). Hints and the controls panel use that device's buttons. Plugging in a controller switches to it automatically until you choose otherwise.
- **Keys**: every keyboard action can be rebound; click a key and press a new one. Reset to defaults is one button.
- **Mouse**: inverted look on or off, and sensitivity.
- **Graphics**: Auto / Low / Medium / High sets shadows, grass density and pixel resolution; Distance (Near / Mid / Far) sets how far you can see before the haze. Auto picks a level for the device's graphics card. On a laptop with an integrated GPU, Low + Near roughly triples the frame rate.
- **Sound**: birdsong on or off.
- **Help**: whether the controls panel shows while playing (it fades after a few seconds; `H` brings it back).

## Controls

| Keyboard and mouse | Controller | Touch |
|---|---|---|
| W A S D walk | Left stick | On-screen pad |
| Mouse look (inverted; click the game to grab the mouse, Esc to release) | Right stick | Drag |
| Shift run (stamina bar) | B or left-stick click | Run button |
| Space jump | A | Jump button |
| E pick up, talk, sit | X | Pick up button |
| R ride / get off the bicycle (once you have one) | D-pad up | Ride button |
| I open the satchel | D-pad down | Bag button |
| V switch between the back view and the front view | Y | View button |
| Z or mouse wheel: close, mid or wide camera | Bumpers | — |
| M open the world map | Start | Map button |

During a conversation, A presses the highlighted button and the d-pad moves between choices.

## What is in the file

- **Thistledown Hollow**: Bramblewick's shop (interior and exterior), the village street, the nursery, the
  woodshop, Fen's stall, the berry bush, the Old Oak, a fence and the south gate.
- **Ten Hollow lessons**: the spill, the first trade, the big harvest (tens and ones), the ledger, the nursery
  (counting by 2s, 5s and 10s, the water cycle), winter stores (subtracting and a first bar chart), the measuring
  stick, shapes in the woodshop, the hundred-acorn count, and raising a rhinoceros beetle (life cycle).
- **The Long Road**: a separate scene reached through the gate, linking the Hollow to the orchards.
- **The River Road**: real hills between the orchards and Fernwater. A winding dirt road climbs a ridge past oaks, pines, boulders and cairns; slopes slow you down, running uphill burns stamina, anything steeper than about 45 degrees is a wall, and you can leave the road and wander the whole valley.
- **The High Road**: from Fernwater's far bank a second road climbs through heather, crags and old drystone walls to the highlands.
- **Cairnhold Highlands** (Region 4, geometry and measurement, ten lessons): Wren Ashcombe, master surveyor, and Pip, apprenticing again. Units and the hundred-link chain, angles on a protractor, perimeter and area, odd fields and triangles, circles and pi, volume of the cisterns, Pythagoras, coordinates and scale on the lodge maps, similar triangles, and the capstone: your numbers build the Cairnhold Bridge across the gorge. The Surveyors' Lodge is a walk-in interior.
- **The Cliff Road**: from Cairnhold's far gate, past the bridge, a road winds down the escarpment (keep left of the guard wall) to Cogsworth.
- **Cogsworth** (Region 5, pre-algebra, ten lessons): a clockwork town built into a cliff, with gear trains turning on the rock face, a water clock in the square, a brass lift down to the ice cellars, the Clockworks and the Bicycle Works (both walk-in). Tamsin Gearwright teaches ratios with gear teeth, rates with the water clock, proportions with clock oil, percent, negative numbers on the lift, integer arithmetic, order of operations as gear chains, first equations (the missing tooth), a Season Fair that re-tests older skills, and the capstone: you gear your own bicycle to climb the hill out of town. The bicycle is the game's first fast travel: `R` (D-pad up on a controller) gets on and off; it rides at about twice walking speed on land, slows hard on slopes, and never counts toward stamina.
- **The Mountain Road**: through the gate at the top of Cogsworth's hill, a road about 56 feet long climbs to the snow line past abandoned ore carts and dwarven waymarks, ending at the mine mouth of Emberdeep.
- **Emberdeep** (Region 6, Algebra 1 and 2, ten lessons): the dwarven forges under the mountain. A lava river runs along the east side (you can't step in it), two furnaces with gauges reading 800-1200, anvils, house-sized hammers, a brass balance scale, the recipe lectern, the bellows, and the forge-grid: a square of the floor tiled as a coordinate grid. Master Durna Ironvein teaches one- and two-step equations as a balance, multi-step equations with like terms, inequalities (the furnace window), graphing lines (slope as chimney pitch, intercepts), systems of equations (two recipes at once), quadratics (factoring, roots, vertex, the formula), polynomials and rational expressions, exponents as bellows doubling, logarithms as counting the doublings, and the Mountain Fair review. Capstone: Tamsin brings the engine; you size its cylinder and time the spark (a system of two cycles), then weld it with Durna. The sixth mark reads "unknown."
- **The Sea Road**: out of the back of Emberdeep, through a second iron arch, and about 60 feet down the mountain to the coast, with the sea on your right the whole way, sea stacks offshore and wind-bent pines leaning away from the water.
- **Lanternreach** (Region 7, trigonometry, functions and pre-calculus, ten lessons): a lighthouse town on sea cliffs, always at dusk under stars and a low moon. The lighthouse beam goes round once every 12 seconds. There is an observatory with a brass dome, three cottages you can walk into (the Keeper's Cottage, Reef & Rope Chandlery, the Tide Office with a gauge that rises and falls), a shadow stake, a star board, and a cove with a beach, a pier, three boats bobbing at their moorings, a tide gauge and a bell buoy flashing red out in the water. You can walk down into the cove and swim; from the water you climb onto the pier at its ramp. The Pillar Rocks stand far out to sea, lit from their tops, where the sky-lifts will anchor. Astra Vell, keeper and astronomer, teaches right-triangle trig from the cliff's shadow, degrees and radians and the unit circle from the turning beam, sine waves from the tide gauge, vectors from boats in a current, function families and their shifts and flips, inverse functions, arithmetic and geometric sequences and series from the keeper's log, limits from the ship that halves its distance, the Regatta (Emberdeep's algebra under sail), and the capstone: navigating the small boat to the Pillar Rocks by the pole star, a bearing from the chart, and a current correction. The seventh mark reads "change."
- **The sky-lift**: a brass-ribbed gondola at the end of Lanternreach's pier. Walk to the end of the pier and it carries you, over about twenty seconds, out over the Pillar Rocks and up through the cloud to Skyhaven. You can look around on the way; the sea drops away below. It runs back down from the station on the main island.
- **Skyhaven** (Region 8, Calculus 1 and 2, twelve lessons): three floating islands roped together by sagging plank bridges, rock undersides trailing roots, clouds below and the whole world small beneath them. The Glider Guild hall (with the broken wing and a tangent chalked on the board), the Balloon Works, the Engine House where your Emberdeep engine sits on a bench, a wind tunnel with a turning fan, balloon docks, wind socks, and a launch rail that runs off the island edge with your glider on it. Rope fences keep you from walking off the edge. Professor Ilsabet Quill (goggles, scarf) teaches limits as the shrinking secant, the derivative and the power rule, product/chain/quotient rules, maxima and minima (the best wing angle), related rates (the balloon and the winch), integrals as area and Riemann sums from tunnel readings, the fundamental theorem both ways, substitution, parts and partial fractions, volumes of revolution and arc length, series and Taylor series as the engine's timing table, first differential equations, and the Guild Fair, where every teacher on the road sends one problem by messenger bird. Fen and Pip, grown, are at the Engine House and turn up for the build.
- **The flight home**: the capstone sizes a motor-glider from your own numbers (wing area, glide ratio, engine minutes), then you launch. The glider flies you over the whole Long Road laid out small below: Lanternreach's light, the mountain of Emberdeep, a turning gear for Cogsworth, Cairnhold's stones, the river at Fernwater, the Orchards, and down into Thistledown Hollow, where the eighth mark is read: "One, many, part, shape, balance, unknown, change, and home."
- **Buildings you walk into**: Fernwater's four shops and Quern Mill each load their own interior when you step through the door (bakery with oven, counter and tables; the money-changer's barred counter; the rope and net shops; the mill's stones, hopper and gearing). Shopkeepers talk to you inside.
- **Ambient life**, all at real scale next to a seven-inch elf: flocks of birds and a lone hawk overhead; deer at the tree line (a buck with antlers), a fox trotting the hills, a badger, rabbits that bolt, pheasants that burst up out of the grass, hedgehogs and field mice in the meadows, squirrels running up and down the oaks, crows on the fences that flap off when you come near, ducks on the river, frogs on the bank that hop in, a heron, dogs in the Hollow and Fernwater that come to say hello, a few butterflies, and synthesized birdsong outdoors (after the first click or key press).
- **The river**: walk down the bank and you wade in and swim (slowly, no jumping); the beetle waits on the shore. The boardwalk stays walkable above you, and you climb back onto it at the ramps.
- **The Mallow Orchards**: Quern Mill with its mouse-powered wheel, orchards in rows, crates, a cider press.
  Lessons: rows and rows (multiplication as arrays), the mill songs (times tables), fair shares, leftovers
  (remainders), Pip's puzzle (factors and multiples), big orders (the area model), the long split (long division
  in chunks), the cider press (mixed problems), and the Season Fair (a re-test of the Hollow).
- **Fernwater Crossing**: a river town on stilts with a street of shops, a bakery, a money-changer, a rope-walk, a ferry and a river
  gauge. Lessons: half a rope (fractions as equal pieces), the baker's cakes (fractions of a whole and of a group,
  equivalence), adding slices, the money-changer (acorn coins, silver leaves, gold rings and the decimal point),
  decimals on the dock (reading, comparing and rounding), fractions of a trip (multiplying and dividing fractions),
  price per (unit price), percent off, and Fen's barge (a ledger that reviews the whole region).
- **Saving**: the browser remembers your coins, finished lessons, your beetle and where you were standing. Sign in on the map screen (email + password) and the same save goes to the cloud too, so progress follows you between devices; the browser copy is the offline fallback. Accounts live in a Supabase project with row-level security, so each player can only read their own save.
- A world map (press M) showing the whole land, where you are, and the layout of the region you're in, with every built lesson one click away. Lessons can also be opened by URL hash,
  for example `index.html#lesson-3`, `index.html#orch-2` or `index.html#hills`.

## Design notes

The full storyline for all eight regions (counting through Calculus 2), the characters, and the design
principles live in the project's design documents. The short version:

- One idea per lesson, shown several ways, used in the world before the next one opens.
- Skills are keys: you cannot do a trade until you really know its math, and a badly done job gives a bad result.
- Real but fun: real scale, real animals, real consequences (a dropped acorn sprouts where it fell).
- Mistakes are okay. You fix them and keep going.

## Roadmap

- The road is complete: eight regions, Thistledown Hollow to Skyhaven. What comes next is depth, not length: the AI tutor, riding, multiplayer, growing villages.
- Real 3D models and textures in place of the code-built shapes.
- Characters backed by a language model so a learner can ask Bramblewick why.
- Classroom mode (one teacher, a class in a shared village), then an open shared world.
- VR through WebXR.

## Credits

Built by Jake Hardin with Claude. Fonts: Uncial Antiqua and Shantell Sans from Google Fonts.
