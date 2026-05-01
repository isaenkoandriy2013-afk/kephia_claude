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

### Unity PartTools export gotchas

- **Scale must be `1, 1, 1`** on the object's Transform before exporting. Exporting at e.g. `100,100,175` bakes that scale into the .mu — the object will be enormous in-game and Parallax bounds calculations may silently fail.
- **Select the child mesh object**, not the root FBX object, before clicking KSP → Write to file. Exporting the root produces a valid-looking .mu that contains no geometry (95 bytes or similar tiny size).
- **Unity 2019 does not import DDS textures.** Convert textures to PNG before importing into the Unity Assets folder for previewing/assigning materials. The actual in-game textures referenced in the Parallax scatter config are still the original DDS files in GameData — the PNG is only for Unity's material preview.
- **A correctly exported .mu with a mesh is typically 10–15 KB+.** A file under ~1 KB almost always means the root object was selected instead of the child mesh.
- **Position and rotation must be 0,0,0 before exporting.** Any baked-in position offset will cause the scatter to spawn underground. Any baked-in rotation will tilt the model sideways in-game. Always zero out Transform before clicking Write.
- **Blender FBX axis settings:** Export with **Y Forward, Z Up**. In Blender, rotate the mesh in Edit Mode so the tip points up (+Z), base sits on the grid (Z=0), then Apply All Transforms before exporting. Unity will show rotation as ~-90° X due to axis conversion — this is normal and correct.
- **frustumCullingStartRange** should be close to the scatter's render range for large objects. Too small (e.g. 10) causes objects to flicker or disappear when the camera moves. Start at ~2500 for large scatters with range=3000.

---

## Config Hygiene

### VertexColorMap texture must match the body

The `VertexColorMap` map path in `PQS/Mods` must use the body's own color texture (`{BodyName}Clr.dds`), not a copied value from another body. Copy-paste from another body's config is a common source of this bug.
