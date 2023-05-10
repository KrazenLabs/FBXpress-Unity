
# FBXpress for Unity

One-click FBX exporter add-on for Blender 3.40+ compatible with Unity's coordinate and scaling system. Exported FBX files are imported into Unity with the correct rotations and scales.
It is based on the Blender to Unity Exporter made by EdyJ (https://github.com/EdyJ/blender-to-unity-fbx-exporter).
However, I wanted to streamline the process more for my development pipelines, so instead of having to set your options and export location on every export manually, you can now set them once and save them in your project and then do a quick export by only clicking a single menu option!

## How to install

1. Download the add-on file from the release page: [`fbxpress-unity.py`](https://github.com/KrazenLabs/FBXpress-Unity/releases).
2. In Blender go to Edit > Preferences > Add-ons, then use the Install… button and use the File Browser to select the add-on file.
3. Enable the add-on by checking the enable checkbox.

<p align="center">
<img src="/img/install.png" alt="Add-On Window">
</p>

## How to use

Go to scene properties to select your export settings. The settings are saved in your .blend file, so you can use different export settings for different projects.

<p align="center">
<img src="/img/export settings.png" alt="Export properties">
</p>

After that, you can export with a single click on the new export menu entry:

**File > Export > Unity FBX (.fbx)**

<p align="center">
<img src="/img/export menu.png" alt="Export menu entry">
</p>

Exports all Empty, Mesh and Armature objects in the current scene except those in excluded collections. The full hierarchy is properly preserved and exported, including local positions and rotations.

### Be aware that any pre-existing file with the same name will be automatically overwritten!

## How it works

The exporter modifies the objects in the Blender scene right before exporting the FBX file, then reverts the modifications afterwards.

Every object to be exported receives a rotation of +90 degrees around the X axis in their transform _without_ actually modifying the visual pose of its geometry and children. This is done in the root objects, then recursively propagated to their children (as they inherit a -90 rotation after transforming their parent). The modified scene is then exported to FBX using Blender's built-in FBX exporter with the proper options applied. Finally the scene is restored to the state before the modifications.

When Unity imports the FBX file all objects receive a rotation of -90 degrees in the X axis to preserve their visual pose. As the objects in the FBX already have a rotation of X+90 then the undesired rotation is canceled and everything gets imported correctly.

## Known issues

- Negative scaling is imported with a different but equivalent transform in Unity. Example: scale (-1, 1, 1) and no rotation is imported as scale (-1, -1, -1) and rotation (-180, 0, 0). In Unity this is equivalent, and may be changed to, the original scale (-1, 1, 1) and rotation (0, 0, 0).
- Child objects in instanced collections receive an unneeded 90 degrees rotation in the X axis. Clearing this rotation in Unity gives the expected result. ([#3](https://github.com/EdyJ/blender-to-unity-fbx-exporter/issues/3))
- Exporting right after deleting an object throws an exception. Workaround: select some object before exporting. ([#17](https://github.com/EdyJ/blender-to-unity-fbx-exporter/issues/17))


## About the authors

One-click version by:
Krazen
https://krazenlabs.com

Original version by:
Angel "Edy" Garcia<br>
[@VehiclePhysics](https://twitter.com/VehiclePhysics)<br>
https://vehiclephysics.com
