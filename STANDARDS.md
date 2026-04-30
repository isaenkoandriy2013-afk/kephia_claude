# STANDARDS.md

Standards, gotchas, and rules discovered through working on KephiaSystem.

---

## EVE Volumetric Clouds

### Scatterer integration is required for clouds to be lit

Any body with EVE volumetric clouds (`layerRaymarchedVolumeV5`) **must** have these two flags set in its Scatterer planet list entry:

```cfg
usesCloudIntegration = True
cloudIntegrationUsesScattererSunColors = True
```

If `usesCloudIntegration = False`, Scatterer does not pass lighting to EVE. The clouds will render completely black regardless of EVE cloud config values (`skylightMultiplier`, `lightMarchDistance`, `density`, etc.). This is not fixable from the EVE side.

**File location:** `Misc/Scatterer/Atmospheric/<BodyName>/Scatterer_<BodyName>.cfg`

### lightMarchDistance should be kept low

Values above ~200 cause harsh internal self-shadowing that creates black patches inside cloud volumes. Recommended starting value: `100`. Raise only if clouds look flat/shadowless.

### density starting values

| Cloud type | Recommended starting density |
|---|---|
| Cirrus | 0.0005 – 0.001 |
| Stratus | 0.0008 – 0.002 |
| Cumulus / Congestus | 0.004 – 0.008 |

High density (0.012+) combined with any `lightMarchDistance` will produce black cloud cores.

---

## Parallax Custom Scatter Models

### .mu files must be exported from Unity via PartTools — not renamed

A `.mu` file is a Unity asset bundle. Renaming a Blender `.fbx` or any other file to `.mu` will not work — KSP cannot read it and Parallax will throw "model file doesn't exist" even if the file is present on disk.

**Correct workflow:**
1. Export model from Blender as `.fbx`
2. Import `.fbx` into Unity 2019.4.18f1 with PartTools installed
3. Add the **Part Tools** component to the object in the Hierarchy
4. Set **Model Name** and **File URL** (pointing to your `GameData/` subfolder)
5. Click **Write** to export the `.mu`

**Model path rules:**
- Place models under `KephiaSystem/Models/<BodyName>/` (e.g. `KephiaSystem/Models/Elaris/`)
- Reference in config without the `.mu` extension: `model = KephiaSystem/Models/Elaris/ElarisSpike001`
- Parallax requires exactly 2 LOD entries per scatter — use the same model path for both LODs if only one mesh exists
- Avoid dots in model filenames (e.g. `ElarisSpike0.01` fails; use `ElarisSpike001` instead)

---

## Config Hygiene

### VertexColorMap texture must match the body

The `VertexColorMap` map path in `PQS/Mods` must use the body's own color texture (`{BodyName}Clr.dds`), not a copied value from another body. Copy-paste from another body's config is a common source of this bug.
