Dark-Souls-II-Archive-Dump
==========================
A partial dump of the encrypted game archives (PC Version 1.0.7) for those that are interested in examining the game assets.

This repository should contain all the files that are required to load a savegame at the Majula bonfire.
Most of the files are stored in the following archives (Some files aren't packed and can be found in the sub-directories of the  /Game/ directory): 
* GameDataEbl
* HqChrEbl
* HqMapEbl
* HqObjEbl
* HqPartsEbl

The following archive file formats are used:

Name       | Description
---------- | -----------
.anibnd    | Animation archive
.bdt       | General purpose archive (headerless)
.bhd       | General purpose archive header
.bnd       | General purpose archive
.commonbnd | General purpose archive
.dclbnd    | Decal archive
.dcx       | Compressed file archive
.envbnd    | Map environment archive
.febnd     | Frontend archive
.fetexbnd  | Frontend texture archive
.ffxbnd    | SFX archive
.fltbnd    | Filter archive
.fontbnd   | Font archive
.gibhd     | Model archive header
.hkxbhd    | Havok Engine archive header
.mapbhd    | Map archive header
.texbnd    | Texture archive
.tpfbdt    | Texture archive (headerless)
.tpfbhd    | Texture archive header

The following file formats are used in the archives:

Name       | Description
---------- | ------------
.acb       | Unknown (Map model/texture related)
.bin       | Word filter
.bmp       | Bitmap
.breakobj  | Destructible map objects
.btl       | Unknown (Map related)
.btpb      | Unknown (Map/Point could related)
.ccm       | Unknown (Font related)
.clm       | Unknown (Cloth related)
.ctl       | Unknown (FaceGen related)
.egt       | Unknown (FaceGen related)
.emevd     | Special effect table
.esd       | AI state machine
.fev       | Audio
.fltparam  | Filter settings
.flvpwv    | Unknown (Model related)
.fmg       | String table
.fpo       | Unknown (Shader related)
.fsb       | Sound
.fxo       | HLSLShader
.hkx       | Unknown (Havok related)
.hkxpwv    | Unknown (Havok related)
.ini       | Settings
.itl       | Sound
.list      | Map id and name table
.msb       | Unknown (Map related)
.mtd       | Material
.mte       | Unknown (Map related)
.mte       | Unknown (Model related)
.ngp       | Unknown (Map related)
.nmb       | Unknown (Animation related)
.nsa       | Animation
.param     | General purpose lookup-table
.pem       | RSA public key container
.pfbbin    | Unknown (Model related)
.sfxparam  | Sound settings
.tae       | Unknown (Character related)
.tpf       | Texture
.vpo       | Shader
.vsd       | Voice synchronization
.xml       | Shader information
.xpu       | GUI HLSL Shader
.xvu       | GUI HLSL Shader

#Additional notes:
There are 3 file formats that are used for many purposes.
* BND4 or Binder 4 is the most used archive format. It contains a table of all the files that it contains, their ASCII or UNICODE file names and sometimes even the full path from when they were packed. 
* PARAM is a game archive that is is used to store easy accessible settings for many aspects of the game. Each map has its own set of param files that contain information about events, spawnpoints and objects. Most param files are stored in the file regulation.bnd. This file contains the so called calibrations (i.a. the stats of items, enemies and NPCs). It can be found in the /Game/ directory and is encrypted with AES-128. Each PARAM file consists of a lookup-table and a set of values of the same size. The size of each value depends on which param type is used (Each file contains the name of the type in its header).
* DCX is a format that is used to contain compressed data. e.g. the DEFLATE algorithm is used to compress the aforementioned calibration file. Judging from the DFLT signature in the header of each file it could be possible that other compression algorithms can be used.

In Dark Souls II each map has its own id. Majula for example has the id 10040000. FROM formats the map id like this mXX_XX_XX_XX. That means e.g. that files that are related to Majula have m10_04_00_00 in their name. Valid map ids can be found in WorldMapList.list or MapName.fmg.

#Encryption

##Regulation: 
&nbsp;      | &nbsp;
----------- | ----------------------------------------------------------------
Encryption  | AES-128 CTR
Key         | 40178130DF0A94543309E171ECBF254C
IV          | 80DD6C870F2017915B4CFD8F00000001 (Valid for calibration 1.0.12)


##Savegames:
&nbsp;      | &nbsp;
----------- | ----------------------------------------------------------------
Encryption  | AES-128 CBC
Key         | B7FD463E4A9C1102DF1739E5F3B2A50F
IV          | First 16 bytes in the USER_DATA000-USER_DATA022 files
Checksum    | Second 16 bytes in the USER_DATA000-USER_DATA022 files (Unknown algorithm)

##Archive headers:
&nbsp;      | &nbsp;
----------- | ----------------------------------------------------------------
Encryption  | RSA 2048
Key         | Public keys are stored in PKCS#1 PEM files
Remarks     | The game uses OpenSSL and Eric Young's PKCS#1 RSA_eay_public_decrypt function

##Archive data:
&nbsp;      | &nbsp;
----------- | ----------------------------------------------------------------
Encryption  | AES-128 ECB
Key         | Keys are stored in the header file
Hash        | SHA-256
Salt        | Stored in the header file (e.g. FRPG2_STEAM_GAME_DATA_EBL_SALT for GameDataEbl.bhd)

#Hashing

##File name hash algorithm

1. Remove the prefix
  1. Find the first occurrence of ':'
    * Abort if no ':' was found
    * Continue with the substring after ':'
2. Initialize the hash with 0
3. Iterate through the substring
  1. If the character is uppercase then convert it to lowercase
  2. If the character is '\' then replace it with '/'
  3. Multiply the hash with 37
  4. Add the numeric representation of the character to the hash

##File name hash example
File name                                     | Archive      | Hash
--------------------------------------------- | ------------ | -----------
map:/param/GeneratorParam_m10_02_00_00.param  | GameDataEbl  | 2779221649
