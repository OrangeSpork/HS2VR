# About

Somewhat rough but functional VR mod to allow HS2 VR fun.

Credit to killmar-the-3rd for the initial implementation, this builds on his work. https://github.com/killmar-the-3rd/HS2VR
Credit for the underlying library (VRGIN) goes to Eusth (https://github.com/Eusth/VRGIN) and Ooetksh for their AI-Shoujo version.
The present Honey Select 2 VR mod is based off (https://github.com/Ooetksh/KoikatuVR). 

# What Works?

Everything, albeit with some quirks. Main game, Maker and Studio are all playable. The full game experience, only in 3D.

# Specific Improvements from V 0.2.0

Plugin coexists with the official VR plugin now, you can have them this installed and still use official VR as you wish.

Seated mode works (and is the new default). This mode is VR + mouse/keyboard, controllers not needed. Controls are the same as the 2D game except the UI floats in front of your face. Use the mouse and keyboard to drive. Switch to Standing mode (where the UI is attached to the controllers) by pressing Ctrl+C, Ctrl+C (double tapping ctrl-c). You can change the default mode (and various other stuff) in the VRSettings.xml file in game root.

Maker now functions and has a visible UI.

Major camera improvements for both seated and standing. Basically everything should now work as expected, no more ending up in strange places and facing backwards. Title, lobby and dialog scenes will attempt to adjust for the FOV change by moving the camera closer to the girl so she occupies the expected amount of view space (otherwise you feel weirdly far away). Note that the fixed VR FOV causes some camera positions to be a bit amusing (like the 2D view of the ledger books that in 3D has you standing on the counter instead).

Camera buttons in studio now work, as do the in scene movable camera views, though selecting the in scene cameras can be fun since the dropdown is invisible.

# Known Issues

Switching modes (Seated to Standing or vice versa) results in an awkward camera height, simply switch scenes or camera views to correct it.\
It's possible to come out of an HScene or Maker with the view tilted. If that happens switch modes and then switch back (Ctrl+C, Ctrl+C - double tap control c to switch).\
3D space studio object manipulation is basically impossible as the raycast source and your eyeballs are now different. You can use the move controller and HS2PE to work around a lot of this, but there are limits.\
Certain GUI elements in studio have a raycast component that renders them invisible in VR. Specifically the scene settings, camera light and the in-scene camera selection drop down. They are present and actually clickable just not visible. Maniputlation of scene settings will probably need to be done outside of VR unless you have incredible spatial memory. Camera light can be adjusted in Graphics instead. The camera selector drop down isn't too hard to click blind...

# Notes on included HS2Graphics

While we wait for Hooh to overhaul Graphics and release HoohSelect2Graphics - The Visual Experience, this is a duct-taped copy of HS2Graphics with support for VR. It's equivalent to the HS2Graphics included with Hanmens SSS packs (and supports that functionality) but has various fixes for VR functionality (including SSS skins in VR). It also adds a preset system that allows you to designate a preset for various modes which will be reapplied as the game jumps between scenes. No more need to keep reactivating SSS everytime the scene changes.

This works for 2D and both the official and unofficial VR modes.

Some notes on VR graphics settings:

Since the VR interface is in scene TAA applies to it and tends to make it a bit blurry, probably stick to SMAA in scenes (though I break this guideline all the time...)\
For best VR performance I recommend turning off MSAA and Realtime Reflection Probes (on Settings) and lowering the SSS settings Postprocess Iterations to 2 and Shader Iterations per pass to 4. Deactivating dither may help, although I didn't notice a big difference.

Note: Be careful with shadows, it might be a quirk of my setup but turning off shadows cuts my frame rate in half. Make sure shadows are on in graphics and in any other mods touching the graphics settings.

# Simple Installation

Requires HS2DX (will not work with vanilla HS2), BepInEx

Unzip the HS2VR.zip file into your base game directory. 
Either patch the globalgamemanagers in HoneySelect2_Data and StudioNEOV2_Data yourself or use the ones from the release section. MAKE SURE YOUR GAME VERSION MATCHES THE GLOBALGAMEMANAGERS VERSION.

Optionally (but recommended), install the included version of HS2Graphics (same as the latest SSS version of HS2Graphics...but with VR support and related QOL improvements).




# Installation Detail
## Enabling VR


- Get UABE (https://github.com/DerPopo/UABE).
- Open `HoneySelect2_Data/globalgamemanagers`
- Find the asset of Type BuildSettings
- Export Dump
- Open the exported dump with a text editor and change the lines

```
0 vector enabledVRDevices
0 Array Array (0 items)
0 int size = 0
```
to
```
0 vector enabledVRDevices
0 Array Array (2 items)
0 int size = 2
[0]
1 string data = "None"
[1]
1 string data = "OpenVR"
```
- Import Dump and save. It won't allow you to overwrite the file in the game directory, so save it somewhere else and make a backup of your original `globalgamemanagers` before copying your patched version into `HoneySelect2_Data`.

You can also find a pre-patched `globalgamemanagers` on discord.

To check that VR is working, start Steam VR and run `HoneySelect2.exe` with the the option `-vrmode "OpenVR"`. This should show the game in VR, albeit without any UI.

## Install the mod

Copy `BebInEx` and `HoneySelect2_Data` folders into your game directory. That should deposit `HS2VR` in `BebInEx/plugins` and `openvr_api.dll` in `HoneySelect2_Data/Plugins`.

If SteamVR is running, the game should start in VR mode. If you need to force the game into non-VR mode while SteamVR is running and don't want to disable the VR mod for whatever reason, pass the `--novr` command line argument.

## Controls
The controls follow the `VRGIN` setup: 
- The B/Y buttons switch the current tool.
- The menu tool gives access to the UI. Use the grip button to grap the UI plane and reposition it.
- The warp tool allows movement. Grip button moves the camera, grip+trigger rotates it. Clicking the touchpad warps the camera to the indicated location.
- The play tool allows for interaction during H-Scenes. For now, moving the touchpad up/down acts as the mouse wheel for starting, speed up, and speed down. Pressing touchpad left/right cycles through the finish options, pressing in the center actives the current active option.

# Building from source

The mod currently uses a patched `VRGIN.dll` from the AI-Shoujo VR mod by Ooetksh, not the included git submodule. Once that version of VRGIN makes it onto github, the repo can be updated to include it.

# Known issues
- Camera placement is buggy. The effects of Unity XR input tracking, SteamVR, and whatever VRGIN is doing on the various cameras is less than clear to me. If someone knows how that stuff works, please let me know.
- During scene transitions seizure-iduction flickering is happening. Until that is fixed, just close your eyes ¯\\_(ツ)_/¯.
- Mirrors don't work properly. 
- Resolution of UI is crap.
