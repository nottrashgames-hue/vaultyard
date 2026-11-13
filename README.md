# Vaultyard

A WebXR physics playground: run, sprint, jump, climb with physics hands, and shoot an M1911 at the range.

Open the GitHub Pages URL in the Quest browser and press **Enter VR**.

## Controls (VR)
- Left stick: move · Left stick click: sprint · A/X: jump · Right stick: snap turn
- Grip: grab props, or grip any surface and pull to climb
- Pistol: grip near it to pick it up by the grip · Trigger: fire · B/Y: drop magazine
- Other hand on the slide, pull back and let go to chamber · Line a magazine up under the grip and slide it all the way in to reload
- Ammo pouch on your left hip: reach in with your free hand and grip to get a full magazine for the gun you're holding; let a magazine go inside the pouch to put it away

## Procedural damage
The floating sphere at the range is a `DamageVolume`: a voxel signed-distance field meshed with surface nets in 16³ chunks. Each shot carves an entry crater, a wound channel as far as the round's penetration lasts (existing tunnels cost nothing, so repeat shots dig deeper) and a flared exit wound when it gets through. For NPCs, give each rigid body part its own volume and parent `volume.object` to the bone.

See CREDITS.md for asset licenses.

## Blood
Wounds on damage volumes paint a blood trail down the surface and drip from its lowest point, slowing as they clot. Impacts spray drops; every drop that lands on static geometry leaves a splat that stays (instanced, up to 3000).

## Shotgun
Pump-action 12 gauge on the second bench, modelled from primitives. Grip it to pick it up by the stock, grip the forend with the other hand to aim two-handed and pump. Push shells into the port under the receiver (6 in the tube); the pouch gives shells while you hold it. Nine pellets per shot; pellets that land close together tear chunks of flesh out that fall as physics pieces.

## Gore (direction: GREYMEAT by Lev Filippov)
The flesh target has layers (skin, fat, muscle) over a bone core. Wounds are wet, entries get an abrasion collar, bruises spread and darken over a couple of minutes and yellow at the edges. Blood runs down the skin as surface parcels and only drips when it gathers on an overhang. Drops are 2D sprites; splats stay. Push a red RESET RANGE button (by the benches, or at spawn) to reset everything.
