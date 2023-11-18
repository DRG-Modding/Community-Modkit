# DRG Community Modkit Project

## [Download](https://drive.google.com/file/d/1fHSIPI14A9GJhrFsSjlh8e2t06AECYAO/view?usp=sharing)

## [Preview Video](https://youtu.be/0odKe6elOLQ)

**Current version:** `1.1`

**Based on the game version:** `U38 (Season 4) Patch 10 (Maintenance Update)`

**Based on the FSD-Template commit:** [U38P10](https://github.com/DRG-Modding/FSD-Template/commit/0da9e507ede3c5b71de929c1691337bd069a7a48)

**Full opened project size:** `15GB`

## What is this for?

The modkit is almost an exact mirror of DRG's original Unreal project without any of the code implementations. It can be useful for:
- Blueprint modding, when you are making a mod that needs to get references to various game assets or create new ones based off existing assets
- Animation modding, when you want to test out your animations on the skeletons/skeletal meshes already there
- Skin/texture/material/model modding, for testing your assets on existing game assets and meshes in the editor
- Map modding, for example editing the spacerig or making a new map using existing DRG assets

With this modkit, you can make your own content inside of the editor in almost the same way Ghost Ship Games can when developing the game - minus the ability to test blueprint or map mods inside of the editor due to there being no code implementations.

As an example, the [Mission Content Revise](https://mod.io/g/drg/m/massive-content-revise-testing) mod contains thousands of new and edited assets and would not be as good as it is today without the modkit.

If you wish to use this modkit for learning or modding purposes, you __must__ have followed at *least* the [basic modding guide](https://mod.io/g/drg/r/drg-basic-modding-guide) first! If you are using this for blueprint modding, you __must__ have followed the [blueprint modding guide](https://mod.io/g/drg/r/how-to-blueprint-mod) too!

## Usage

1. Unzip the modkit files into a folder name of your choice.

2. Copy your mod files into the `Content` folder.

3. Then:
    - If you have no interest in looking through the source code, just open the `FSD.uproject` (you need UE4.27 installed to open).

    - If you wish to look through the source code, you will need to generate the project files by right clicking the `FSD.uproject` file and selecting `Generate Visual Studio project files`, then open `FSD.sln` in your IDE.

4. Have fun being able to access thousands of references at once!

**Note:** There are three folders starting with `_` in the modkit files, that are *not* game files:
- `_EditorBPs` contains editor utility blueprints and a debug map, that contains a grid of all the blueprint actors, static meshes & skeletal meshes in the game. Please note that this map will be laggy as the meshes are reconstructed with only the base level of detail (the highest quality)
- `_MapDumper` which is the mod responsible for dumping the game's maps
- `_ModHub` which is the latest version of the mod hub framework files

## UnrealLink

While most people use Visual Studio for opening the project solution as it is free, there is also another option if you have access to it anyway. 

The [Jetbrains Rider](https://www.jetbrains.com/rider/download) IDE has a special system called [Unreal Link](https://www.jetbrains.com/help/rider/Unreal_Engine__UnrealLink_RiderLink.html) that enables advanced integration between the IDE and the editor. Here's an example of what UnrealLink could look like if you use it:

![Unreal Link Example](https://cdn.discordapp.com/attachments/1109192354595876944/1150799779316629635/image.png)

You can also change your IDE inside of `Editor preferences` -> `Source code`. Then, when you click on the base class of an asset, it will open the C++ class in your IDE.

## Modding

Any C++ changes can be built like normal, just remember that there are now assets in the project that may break. This is why Unreal Link is so useful - you can use it to find out which assets may be affected by your changes.

**Editing an existing game asset is not recommended, unless you know what you are doing!** Usually creating a child actor override of the asset you wish to edit is the best way to do it. This prevents possible mod conflicts.

## FAQ

**Why do blueprints have no code?**

> Blueprints are compiled into kismet bytecode, which currently is not reconstructed back in the editor as it is an extremely difficult problem to tackle.
>
> If you wish for an alternate, human-readable way to view the kismet bytecode, you can use [this tool](https://trumank.github.io/drg-control-flow-graphs/).

**Why are the materials and particles all white/fuzzy looking?**

> Unfortunately for us, the GSG are smart about their materials and used material parameters and switches in all of the base materials, which makes it basically impossible for us to reconstruct them automatically. This is because the parameter data is stored in the `globalshadercache.bin` file, rather than in the cooked `.uasset` data. 
>
> By extension, because the particles are using these partially reconstructed materials, they too will look all weird.
> 
> However, you can manually "fix" a material in-editor if you know what it looks like in-game. There is a [mod](https://cdn.discordapp.com/attachments/709825862672973866/998271202546171955/MatTestMod.zip) that can help you with this by spawning a textured cube with every base material in the game.

**When I open a reconstructed map, why is it all black?**

> You need to turn off post process material by going to `Show` -> `Post Processing` -> `Post Process Material` in the editor viewport.

![How to turn off post process material](https://cdn.discordapp.com/attachments/1109192354595876944/1151018604129701888/image.png)

**Why can't I open particles or sound waves?**

> These asset types are actually the cooked files copied directly from the unpacked game files, since they work just fine in-editor as references. Cooked files cannot be opened or saved in the editor.
> 
> However, if you are feeling lucky or dangerous (that may cause cook failures), you can use the editor utility blueprint in `_EditorBPs` called `CookedToUncooked`. This will take any selected assets and "uncook" them in-place. This is very slow, and again, will very likely cause cook failures or even editor crashes.

**What's with the blueprint actors in the debug world that look like big poles?**

> These are the terrain carvers responsible for making special shapes in the terrain. As it is currently understood, their shapes are set during runtime by the static mesh carver (SMC) files.

**Why are some assets missing?**

> Some asset types are missing because the code to reconstruct them has not been written yet, or for some other reason. Please see the below section on missing files for more detail.

## Missing files

There are a few files that are in the game content but are not in the modkit content. These are:
- Movies
- Niagara assets
- Media players
- File media sources
- Static mesh carvers (those that begin with `SMC_` in the unpacked files)

## Known issues

- Gameplay Tags in blueprints are all "None", so you will need to change those manually if you intend on using them as references
- Animation sequences have no track data, so they won't play any sounds or particles
- Some static meshs have the wrong material order - this may cause problems when merging static meshes together

## Credits

Buckminsterfullerene - Modkit creator (you can find the repo of helper files used to generate it [here](https://github.com/DRG-Modding/Modkit-Helpers))

Credits related to [UE4SS UHT Generator](https://docs.ue4ss.com/guides/generating-uht-compatible-headers.html), the UE4SS utility used to generate the dummy C++ headers (FSD-Template):
- Archengius
- Narknon
- Buckminsterfullerene

Credits related to [UEAssetToolkitGenerator](https://github.com/LongerWarrior/UEAssetToolkitGenerator), the program used to generate JSON data about the assets from the unpacked files:
- LongerWarrior
- Buckminsterfullerene
- atenfyr

Credits related to [UEAssetToolkit](https://github.com/Buckminsterfullerene02/UEAssetToolkit-Fixes), the UE plugin used to generate the assets in-editor from the JSON data:
- Archengius
- Mircearoata
- Buckminsterfullerene
- LongerWarrior

Credits related to the [FBX export pipeline](https://github.com/LongerWarrior/UEAssetToolkitGenerator/wiki/Generating-FBX):
- CUE4Parse developers, Zain, Buckminsterfullerene (for the ActorX mass export script)
- Zain, matyalatte, Befzz (for the FBX mass export blender scripts)

## Legal stuff

GSG have officially approved the creation and distribution of this to the DRG Modding community, as long as you adhere to their [UGC policy](https://drg.mod.io/guides/deep-rock-galactic-ugc-policy) (i.e. don't use the content for anything other than DRG-related production).