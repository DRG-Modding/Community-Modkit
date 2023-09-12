# DRG Community Modkit Project

## [Download](https://drive.google.com/file/d/1_7lOUkByxW4407ws0l08kVjPNRfIRdRD/view?usp=sharing)

## [Preview Video](https://youtu.be/0odKe6elOLQ)

**Current version:** `1.0`

**Based on the game version:** `U38 (Season 4) Patch 7`

**Based on the FSD-Template commit:** [U38P7](https://github.com/DRG-Modding/FSD-Template/commit/4b061ac1729be853146fe9fb5fa37167f0ab383d)

**Full opened project size:** `15GB`

## Usage

1. Unzip the modkit files into a folder name of your choice.

2. Copy your mod files into the `Content` folder.

3. Then:
    - If you have no interest in looking through the source code, just open the `FSD.uproject`.

    - If you wish to look through the source code, you will need to generate the project files by right clicking the `FSD.uproject` file and selecting `Generate Visual Studio project files`, then open `FSD.sln` in your IDE.

4. Have fun being able to access thousands of references at once!

**Note:** There are three folders starting with `_` in the modkit files, that are *not* game files:
- `_EditorBPs` contains editor utility blueprints and a debug map, that contains a grid of all the blueprint actors, static meshes & skeletal meshes in the game. Please note that this map will be laggy as the meshes are reconstructed with only the base level of detail (the highest quality)
- `_MapDumper` which is the mod responsible for dumping the game's maps
- `_ModHub` which is the latest version of the mod hub framework files

## UnrealLink

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

**Why is the drop pod so stretched and big in some of the reconstructed maps?**

> There is currently a bug with components, causing the drop pod blueprint actor in particular to be stretched and big. The drop pod has been manually resized/repositioned in a couple of the levels, but a proper fix is being worked on.

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

As well as the following animations that fail to import due to having only one frame:

```
Content\Enemies\HydraWeed\Assets\ANIM_HydraWeed_Heart_Open.uasset
Content\Enemies\HydraWeed\Assets\ANIM_HydraWeed_Hidden.uasset
Content\Enemies\Jellyfish\Animations\BlendSpace\ANIM_Jelly_Fish_Idle_Pose.uasset
Content\Enemies\Jelly_Breeder\Animations\ANIM_JellyBreeder_Idle_Frame.uasset
Content\Enemies\MuleInfected\Animations\ANIM_InfectedMule_Body_Collapse_Idle.uasset
Content\Enemies\RivalTech\PatrolBot\Animations\ANIM_PatrolBot_Rolling_Pose.uasset
Content\Enemies\RivalTech\Shredder\Assets\ANIM_Shredder_OpenState.uasset
Content\Enemies\Shark\Animations\ANIM_CaveShark_A_IdleFrame.uasset
Content\Enemies\Shark\Animations\ANIM_CaveShark_A_Jump_Bend.uasset
Content\Enemies\Shark\Animations\ANIM_CaveShark_A_Jump_Straight.uasset
Content\Enemies\Spider\Animation\ANIM_Spider_Boss_Heavy_BackarmorClosed_A.uasset
Content\Enemies\Spider\Animation\ANIM_Spider_Boss_Heavy_BackarmorOpen_A.uasset
Content\Enemies\Spider\Animation\ANIM_Spider_Boss_Heavy_WeakspotsClosed_A.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Down_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Left_Down_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Left_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Left_Up_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Right_Down_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Right_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Right_Up_Frame.uasset
Content\Enemies\Woodlouse\Animations\AimAssets\ANIM_WoodLouse_A_Look_Up_Frame.uasset
Content\GameElements\Drone\Animations\ANIM_Drone_Combat_Idle.uasset
Content\GameElements\GameEvents\AmberEvent\Models\ANIM_AmberEvent_Grinder_Folded_Idle.uasset
Content\GameElements\GameEvents\GuntowerEvent\ANIM_GunTower_Base_Tower_Closed_Idle.uasset
Content\GameElements\GameEvents\GuntowerEvent\GunTower_Module_AimingLMG\ANIM_GunTower_LMG_Closed.uasset
Content\GameElements\GameEvents\GuntowerEvent\GunTower_Module_RadialFire\ANIM_GunTower_RadialFire_Closed_Idle.uasset
Content\GameElements\GameEvents\GuntowerEvent\GunTower_Module_RadialFire\ANIM_GunTower_RadialFire_Open_Idle.uasset
Content\GameElements\GameEvents\GuntowerEvent\GunTower_Module_RandomFire\ANIM_GunTower_RandomFire_Closed_Idle.uasset
Content\GameElements\GameEvents\GuntowerEvent\GunTower_Module_RandomFire\ANIM_GunTower_RandomFire_Open_Idle.uasset
Content\GameElements\GameEvents\PlagueMeteor\RockCrackerModels\ANIM_RockCracker_Closed.uasset
Content\GameElements\GameEvents\RewardDispenser\EventButtons\ANIM_Event_StartButton01_Idle.uasset
Content\GameElements\GameEvents\RockEnemies\Models\ANIM_RockEnemyEvent_PowerUpSprinkler_FoldOut_Idle.uasset
Content\GameElements\Objectives\Facility\Caretaker\Body\ANIM_CaretakerBody_Open.uasset
Content\GameElements\Objectives\Facility\DefenseTurret\Animations\ANIM_Gun_Turret_Close_Pose.uasset
Content\GameElements\Objectives\Facility\DefenseTurret\Animations\ANIM_Gun_Turret_Open_Pose.uasset
Content\GameElements\Objectives\Facility\DefenseTurret\Assets\ANIM_Gun_Turret_01_ClosedPose.uasset
Content\GameElements\Objectives\Facility\DefenseTurret\Assets\ANIM_Sniper_Turret_01_InactivePose.uasset
Content\GameElements\Objectives\Facility\DefenseTurret\Assets\ANIM_Sniper_Turret_01_OpenPose.uasset
Content\GameElements\Objectives\Facility\DefensiveTentacles\Animations\ANIM_Defence_Tentacle_Head_Melee_Pose.uasset
Content\GameElements\Objectives\Facility\DefensiveTentacles\Animations\ANIM_Defence_Tentacle_Head_Ranged_Attack_Idle_Pose.uasset
Content\GameElements\Objectives\Facility\Tethers\Assets\ANIM_HackingPod_Base_Closed_Idle.uasset
Content\GameElements\Objectives\Facility\Tethers\Assets\ANIM_HackingPod_NodeDispenser_ClosedFrame.uasset
Content\GameElements\Objectives\Facility\Tethers\Assets\ANIM_HackingPod_NodeDispenser_NoTetherFrame.uasset
Content\GameElements\Objectives\Facility\Tethers\Assets\ANIM_HackingPod_NodeDispenser_Open_Idle.uasset
Content\GameElements\Objectives\Facility\Vault\ANIM_Vault_ClosedState.uasset
Content\GameElements\Objectives\Facility\VaultShieldEmitter\ANIM_VaultShieldEmitter_Closed.uasset
Content\GameElements\Objectives\HackBuilding\Assets\ANIM_ProspectorDataDeposit_Closed_Idle.uasset
Content\LevelElements\Minehead\ANIM_Minehead02_Closed.uasset
Content\LevelElements\Minehead\ANIM_Minehead02_Open.uasset
Content\WeaponsNTools\CoilGun\Animation\FP_Pistol_CoilGun_A_Closed_Idle.uasset
Content\WeaponsNTools\Extractor\Fuel_Cannister\ANIM_FuelCannister_B_NotShooting.uasset
Content\WeaponsNTools\SupplyPod\Assets\SupplyPod_Barrel\ANIM_SupplyPod_Barrel_Dispenser_NoBarrelFrame.uasset
```

## Known issues

- Gameplay Tags in blueprints are all "None", so you will need to change those manually if you intend on using them as references
- Animation sequences have no track data, so they won't play any sounds or particles
- Some animations are missing as shown in the above list - there must be something wrong with the FBX export pipeline as these animations should not be invalid for just having one frame
- Component hierarchies for some blueprints are incorrect, such as the droppod's root component being wrong - fix is currently in the works, and will change the dummy C++ classes
- Some static meshs have the wrong material order - this may cause problems when merging static meshes together
- Physics assets aren't generated with full properties - this has been noticed after modkit distribution and will be fixed in the next version

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
