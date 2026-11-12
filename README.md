# Vaultyard

A WebXR physics playground: run, sprint, jump, climb with physics hands, and shoot an M1911 at the range.

Open the GitHub Pages URL in the Quest browser and press **Enter VR**.

## Controls (VR)
- Left stick: move · Left stick click: sprint · A/X: jump · Right stick: snap turn
- Grip: grab props, or grip any surface and pull to climb
- Pistol: grip near it to pick it up by the grip · Trigger: fire · B/Y: drop magazine
- Other hand on the slide, pull back and let go to chamber · Push a held magazine into the grip to reload
- Ammo pouch on your left hip: reach in with your free hand and grip to get a full magazine for the gun you're holding; let a magazine go inside the pouch to put it away

## Procedural damage
The floating sphere at the range is a `DamageVolume`: a voxel signed-distance field meshed with surface nets in 16³ chunks. Each shot carves an entry crater, a wound channel as far as the round's penetration lasts (existing tunnels cost nothing, so repeat shots dig deeper) and a flared exit wound when it gets through. For NPCs, give each rigid body part its own volume and parent `volume.object` to the bone.

See CREDITS.md for asset licenses.
