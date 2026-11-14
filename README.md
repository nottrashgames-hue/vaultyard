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
Pump-action 12 gauge on the second bench: Quaternius's low-poly Shotgun_3 (CC0) with its wooden forend split out as the pump, in the same flat-shaded wood and metal as the M1911. Grip it to pick it up by the stock, grip the forend with the other hand to aim two-handed and pump. Push shells into the port under the receiver (6 in the tube); the pouch gives shells while you hold it. Nine pellets per shot; pellets that land close together tear chunks of flesh out that fall as physics pieces.

## Gore (direction: GREYMEAT by Lev Filippov)
The flesh target has layers (skin, fat, muscle) over a bone core. Wounds are wet, entries get an abrasion collar, bruises spread and darken over a couple of minutes and yellow at the edges. Blood runs down the skin as surface parcels and only drips when it gathers on an overhang. Drops are 2D sprites; splats stay. Push a red RESET RANGE button (by the benches, or at spawn) to reset everything.

## Ragdoll NPC (down the shotgun lane)
An active ragdoll: 13 rigid bodies on cone-twist and hinge joints. Each joint has a muscle (rotational motors solved with the joints and contacts) that pulls it toward a standing pose with a capped torque; balance is a spring-damper on the pelvis that reacts into the planted shoes, so it only works through the ground. When a shove would carry its centre of mass past its feet (capture point), it swings a leg out and plants that foot where it will stop the fall, stepping again if needed; the swing is a force on the shoe with the reaction on the hips. Push hard enough and it falls; if it's still fit to, it sits up, tucks its feet under and stands back up, with every lifting force reacting into the parts on the ground. Grab a limb and drag it. Hands press on it softly (a resting hand pushes about 35 N, a fast shove up to about 400 N) and your body only nudges it, so touching it doesn't knock it over; under a steady push it bends at the hips to lean into it.

Every part carries its own `DamageVolume` with skin or clothing, muscle, bone (skull, ribs, spine, long bones) and organs (brain, heart, lungs, liver, intestines). A round through the brain kills instantly; the heart gives it a few seconds; blood loss from its wounds weakens it and eventually kills it; leg hits make it buckle. Open the belly wide enough and a loop of intestine spills out, swings, drips and can be grabbed; yank it hard and it tears loose.

Torn shotgun chunks are meshed at a coarser 12 mm grid, and crater rebuilds are spread over the next few frames.

### Blood, shock and wounds
A tag over the NPC's head shows its blood volume. Bleeding from every wound drains it; bare skin turns waxy grey-white as it drops, it weakens, passes out at 60% and dies at 45%. While conscious it presses a hand on its worst reachable wound, which cuts that wound's bleeding to a quarter.

### Gore sounds
Bullet, bone, tearing and body-fall hits are recordings from Will Leamon's Fleshy Fight Sounds (OGA-BY 3.0); splats are cut from CC0 splatter recordings at load time; chunks and guts use CC0 squishes. See CREDITS.md.

### Dismemberment
After every hit the damaged part's flesh is checked for pieces that are no longer connected. The piece holding the joint toward the body stays; any other sizeable piece becomes its own rigid body with its own flesh (a shoe or hand goes with it), and joints on it move with it. A part shot down to a thin strip tears through. Losing an arm or a leg is survivable (limb wounds clot over time, and a lost leg keeps him down); a torso split or losing the head is fatal.

### Shotgun loading
Bring a shell up anywhere under the receiver or the front of the tube and it goes in, in any orientation.

### Blood pools
Drops that land on a floor collect into pools that grow with the blood reaching them and run together when they touch, so a long bleed is a handful of puddles instead of thousands of splats. Anything dragged through a pool (bodies, severed parts, torn chunks, guts) picks blood up and stains; while it is actually touching a surface (taken from the physics contacts) it lays streaks along the path it slides, thinning out over ~20 cm. Walk through one and you leave footprints. Blood dries darker after about a minute and fades away after three or four.

### Machete and mesh contacts
Hands and the machete meet the NPC through its flesh (the damage volumes' distance fields), not its box colliders, so a hand presses on the real, damaged surface. The machete samples 15 points along its edge each physics step and traces each point's path: an edge point moving edge-first through flesh carves a continuous cut and is resisted (bone far more), the tip pushing in is a stab, and the blade can't slide sideways in its own cut. Once the edge bites, the body holds the blade on the line it entered along (no physics jitter): it can only go deeper, against the flesh and bone in front of the edge, or back out the way it came, and it moves with the body part it's in. A swing cuts until it slows to a stop and sticks; pull back to free it, or hold your hand well past it along the line for a moment to drive it on. Cut a limb through and it comes off. It's held like a sword: handle through the fist, blade up out of the thumb side, edge forward.

Surface blood (bleed points, trickles, hanging drops) is checked against the flesh every frame: anything left in mid-air where flesh was shot or cut away snaps back onto the surface or goes.
