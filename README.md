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
Mallow Orchards (nine lessons) and Fernwater Crossing (five lessons so far) are playable. Any lesson can be started directly, which is handy for testing.

## Controls

| Keyboard and mouse | Controller | Touch |
|---|---|---|
| W A S D walk | Left stick | On-screen pad |
| Mouse look (inverted; click the game to grab the mouse, Esc to release) | Right stick | Drag |
| Shift run (stamina bar) | B or left-stick click | Run button |
| Space jump | A | Jump button |
| E pick up, talk, sit | X | Pick up button |
| V switch between the back view and the front view | Y | View button |
| M open the world map | Start | Map button |

During a conversation, A presses the highlighted button and the d-pad moves between choices.

## What is in the file

- **Thistledown Hollow**: Bramblewick's shop (interior and exterior), the village street, the nursery, the
  woodshop, Fen's stall, the berry bush, the Old Oak, a fence and the south gate.
- **Ten Hollow lessons**: the spill, the first trade, the big harvest (tens and ones), the ledger, the nursery
  (counting by 2s, 5s and 10s, the water cycle), winter stores (subtracting and a first bar chart), the measuring
  stick, shapes in the woodshop, the hundred-acorn count, and raising a rhinoceros beetle (life cycle).
- **The Long Road**: a separate scene reached through the gate, linking the villages.
- **The Mallow Orchards**: Quern Mill with its mouse-powered wheel, orchards in rows, crates, a cider press.
  Lessons: rows and rows (multiplication as arrays), the mill songs (times tables), fair shares, leftovers
  (remainders), Pip's puzzle (factors and multiples), big orders (the area model), the long split (long division
  in chunks), the cider press (mixed problems), and the Season Fair (a re-test of the Hollow).
- **Fernwater Crossing**: a river town on stilts with a market, a bakery, a money-changer, a rope-walk, a ferry and a river
  gauge. Lessons: half a rope (fractions as equal pieces), the baker's cakes (fractions of a whole and of a group,
  equivalence), adding slices, the money-changer (acorn coins, silver leaves, gold rings and the decimal point),
  and decimals on the dock (reading, comparing and rounding).
- A world map (press M) showing the whole land, where you are, and the layout of the region you're in, with every built lesson one click away. Lessons can also be opened by URL hash,
  for example `index.html#lesson-3` or `index.html#orch-2`.

## Design notes

The full storyline for all eight regions (counting through Calculus 2), the characters, and the design
principles live in the project's design documents. The short version:

- One idea per lesson, shown several ways, used in the world before the next one opens.
- Skills are keys: you cannot do a trade until you really know its math, and a badly done job gives a bad result.
- Real but fun: real scale, real animals, real consequences (a dropped acorn sprouts where it fell).
- Mistakes are okay. You fix them and keep going.

## Roadmap

- Finish Fernwater (fractions of a trip, unit price, percent off, Fen's barge), then the regions beyond.
- Real 3D models and textures in place of the code-built shapes.
- Characters backed by a language model so a learner can ask Bramblewick why.
- Classroom mode (one teacher, a class in a shared village), then an open shared world.
- VR through WebXR.

## Credits

Built by Jake Hardin with Claude. Fonts: Uncial Antiqua and Shantell Sans from Google Fonts.
