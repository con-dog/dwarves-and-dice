# DigDice

**Dwarves and dice.** A first-person dice game where the dice are blocks in the world: you dig them out, carry them, and build poker hands with them.

> [!NOTE]
> This is the public write-up for a private repository. DigDice is in active development and headed for Steam. Rules, numbers and looks here will keep changing until it ships.

<p align="center">
  <img src="media/gifs/plains-shrine-fpv.gif.png" alt="First person on the plains by day: the last die goes into an All Odd shrine, blue rays pour up as its dice turn to whitestone and 27 is added to the score, then a turn past other dwarves to dig a six loose" width="800">
</p>

## What it is

You are a dwarf on a small voxel map with one, three or seven other dwarves. Dice are set into the walls and lying about the field. Standing here and there are **shrines**: groups of frosted-glass ghost dice, each group asking for one hand (a pair, a straight, three of a kind, a sum of 8).

- **Dig.** Swing at a die until it breaks loose, then walk over it to pick it up.
- **Carry.** Your tray holds six things. Dice, stone, ladders and modifiers all compete for the same six slots.
- **Answer a shrine.** Place real dice into the ghosts. When the hand is complete the shrine pays out, all of it, to whoever placed the last die.
- **Whack.** Hit another dwarf and a die falls out of their tray.

A round is five minutes, and the highest score wins. The whole game plays on five keys: the four arrows and `SPACE`.

<table>
  <tr>
    <td width="50%"><img src="media/staged/grove-day-gathering.png" alt="Four dwarves in orange, teal, pink and white standing on the grass in a grove, a CRAFT door on the left and a stack of glass ghost dice on the right"></td>
    <td width="50%"><img src="media/showcase/dusk-god-rays.png" alt="Dusk: red rays rising from gold-rimmed dice in a half-filled shrine, a ghost die waiting above them"></td>
  </tr>
  <tr>
    <td>A grove by day. The glass blocks on the right are a shrine waiting for dice.</td>
    <td>Dusk. Rays rise from a shrine someone has started, in that dwarf's colour.</td>
  </tr>
  <tr>
    <td><img src="media/staged/mining.png" alt="A purple dwarf mid-swing at a six covered in cracks, chips flying off it, a shrine of ghost dice to the right under a coral sky"></td>
    <td><img src="media/gifs/whack-fpv.gif.png" alt="First person, by day: a whack lands on Cocky, an orange dwarf, who flashes white with stars round their head while dice fly out of their tray and bounce across the grass"></td>
  </tr>
  <tr>
    <td>Dig: a six, nearly out.</td>
    <td>Whack: stars, and dice knocked loose.</td>
  </tr>
</table>

## Why

The question I asked myself with this project was: **what would a dice game look like if it were a first-person dungeon crawler?**

It did not start with dice. It started as an engine experiment (a discrete voxel grid with a smooth first-person view, generated entirely from a seed) and went looking for a game:

- **Letters first.** First-person Scrabble: mine letter tiles, spell words in the world. Long words are very hard to see from inside a corridor, and the scoring has too many possibilities to read at a glance.
- **Cards.** 52 of them before any jokers, and unreadable at a distance on a 32×32-texel block face.
- **Dice.** Six faces, each legible as pips at any size, and a small set of hands most people already know.

Dice brought a second problem. In a dice game the fun is watching a hand score, but in first person you only ever see the corridor you are standing in. A scoring engine spread across the map does its work where nobody is looking. Shrines are the answer to that: a hand is asked for at a *place*, you stand in front of it to finish it, and it pays out in front of you. The world stops being storage and becomes the board.

<table>
  <tr>
    <td width="50%"><img src="media/showcase/evolution-1-flat-colour.png" alt="Day two: flat-coloured red, blue and yellow blocks in a corridor with a minimap"></td>
    <td width="50%"><img src="media/showcase/evolution-2-letters.png" alt="Day ten: orange letter tiles V and H with Scrabble scores, and a scoreboard block reading YOU 16"></td>
  </tr>
  <tr>
    <td>Day 2: an engine with nothing to do.</td>
    <td>Day 10: the word game, the day before the letters became dice.</td>
  </tr>
</table>

## How a round plays

<p align="center">
  <img src="media/gifs/hatch-fpv.gif.png" alt="Dusk at the foot of a pyramid: red, purple, teal and orange eggs crack open one after another, a dwarf bursts out of each in a spray of shell, and they scatter up the tiers" width="640">
</p>

**Everyone hatches.** Each round starts with every dwarf in an egg at their spawn. The clock starts once they are out. Get whacked with an empty tray and you are back in an egg for three seconds.

<p align="center">
  <img src="media/showcase/eggs.png" alt="Seven coloured eggs lined up on the grass under an ore canopy, some smiling, some cracked" width="640">
</p>

**Shrines ask, dice answer.** A fixed shrine shows the pips each ghost wants. An open one shows `?` and takes any dice that make its hand. Face a ghost and press `↑` to check it: the hotbar turns into the shrine's hand, its ghosts, and what it would pay.

<table>
  <tr>
    <td width="50%"><img src="media/showcase/shrine-check-four-of-a-kind.png" alt="Checking a shrine: the hotbar reads 4KIND times five, four empty ghost slots with level premiums of one to four, and plus 300 as the most it could pay"></td>
    <td width="50%"><img src="media/showcase/shrine-in-progress.png" alt="A shrine with gold-rimmed dice already placed in it, ghost dice beside them, a CRAFT door on the left and two dwarves nearby"></td>
  </tr>
  <tr>
    <td>Four of a Kind, stacked four high: up to 300.</td>
    <td>Dice in a shrine wear a gold rim. Whoever places the last one takes the lot.</td>
  </tr>
</table>

**Height pays.** A shrine scores like a word in Scrabble: each die's pips × its level premium, added up, × the hand's multiplier. The premium is ×1 on the ground, ×2 a level up, ×3 two up. A pair of 4s on the ground pays (4 + 4) × 2 = 16. Stack them and it pays (4×1 + 4×2) × 2 = 24. So the best shrines are the ones you have to build up to.

| Hand | Multiplier | Example |
| --- | --- | --- |
| High Die | ×1 | `5` |
| Pair | ×2 | `3 3` |
| Three of a Kind, Tiny Straight, All Odd, All Even, Two Pair | ×3 | `4 4 4` |
| Small Straight, Full House | ×4 | `2 3 4 5` |
| Four of a Kind, Large Straight | ×5 | `5 5 5 5` |
| Five of a Kind, Full Straight | ×6 | `1 2 3 4 5 6` |
| Snake Eyes | ×12 | `1 1` |

<table>
  <tr>
    <td width="50%"><img src="media/gifs/shrine-stack-tpv.gif.png" alt="Dusk: a dwarf up a ladder places the top die on a tower of four gold-rimmed dice, blue rays pour up it, the tower turns to whitestone and the score runs from 102 to 302"></td>
    <td width="50%"><img src="media/gifs/shrine-straight-tpv.gif.png" alt="Dawn: the last of six dice goes into a shrine built three levels up a wall, blue rays flood over it, the dice turn to whitestone and the hotbar counts up to plus 174"></td>
  </tr>
  <tr>
    <td>A Small Straight stacked four high: (3×1 + 4×2 + 5×3 + 6×4) × 4 = 200.</td>
    <td>A Full Straight, one to six, built three levels up: +174.</td>
  </tr>
</table>

**Everything is a block.** Stone, four ores, ladders, chests, the CRAFT bench and the EXIT door can all be mined, carried and put down somewhere else. Carry off a bench and nobody else can craft at it. Ores craft into four modifiers that work while they sit in your tray: **Strength** (dig and whack harder), **Speed**, **Sight** (stand still to X-ray dice and shrines through walls) and **Luck** (roll a loose die). Every slot spent on one is a slot that cannot hold a die.

<p align="center">
  <img src="media/staged/modifier-podiums.png" alt="Four dwarves at dusk, each standing on a modifier block: a red pickaxe for Strength, a yellow lightning bolt for Speed, a blue eye for Sight and a green shamrock for Luck" width="640">
</p>

<table>
  <tr>
    <td width="50%"><img src="media/gifs/sight-fpv.gif.png" alt="Sight at work in a city at dusk: the player stands still, the street fades to black and the dice and shrine ghosts all around show as yellow and white wireframe cubes, then the world comes back"></td>
    <td width="50%"><img src="media/gifs/roll-fpv.gif.png" alt="Luck at work: up close to a loose die with a ROLL prompt in the hotbar, its face changing with each roll, ending on a one and two sixes placed in a row"></td>
  </tr>
  <tr>
    <td>Sight: stand still, and nearby dice and shrines show through the walls.</td>
    <td>Luck: face a loose die and roll it until it shows what you need.</td>
  </tr>
</table>

<table>
  <tr>
    <td width="33%"><img src="media/showcase/craft-menu.png" alt="A CRAFT sign over a small wooden door at dusk, the craft menu open in the hotbar with Strength selected"></td>
    <td width="33%"><img src="media/showcase/chest.png" alt="A wooden chest with gold corners and a keyhole, seen up close"></td>
    <td width="33%"><img src="media/showcase/dwarf-angry.png" alt="A purple-hooded dwarf with a pickaxe in a stone tunnel next to a wall of greenstone; the hotbar names him ANGRY"></td>
  </tr>
  <tr>
    <td>A craft bench, with Strength picked from its menu.</td>
    <td>A chest. Some landmarks seal hoards of high dice inside.</td>
    <td>Angry, who is about to whack you.</td>
  </tr>
</table>

**Then the board.** When the clock runs out, the scoreboard comes up a row at a time from last place, the winner last.

<p align="center">
  <img src="media/gifs/round-over.gif.png" alt="The round-over board in the grove: the clock runs out, then eight dwarves are listed a row at a time from Harpy in eighth up to Cocky first with 222, and COCKY WINS in gold" width="640">
</p>

## The dwarves

The other seats are played by bots: eight dwarves, A to H, one for every way to play. Each has a five-letter name ending in Y, and the name is a promise about what they will do. All but Egg, who has no way of their own to play and so no proper name either.

<p align="center">
  <img src="media/showcase/dwarf-lineup.png" alt="Seven dwarves standing in a row on the grass, each in a different colour of hood and tunic with a pickaxe in hand, the grove behind them" width="800">
</p>

| Dwarf | Plays |
| --- | --- |
| **Angry** | stacks Strength, whacks anyone in reach |
| **Beady** | stacks Sight, X-rays for the big shrines |
| **Cocky** | stacks Luck, bets on rolls |
| **Dusty** | climbs, bridges and tunnels everywhere |
| **Egg** | a bit of everything |
| **Fiery** | stacks Speed, first to every shrine |
| **Giddy** | crafts dice nonstop and keeps everything |
| **Harpy** | lurks by your shrine to finish it |

## Worlds

Every world is generated from a seed, and nothing about it is stored. A seed picks the format (a duel, four players or eight, on a 16×16×4, 24×24×6 or 32×32×8 grid), one of fifteen map archetypes (a vault, canyons, plains, a labyrinth, a grove, pillars, a ziggurat, a city, a quarry, bridges, pyramids, a colosseum, a castle, ruins, islands), how generous the dice are, the time of day and the sky.

<p align="center">
  <img src="media/gifs/scale.gif.png" alt="Two generated worlds side by side as small turning models on a dark background: a 2-player world on 16 by 16 and, far larger beside it, an 8-player grove on 32 by 32" width="800">
</p>

<p align="center">
  <img src="media/gifs/archetypes-models.gif.png" alt="Twelve of the map archetypes as small labelled models, shown one at a time: pillars, islands, canyons, city, colosseum, pyramids, grove, ziggurat, bridges, ruins, and the labyrinth and vault lit up at night" width="640">
</p>

<table>
  <tr>
    <td width="50%"><img src="media/showcase/city.png" alt="A city of grey stone towers with ore windows and red ore roofs, ghost dice stacked on the rooftops"></td>
    <td width="50%"><img src="media/showcase/quarry.png" alt="The quarry by day: a plateau of ore slabs under a flat blue sky, a two-high stack of ghost dice on top and a dwarf beside a row of them"></td>
  </tr>
  <tr>
    <td>The city: towers, streets, and shrines on the roofs.</td>
    <td>The quarry: a plateau cut with pits, shrines up top.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/grove-day-canopy.png" alt="Looking across the tops of the grove's ore canopies by day, three dwarves standing up on them and ghost dice in the foreground"></td>
    <td><img src="media/staged/no-hud/pyramids-day-shrine.png" alt="Two dwarves either side of a tall stack of ghost dice, a loose die on the grass between them and pink rays over a shrine in the distance"></td>
  </tr>
  <tr>
    <td>The grove from up in its canopy.</td>
    <td>Among the pyramids: a stacked shrine and a loose die.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/pillars-dawn-tower.png" alt="Dawn: a tall tower of gold-rimmed dice in the middle, a dwarf on either side of it and a CRAFT door at each edge of the frame, under a rose sky"></td>
    <td><img src="media/staged/no-hud/ziggurat-dawn-steps.png" alt="Dawn on the ziggurat: stepped tiers of ore, stone and dice, one dwarf at its foot and two more higher up its steps"></td>
  </tr>
  <tr>
    <td>Dawn among the pillars: a tall shrine, mostly filled, between two craft benches.</td>
    <td>The ziggurat at dawn, dwarves up its steps.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/bridges-dusk-towers.png" alt="Dusk: a tower of ghost dice on a gold-rimmed six, three dwarves around it, stone towers and an arch behind and the sun low in a coral sky"></td>
    <td><img src="media/staged/no-hud/canyons-dusk-modifiers.png" alt="Dusk in the canyons: a dwarf by a CRAFT door under a wall of ore, Sight and Strength blocks on the grass, and a shrine with a gold-rimmed six in it"></td>
  </tr>
  <tr>
    <td>The bridges at dusk, the sun going down behind the towers.</td>
    <td>The canyons: a craft bench, modifier blocks and a shrine in play.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/colosseum-day-drum.png" alt="The colosseum from outside by day: a tall round wall of mossy stone with a hoop of yellow ore and a rim of red, three dwarves and a shrine of ghost dice on the grass in front of it"></td>
    <td><img src="media/staged/no-hud/colosseum-dusk-seats.png" alt="Inside the colosseum at dusk: rows of stone seats stepping up to a red rim, ladders up them, dwarves on the rows and a stack of ghost dice down in the pit"></td>
  </tr>
  <tr>
    <td>The colosseum: a round bowl of seats, a gate through it on each side.</td>
    <td>Inside it at dusk, looking across the pit from the seats.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/castle-dawn-keep.png" alt="Dawn in the castle: a tall keep with a red roof and bands of yellow and green windows, a ladder up its face, seen from the wall walk across the ward under a rose sky"></td>
    <td><img src="media/staged/no-hud/castle-day-ward.png" alt="The castle by day from inside its walls: dwarves up on the wall walk either side, turrets capped in blue ore and whitestone, red and yellow lintels over the gates and ladders in the corners of the ward"></td>
  </tr>
  <tr>
    <td>The castle at dawn: its keep, from the wall walk.</td>
    <td>A smaller castle by day: turrets, gates, and dwarves on the walls.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/ruins-dusk-halls.png" alt="Dusk in the ruins: three dwarves on the grass among broken mossy walls with bands of red and blue ore in them, ghost dice between them and a tower of gold-rimmed dice sending up pink rays"></td>
    <td><img src="media/staged/no-hud/ruins-night-frieze.png" alt="Night in the ruins: two dwarves beside a broken wall with yellow and blue ore set in it, shrines of ghost dice glowing white either side under the stars"></td>
  </tr>
  <tr>
    <td>The ruins at dusk: broken halls, ore in what still stands, moss on all of it.</td>
    <td>The ruins by night.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/islands-day-under.png" alt="Three dwarves on the grass round a tall stack of dice, slabs of mossy stone hanging in the air behind them with red and yellow ore on their undersides and ladders down to the ground"></td>
    <td><img src="media/staged/no-hud/islands-dawn-shrine.png" alt="Dawn among the islands: three dwarves on the grass by a row of ghost dice, a mossy slab of stone hanging in the air behind them with yellow ore under it and dice stacked on top, under a rose sky"></td>
  </tr>
  <tr>
    <td>The islands: rocks hanging over an open field, a ladder down from each.</td>
    <td>At dawn: dice up on one already, ghost dice waiting under it.</td>
  </tr>
  <tr>
    <td><img src="media/showcase/grove-dusk.png" alt="A grove at dusk under a coral sky, dwarves crossing the grass between the trunks"></td>
    <td><img src="media/showcase/dawn-rooftops.png" alt="Dawn under a rose sky, looking across red and blue ore rooftops at a ghost die sending up white rays"></td>
  </tr>
  <tr>
    <td>About one world in twelve is at dusk.</td>
    <td>As many are at dawn.</td>
  </tr>
</table>

<p align="center">
  <img src="media/gifs/times-of-day.gif.png" alt="One spot in the grove, a dwarf at a shrine sending up gold rays, shown by day, at dusk, at night and at dawn: the sky going from blue to coral to black to rose" width="640">
</p>

One world in twenty is at night: a black sky with that world's own stars, shrines glowing like lamps, and a lamp on every dwarf's helmet.

<table>
  <tr>
    <td width="50%"><img src="media/staged/no-hud/vault-night-walls.png" alt="Night at the vault: walls banded in blue, yellow, red and green ore, a dwarf standing in the light of a glowing row of ghost dice, two more dwarves nearby"></td>
    <td width="50%"><img src="media/staged/no-hud/plains-night-field.png" alt="Night on the plains: a wide field dotted with glowing shrines, four dwarves standing among them, gold rays rising in the distance"></td>
  </tr>
  <tr>
    <td>The vault.</td>
    <td>The plains, every shrine a lamp.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/city-night-street.png" alt="Night in a city street: two dwarves beside a lit, gold-rimmed die with a ghost die on top of it, pink rays rising, dice set into the wall behind"></td>
    <td><img src="media/staged/no-hud/labyrinth-night-shrine.png" alt="Night in the labyrinth: a dwarf standing in front of gold-rimmed dice under a row of ghost dice, red rays rising over them, another dwarf by the wall"></td>
  </tr>
  <tr>
    <td>A city street.</td>
    <td>The labyrinth.</td>
  </tr>
  <tr>
    <td><img src="media/showcase/night-city-rooftop.png" alt="Night on the city's rooftops: ghost dice glowing on a lit greenstone roof, more shrines on the towers behind"></td>
    <td><img src="media/showcase/night-grove.png" alt="Night in the grove: red, yellow and green ore canopies lit from below, a ghost die on top of one and a loose die in the foreground"></td>
  </tr>
  <tr>
    <td>The city from its rooftops.</td>
    <td>The grove.</td>
  </tr>
  <tr>
    <td><img src="media/staged/no-hud/castle-night-turrets.png" alt="Night at the castle: three dwarves on the grass before its wall, a turret capped in blue ore above them and rows of ghost dice glowing white beside them"></td>
    <td><img src="media/staged/no-hud/islands-night-field.png" alt="Night under the islands: three dwarves with lit helmet lamps on the grass, a tall shrine sending up white rays beside them and slabs of stone hanging in the dark behind, yellow ore under one"></td>
  </tr>
  <tr>
    <td>The castle.</td>
    <td>The islands.</td>
  </tr>
</table>

<p align="center">
  <img src="media/gifs/vault-night-jackpot.gif.png" alt="A shrine being answered at night at the vault: orange rays flooding up from two lit, gold-rimmed dice as they turn to whitestone" width="400">
  <img src="media/gifs/grove-dusk-jackpot.gif.png" alt="A pair of fours finished in the grove at dusk: gold rays pouring up, the dice turning to whitestone and 16 added to the score" width="400">
</p>

When a shrine is answered it goes up in a flood of rays and its dice turn to whitestone, which crafts into fresh dice.

## How it is built

TypeScript and WebGL 2, running in a browser tab. About 35,000 lines, nine weeks in.

- **Discrete model, fluid view.** The simulation advances in fixed 60 Hz ticks with integer motion, and input is applied only between ticks. The camera is interpolated between ticks, so movement obeys the grid while reading as smooth.
- **Deterministic.** All of the game's chance (the world, crafted dice, rolls, the bots) comes from the seed. A round is fully determined by its seed and its inputs and plays the same on any machine.
- **No art files.** Every texel of stone, moss, ore, ladder and die is generated in code from the material and its position on the face. The only hand-drawn pictures are small pixel grids written in the source.
- **No audio files.** Every sound effect and all the music is written as notes for a sample-by-sample model of the SNES sound chip: eight voices, BRR-compressed samples, gaussian interpolation, echo.
- **Logic and rendering are separate.** The renderer only reads the simulation's state. The same game runs headlessly with no canvas, which is what the tests, the bot arena and the tuner use.
- **Bots that play by the rules.** A bot has perception, beliefs about what it has seen, a navigator and a solver for shrines, and drives its dwarf with the same inputs a player has. How bots weigh things up is tuned by an evolution strategy that plays them against each other on fresh seeds every generation.
- **Language models can take a seat.** A model is told what its dwarf has seen and gives one order at a time; a bot carries each order out. The game runs in lockstep and waits for the answer.

## Status

In development. The repository is private, so the details here are deliberately partial. If you have questions, reach out.
