# Diablo 1 DUN File Format - Level Maps

[1. Description](#1-description)  
[2. File structure](#2-file-structure)  
[3. `{DUN HEADER}`](#3-dun-header)  
[4. `{MAP LAYERS}`](#4-map-layers)  
[4.1 `{BASE LAYER}`](#41-base-layer)  
[4.2 `{PROTECTIONS LAYER}`](#42-protections-layer)  
[4.3 `{MONSTERS LAYER}`](#43-monsters-layer)  
[4.4 `{OBJECTS LAYER}`](#44-objects-layer)  
[4.5 `{ROOMS LAYER}`](#45-rooms-layer)  
[5. Executable Hardcoded DUN Level Maps](#5-executable-hardcoded-dun-level-maps)  
[6. Credits](#6-credits)


## 1. Description

Diablo 1 DUN level maps use the `.dun` file extension.  
The DUN level maps contain portions of levels used by the random level generation algorithm.  
Some of these files contain generic level layouts, other contain quest specific level layouts.

Level maps data longer than one byte (WORDs and DWORDs)	is stored using little-endian byte order.


## 2. File Structure

```
{DUN HEADER}
{MAP LAYERS}
```


## 3. `{DUN HEADER}`

```
{MAP WIDTH}
{MAP HEIGHT}
```

`{MAP WIDTH}` and `{MAP HEIGHT}` are both one WORD long (2 bytes).


## 4. `{MAP LAYERS}`

```
{BASE LAYER}
[{PROTECTIONS LAYER}
{MONSTERS LAYER}
{OBJECTS LAYER}
{ROOMS LAYER}]
```

Each INDEX is one WORD long (2 bytes).


### 4.1 `{BASE LAYER}`

```
{INCREMENTED TILE INDEX} * {MAP WIDTH} * {MAP HEIGHT}
```

This layer is the base layer of the map, it defines the floor/walls/doors.  
The structure consists of `{INCREMENTED TILE INDEX}`es.  
Those indices define the level starting from top left tile and ending with bottom right tile.  
The real tile index (in `lX.til`) is obtained with the following formula:

```
{TILE INDEX} = {INCREMENTED TILE INDEX} - 1
```

When `{INCREMENTED TILE INDEX}` is equal to 0 the default floor tile is used and might be changed if it is not a complete map.


### 4.2 `{PROTECTIONS LAYER}`

```
{PROTECTION FLAGS} * {MAP WIDTH} * {MAP HEIGHT}
{PLACEHOLDER} * {MAP WIDTH} * {MAP HEIGHT} * 3
```

This layer defined which items lie on the level ground.  
Now it specifies the protections of the tile and the related subtiles against modifications by the dungeon generator.  
The bits of the {PROTECTION FLAGS} have the following meaning (starting from lowest bit):
- `1` : the tile might be decorated, but otherwise should be unchanged
- `2` : the tile can not be changed at all
- `3`..`8` : (unused)
- `9` : no monster should be generated on the first subtile (top left)
- `10` : no object should be generated on the first subtile (top left)
- `11` : no monster should be generated on the second subtile (top right)
- `12` : no object should be generated on the second subtile (top right)
- `13` : no monster should be generated on the third subtile (bottom left)
- `14` : no object should be generated on the third subtile (bottom left
- `15` : no monster should be generated on the fourth subtile (bottom right)
- `16` : no object should be generated on the fourth subtile (bottom right)


### 4.3 `{MONSTERS LAYER}`

```
{MONSTERS TABLE INDEX} * {MAP WIDTH * 2} * {MAP HEIGHT * 2}
```

This layer defines which monsters stand in the level.  
This layer is a sub-tile layer, which is 4 times the size of the tile layer `{BASE LAYER}`.
If the highest bit is not set, the monster is a normal monster and the type is selected from a hardcoded table.
If the highest bit is set, the monster is a unique monster and it is selected by subtracting one from the rest of the bits.


### 4.4 `{OBJECTS LAYER}`

```
{OBJECTS INDEX} * {MAP WIDTH * 2} * {MAP HEIGHT * 2}
```

This layer defines which objects (chests, stands, etc.) are positioned in the level.  
This layer is a sub-tile layer, which is 4 times the size of the tile layer `{BASE LAYER}`.


### 4.5 `{ROOMS LAYER}`

```
{ROOMS INDEX} * {MAP WIDTH * 2} * {MAP HEIGHT * 2}
```

This layer defined the room index of the given sub-tile.  
The content of this part is no longer used by the game.  


## 5. Executable Hardcoded DUN Level Maps

Hellfire features hardcoded DUN level maps.

The NaKrull DUN level map is located at the following offsets:
- Hellfire 1.00 US : `0x00091CC8`
- Hellfire 1.00 EU : `0x00091AC8`
- Hellfire 1.01 : `0x000942B0`
The data is 26 bytes long.

The Cornerstone of the World DUN level map is located at the following offsets:
- Hellfire 1.00 US : `0x00091CE8`
- Hellfire 1.00 EU : `0x00091AE8`
- Hellfire 1.01 : `0x000942D0`
The data is 27 bytes long.

These hardcoded DUN level maps differ from the regular DUN level files, instead using WORDs for storing data they use bytes.  
The first byte is the level map width, the second byte is the DUN level map height and then each byte is an incremented tile index.


## 6. Credits

Most of this document is based on the work of the following people:
- Ulmo
- Zamal & Zenda
