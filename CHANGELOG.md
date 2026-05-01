# CHANGELOG.md

---

## 2026-05-01 (session 3)

### Fixed
- `Models/Elaris/ElarisSpike001.mu` — fixed underground spawning caused by baked-in position offset (Y: -0.47, Z: -0.31) and wrong rotation (-102° X). Re-exported from Blender with mesh base at origin (Z=0), tip pointing up, then FBX exported with Y Forward/Z Up axes. Unity rotation shows as -89.98° X due to axis conversion but mesh is correctly oriented.
- `Misc/Parallax/Elaris/Parallax_Elaris_Scatters.cfg` — fixed spikes disappearing when camera moves: changed `frustumCullingStartRange` from 10 to 2500, `frustumCullingScreenMargin` from 200 to 100. Fixed close-up disappearing: reduced first LOD range from 1500 to 500. Updated `minScale`/`maxScale` to 80-120 to compensate for true-scale mesh export.

---

## 2026-05-01 (session 2)

### Fixed
- `Models/Elaris/ElarisSpike001.mu` — re-exported from Unity at scale `1,1,1` (was `100,100,175`), selecting the child mesh object instead of the root FBX object. Previous export was identical file size due to scale being stored as floats, but geometry bounds were broken causing Parallax to silently skip rendering.
- `Misc/Parallax/Elaris/Parallax_Elaris_Scatters.cfg` — temporarily set ElarisSpikes distribution to max spawn (spawnChance=1.0, populationMultiplier=5, minScale=10, maxScale=20, no altitude/slope restrictions) to diagnose visibility issue.
- `Misc/Parallax/Elaris/Parallax_Elaris_PQSMod.cfg` and `Phion/Parallax_Phion_PQSMod.cfg` — reduced `subdivisionLevel` from 7 to 5 to reduce performance overhead.
- Deleted stray `GameData/ets/` folder containing a misplaced copy of `ElarisSpike001.mu`.

---

## 2026-05-01

### Added
- `Models/Elaris/ElarisSpike001.mu` — custom spike model for Elaris, exported from Blender via Unity PartTools
- `Models/Elaris/ElarisSpike001.cfg` — placeholder config to ensure KSP GameDatabase indexes the folder
- `Misc/Parallax/Elaris/Parallax_Elaris_Scatters.cfg` — added `ElarisSpikes` scatter using the custom model, with 2 LOD nodes (range 200/400), `_CullMode = 2`, and distribution across -500 to 8000m altitude

### Fixed
- `Misc/Parallax/Elaris/Parallax_Elaris_Scatters.cfg` — ElarisSpikes scatter was missing a second LOD node (Parallax requires exactly 2); added matching LOD entry to resolve "unable to locate 2 LOD nodes" critical error
- Renamed model file from `ElarisSpike0.01.mu` to `ElarisSpike001.mu` — dots in the filename stem prevented KSP GameDatabase from loading the asset
- Moved model from `Textures/Meshes/` to `Models/Elaris/` — established correct folder convention for custom scatter models

---

## 2026-04-30

### Added
- `Misc/EVE/Zunk_clouds.cfg` — volumetric cloud config for Zunk with two layers: high cirrus (11–13.5 km) and main tropospheric clouds (Stratus, Cumulus, Congestus at 2.2–6.4 km). Uses StockVolumetricClouds built-in textures.

### Fixed
- `Misc/Scatterer/Atmospheric/Zunk/Scatterer_Zunk.cfg` — set `usesCloudIntegration = True` and `cloudIntegrationUsesScattererSunColors = True`. Clouds were rendering fully black because Scatterer was not passing lighting to EVE.
