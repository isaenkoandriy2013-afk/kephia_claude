# CHANGELOG.md

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
