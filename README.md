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
- Thistledown Hollow is 128 by 176 units inside its fence (it was 88 by 122). The street runs 138 units from the yard to the gate. The gate stands where it always did in the wider world; the Hollow grew north and sideways. A save made in the smaller Hollow, outdoors, starts again in the yard.
- Each of the four who work in the Hollow has a cottage with a path from the street: Bramblewick (north-east of the Stores), Marigold Fernsby (north of her nursery), Aldric Bough (south of the woodshop) and Fen Dewberry (west of her stall). They are listed in `HOMES`; each is a 14 by 10 unit cottage outside and a 26 by 20 unit room inside, with a hearth, bed, table and its owner's things. Nobody is at home: they are all at work.
- The Village Stores works as a room: the door is shut until you come near, walking through it fades you in or out, and it shuts behind you. The inside is still built at the same spot as the stump, so the ten Hollow lessons did not have to move.
- Doors inside rooms now swing outward, so the leaf never lands on furniture. The potting shed is 24 by 18 units inside.
- Animals that get stuck pick a new direction. They are only ever moved to a clear spot when more than 140 units from you, so you never see one jump.

## Title screen and your elf

- The game opens on a title screen with the live Hollow turning slowly behind it: Continue (when there is a save), New game, Lessons and map, Settings, and cloud sign-in.
- New game opens the creator: a name (up to 16 characters, "Elfling" if left blank) and colours for skin, hair, hat and tunic, shown on your elf as it turns. Begin starts the first lesson in Bramblewick's shop.
- One elf per player. If there is already progress, Begin warns what will be lost and must be pressed a second time; it then wipes the save (and the cloud copy when signed in) and reloads into the first lesson.
- The name and colours live in the save as `name` and `look`, and in the cloud inside `save_blob`. The `display_name` column in `game_players` is not written yet; multiplayer will need it.

## Phones and tablets

- Walking uses a thumb stick: a fixed round pad at the bottom left. The knob follows your thumb, and how far you push sets how fast you go. Drag the picture to look.
- The round buttons (Map, Run, View, Jump, Pick up) answer the touch itself. The game stops the browser's own touch handling on the control pad so the page cannot scroll or zoom, and that also stops phones sending a click afterwards, so nothing there may rely on click.
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
| R ride / get off the bicycle (once you have one) | D-pad up | — |
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
