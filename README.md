# SCP: ReEnter Modding API (Binary SDK)

Precompiled Blueprint SDK for creating content-only mods for **SCP: ReEnter** with Unreal Engine 5.7.4 on Windows.

## Requirements

- The exact stock Unreal Engine 5.7.4 build supported by the game.
- A Blueprint/content-only Unreal project.
- No native mod DLLs or C++ gameplay modules are supported.

## Installation

1. Close Unreal Editor.
2. Copy this repository to `<YourProject>/Plugins/SCPModAPI`.
3. Enable **SCP Mod API** in the project.
4. Reopen the project and create mod assets under `/Game/Mods/<MOD_ID>/`.

The plugin is intentionally distributed without C++ source code. Do not include the plugin DLL in a Workshop mod pak; package only your mod content and descriptor.

Full documentation: https://scpreenter.com/docs/workshop

## Compatibility

This binary SDK is tied to Unreal Engine 5.7.4 for Win64. Download a matching SDK release when the game or its public mod API changes.

