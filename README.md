# Juan de Austria: exclusive custom skin for Marta (Hellish Quart)

A fan-made skin that works only for Marta. It runs on Marta's native 68-joint skeleton (Mafra5/Marfa), with her original moves and rapier.

- **Same proportions as the source model.** Juan de Austria's shoulders are 8 cm higher than Marta's, so the whole model is scaled to her shoulder height (×0.945). The neck and torso are never stretched or squashed.
- **Clean shoulders.** The T-pose source and its own skin weights fit Marta's limbs and hands, including palm orientation.
- **Rigid head and hair.**
- **No ghost body when hit.** Marta's 17 wound overlay meshes (`DecalBloody`, `DecalSpine1`, ...) are shaped like her own body. The visibility controller keeps them hidden under the skin, together with her stock face.
- **Sharp textures within the game budget.** The 2M-triangle source is reduced to 80k triangles. UV seams are welded first, so the reduced mesh shows the texture without cracks. Each part stays under the 65k vertices the game's wound decal builder supports.

## Release

The zip contains only the four bundles (`skin_marta_juandeaustria`, `pic_marta_juandeaustria` and their `.manifest` files) and `README.txt`. There are no scripts. Install by copying the four files into `Hellish Quart_Data/StreamingAssets/`. `README.txt` also documents the optional manual first-person (FPP) adjustment.

## Layout

- `model/JuanDeAustria_Marta.blend`: the fitted result on Marta's skeleton, editable in Blender 5.3.
- `model/references/`: the four reference views.
- `model/selector_portrait.png`: the selector picture (the period portrait, upscaled 2× with Lanczos).
- `model/JuanDeAustria_source.glb` is not in the repository: it is 157 MB, over GitHub's file limit. Place it there to rebuild from scratch.
- `blender_work/`:
  - `step1_prepare.py` welds, decimates and cleans the source, transfers its rig weights and exports the textures;
  - `step3_rig_marta.py` scales the model, fits Marta's limbs and hands, carries the mesh into Marta's bind pose and splits off the rigid head, with QA renders including a twisted spine;
  - `verify_skeleton.py` checks the 68 bones and the weights.
- `unity_project/`: Unity 2019.4.0f1 project (menu **JuanDeAustria → Construir mod**).
- `release/README.txt`: the text shipped in the zip.

## Rebuild

Copy `Hellish Quart_Data/Managed/PlayMaker.dll` from your own installation to `unity_project/Assets/Plugins/`, then run:

```powershell
$blender="C:\Program Files\Blender Foundation\Blender 5.3\blender.exe"
$skel="unity_project\Assets\JuanDeAustriaMod\Editor\MartaSkeleton.json"
& $blender -b --factory-startup -P blender_work\step1_prepare.py -- model\JuanDeAustria_source.glb model\JuanDeAustria_source.glb build
& $blender -b --factory-startup -P blender_work\step3_rig_marta.py -- build $skel
& $blender -b --factory-startup -P blender_work\verify_skeleton.py -- build\marta\Skin.fbx $skel
.\build.ps1
```

`build.ps1` is a developer build script and is never part of the release. It runs the Unity batch build, copies the four bundles into `release/` and creates `JuanDeAustria_Marta.zip`.
