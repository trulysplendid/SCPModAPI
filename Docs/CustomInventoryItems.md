# Custom inventory items

## Create a definition

In the Content Browser, choose **Miscellaneous → Data Asset → SCP Mod Item Definition**. Save the definition under `/Game/Mods/<MOD_ID>/Items`, for example `/Game/Mods/MyMod/Items/DA_Bandage`.

Set a stable `ItemId` such as `Bandage`. The game derives the full ID `ModItem:MyMod.Bandage` from the mod folder and item ID. Mod and item IDs accept ASCII letters, digits and underscores. Keep the ID and asset path stable after publishing.

Set `DisplayName`, `InventoryIcon` and `WorldMesh`. The world mesh needs simple collision for pickup physics. Use **Simple And Complex** for its collision complexity. Keep the icon, meshes, sounds and behavior Blueprints inside the same mod folder and include their directories when cooking.

Use `HeldMesh` to override the in-hand model. It defaults to `WorldMesh`. Configure `FirstPersonTransform` for the static first-person view. Set `ThirdPersonSocket` (default `hand_r`) and `ThirdPersonTransform` for the model other players see.

## Set the use effect

| Effect | Result |
| --- | --- |
| `None` | The item can be collected and carried. |
| `Heal` | Restores `EffectAmount` health. |
| `RestoreStamina` | Sets stamina to `EffectAmount`. |
| `Custom` | Runs the assigned Blueprint behavior on the server. |

For a bandage, set `Heal`, `EffectAmount` to 25 and `UseDurationSeconds` to 1. Turn on `Consumed On Successful Use` to remove it after it heals the player.

For `Custom`, create a Blueprint derived from **SCP Mod Item Use Behavior**, override **Apply**, and select that class in `UseBehavior`. **Apply** receives the player pawn and item instance GUID. Run the effect synchronously and return true on success. Returning false keeps a consumable in the inventory. The game handles the use duration, cancellation and consumption.

Firearms, card access permissions and SCP-914 recipes each require a separate gameplay integration.

## Add optional first-person animations

Create a hand Animation Blueprint compatible with the game's first-person arm rig. Assign `FirstPersonHandsAnimClass` and `FirstPersonItemMesh` together. Use `FirstPersonItemAnimClass` if the item also animates and `FirstPersonSocket` if it attaches to a specific hand socket or bone.

Use **SCP Mod Item Anim Instance** as the AnimBP parent to read `ItemState`. An existing parent can call **Get Mod Item Animation State** on the pawn from the AnimBP Event Graph. Drive use transitions with `bUsing` and synchronize the animation with `UseProgress` when needed. AnimBPs handle local presentation. The server applies the use effect separately. World pickups and third-person presentation use static meshes.

## Spawn, grant and test

From a server-side mod Blueprint, call **Spawn Mod Item Pickup** with the definition and a world transform, or call **Give Mod Inventory Item** with the player pawn and definition. The grant returns success and the slot index. A full inventory rejects the grant.

Run **Get Validation Errors** on each definition. Cook the entire mod folder and package it using the [Workshop guide](https://scpreenter.com/docs/workshop/). Test the cooked mod on a dedicated server with two clients. Try picking up, using, cancelling use, dropping and picking up with the other client. Check what each client sees and what happens when a player joins after a pickup has spawned.
