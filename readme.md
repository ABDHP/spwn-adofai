# ADOFAI in spwn
v0.2

Creates GD version of ADOFAI levels based on your json data

## What is happening inside the level

You have to *click* when one of the circles arrives on a tile. Timings are flexible, defaulted to 30 degrees.

If your click is not in the timing, you *miss*. You know you missed if BG pulses yellow.

If you didn't click the tile in timing (or clicked too early resulting in a *miss*), you *die* (+ BG pulses red). Also, you *die*, when a certain number of *misses* is reached.

When you click on the last tile, level is *completed*, and you have to wait two more seconds. But be careful: you can *miss* and *die* due to misses even when level is *completed* (i think so).

## Requirements

* `--allow readfile`
* Level: you don't need to place *any* blocks in editor. The only requirements are:
	* Colors:
		* Color 1 is color of the tiles and roads between them (white recommended)
		* Color 2 and color 3 are player's colors (P1 and P2 recommended)
		* Color 4 is twirl color (violet recommended), color 5 is speed up and color 6 is speed down (red and green recommended)
	* Gamemode: ship/wave/ufo, speed 4x
	* Nice-looking bg+ground, set their color to black

## JSON structure

### Root object

The root object has these keys (\* means it's necessary):
* \*bpm :number - starting bpm of the level (1 beat = 180 degrees)
* angle :number - angle defing timings acuracy in degrees (max angle player can fail) - default is 30
* misses :number - max number of misses player can have before dying - default is 5
* speed :number - works like speed trial in ADOFAI - default is 1
* zoom :number - the objects in the level (and distances) are as big as this number is - default is 1
* \*path :string - defines the path of level
* \*events :array - array of events like bpm changing or twirl
* offset :number - offset in seconds (setting this to 0 is a bad idea) - default value is whatever needed to level start in one 180deg beat
* auto :bool - disables possibillity of dying - default  is false

### Path

In ADOFAI, we have 16 directions a tile can go, an each of them has a letter, representing it.

#### Cardinal directions (literally):
* w = up
* a = left
* s = down
* d = right

#### Directions with angle of 45 degrees:
* *w* = up
* e
* *d* = right
* c
* *s* = down
* z
* *a* = left
* q
* *w*, again

#### Directions with angle of 30 degrees:
* *w* = up
* y
* j
* *d* = right
* m
* b
* *s* = down
* v
* n
* *a* = left
* h
* t
* *w*, again

So if you have, for example, a path like 4 times right, then 1 up, them 4 left, you just smash the direction letters together an put them into `path` property:
`path: "ddddwaaaa"`

### Events
Events is array of objects, each of the object representing one of the events.

This object has two properties:
* floor :number - number of the tile where the event fires
* evType :string - type of the event (listed bellow) (named it evType to not mix with spwn's `type` property)

#### Types
* speed - changes the current bpm, starting from the tile it's on and until another speed change or level end. **Requires additional parameter in the event object**: bpm :number
* twirl - changes the direction of player rotation

## converter.spwn

Do you have your ald (or maybe not) levels from the original adofai? If yes, you can use my converter to convert these to my adofai!


See `converter.spwn`
