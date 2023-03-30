# DevilX TMI File Format - Subtile Flags

[1. Description](#1-description)  
[2. File structure](#2-file-structure)  

## 1. Description

A TMI File Format was introduced to have a more refined control over the tile-drawing logic.  
It is similar to SOL File Format, only the flags have different meaning.  
Some of the flags are overlapping. In those cases the TMI values take precedence.

## 2. File Structure

```
{SUBTILE FLAG} * {NUMBER OF SUBTILES + 1}
```

The bits of the {SUBTILE FLAG} have the following meaning (starting from lowest bit):
- `1` : enable transparency on the wall
- `2` : left floor {CEL FRAME} should be drawn in the second pass
- `3` : left floor {CEL FRAME} has foliage pixels
- `4` : left floor {CEL FRAME} has transparent part
- `5` : right floor {CEL FRAME} should be drawn in the second pass
- `6` : right floor {CEL FRAME} has foliage pixels
- `7` : right floor {CEL FRAME} has transparent part
- `8` : unused

Usually the second pass flag(s) should be set for subtiles which are walls / high objects to hide entities behind them.  
Subtiles where this flag is set are completely (re-)drawn in the second pass.

The foliage pixels flag(s) should be set for subtiles which have small protrusion above the floor.  
Subtiles where this flag is set are partly (re-)drawn in the second pass.