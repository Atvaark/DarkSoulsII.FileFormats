Dark-Souls-II-Archive-Dump
==========================
A partial dump of the encrypted PC game archives (Version 1.0.7) for those that are interested in examining the game assets.

This repository should contain all the files that are required to load a savegame at the Majula bonfire.
Most of the files are stored in the following archives (Some files aren't packed and can be found in the sub-directories of the  /Game/ directory): 
* GameDataEbl
* HqChrEbl
* HqMapEbl
* HqObjEbl
* HqPartsEbl

The following file formats are used in the archives:

Name       | Explanation
---------- | -----------
.bnd       | General purpose archive
.texbnd    | Texture archive
.dcx       | Compressed file archive
.tae       | Character related
.esd       | AI state machine
.dclbnd    | Decal/Texture archive
.list      | Map id and name table
.fmg       | String table
.vsd       | Voice synchronization
.ini       | Settings
.fltbnd    | Filter archive
.tpf       | Texture
.breakobj  | Destructible map objects
.fltparam  | Filter settings
.msb       | Map related
.ngp       | Map related
.gibhd     | Model archive header
.hkxbhd    | Havok Engine archive header
.mapbhd    | Map archive header
.tpfbhd    | Texture archive header
.tpfbdt    | Texture archive
.param     | General purpose lookup-table
.acb       | Map model/texture related
.pfbbin    | Model related
.fxo       | HLSLShader
.bin       | Word filter
.ffxbnd    | SFX archive
.xpu       | GUI HLSL Shader
.xvu       | GUI HLSL Shader

#Additional notes:
There are 3 file formats that are used for many purposes.
* BND4 or Binder 4 is another game archive format. It contains a table of all the files that it contains and their file names. 
* PARAM is a game archive that is is used to store easy accessible settings for many aspects of the game. Each map has its own set of param files that contain information about events, spawnpoints and objects. Most param files are stored in the file regulation.bnd. This file contains the so called calibrations (i.a. the stats of items, enemies and NPCs). It can be found in the /Game/ directory and is encrypted with AES-128. Each PARAM file consists of a lookup-table and a set of values of the same size. The size of each value depends on which param type is used (Each file contains the name of the type in its header).
* DCX is a format that is used to contain compressed data. e.g. the DEFLATE algorithm is used to compress the aforementioned calibration file.

In Dark Souls II each map has its own id. Majula for example has the id 10040000. FROM formats the map id like this mXX_XX_XX_XX. That means e.g. that files that are related to Majula have m10_04_00_00 in their name.

