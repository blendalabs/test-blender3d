# Humanoid rig reference

Starter rig for the Blenda Labs prompt-to-animation test. File: `humanoid_rig.blend`.
Armature object `Humanoid_Rig`, mesh object `Humanoid_Mesh`. Pose preview: `pose_sheet.png`. Pose data: `poses.json`.

## Conventions

- Units: metres. Z up. The character faces **-Y**. The character's **left side is +X** (bones ending in `.L`).
- Height about 1.73 m. 22 control bones, all deforming except `root`.
- Rest pose is an **A-pose** (arms 45 degrees down, feet flat). Every bone has zero rotation in the rest pose.
- All pose bones use **Euler XYZ** rotation mode. Rotations are **local** (relative to the bone's rest pose), in degrees in `poses.json` and radians inside Blender.
- Only `root` and `hips` should be moved (location). Every other bone should only be rotated.
- Rotation limits are enforced with Limit Rotation constraints (local space). Values outside the limits are clamped.

## Bones, parents and limits

Limits are local Euler angles in degrees.

| Bone | Parent | Limits (deg) | Notes |
|---|---|---|---|
| `root` | - | none | Moves the whole character. Not deforming. |
| `hips` | `root` | X -30..30, Y -45..45, Z -30..30 | Pelvis. Lower it (location Z) for sitting. |
| `spine` | `hips` | X -40..40, Y -40..40, Z -25..25 |  |
| `chest` | `spine` | X -30..30, Y -40..40, Z -20..20 |  |
| `neck` | `chest` | X -40..40, Y -60..60, Z -30..30 |  |
| `head` | `neck` | X -50..50, Y -70..70, Z -35..35 |  |
| `shoulder.L` | `chest` | X -25..25, Y -15..15, Z -35..35 |  |
| `upper_arm.L` | `shoulder.L` | none | Ball joint, no limits. |
| `forearm.L` | `upper_arm.L` | X -150..0, Y -90..90, Z -5..5 | Elbow hinge. Bend about X only; flexion is negative X. |
| `hand.L` | `forearm.L` | X -70..70, Y -45..45, Z -30..30 |  |
| `shoulder.R` | `chest` | X -25..25, Y -15..15, Z -35..35 |  |
| `upper_arm.R` | `shoulder.R` | none | Ball joint, no limits. |
| `forearm.R` | `upper_arm.R` | X -150..0, Y -90..90, Z -5..5 | Elbow hinge. Bend about X only; flexion is negative X. |
| `hand.R` | `forearm.R` | X -70..70, Y -45..45, Z -30..30 |  |
| `thigh.L` | `hips` | none | Ball joint, no limits. |
| `shin.L` | `thigh.L` | X -150..0, Y -35..35, Z -5..5 | Knee hinge. Bend about X only; flexion is negative X. |
| `foot.L` | `shin.L` | X -45..45, Y -25..25, Z -25..25 |  |
| `toe.L` | `foot.L` | X -45..45, Y 0..0, Z 0..0 |  |
| `thigh.R` | `hips` | none | Ball joint, no limits. |
| `shin.R` | `thigh.R` | X -150..0, Y -35..35, Z -5..5 | Knee hinge. Bend about X only; flexion is negative X. |
| `foot.R` | `shin.R` | X -45..45, Y -25..25, Z -25..25 |  |
| `toe.R` | `foot.R` | X -45..45, Y 0..0, Z 0..0 |  |

## What a positive rotation does

Each cell shows where the bone moves when rotated **+30 degrees** about that local axis from the rest pose.
For X and Z it is the bone tip that moves. For Y (twist along the bone) it is the bone's side face that turns toward the direction shown.
Negative angles move the opposite way.

| Bone | +X moves toward | +Y (twist) turns toward | +Z moves toward |
|---|---|---|---|
| `root` | up | down | left |
| `hips` | forward | back | right |
| `spine` | forward | back | right |
| `chest` | forward | back | right |
| `neck` | forward | back | right |
| `head` | forward | back | right |
| `shoulder.L` | forward | back | up |
| `upper_arm.L` | back | forward | right & down |
| `forearm.L` | back | forward | right & down |
| `hand.L` | back | forward | right & down |
| `shoulder.R` | forward | back | down |
| `upper_arm.R` | back | forward | up & right |
| `forearm.R` | back | forward | up & right |
| `hand.R` | back | forward | up & right |
| `thigh.L` | forward | back | left |
| `shin.L` | forward | back | left |
| `foot.L` | down & back | up | right |
| `toe.L` | down | up | right |
| `thigh.R` | forward | back | left |
| `shin.R` | forward | back | left |
| `foot.R` | down & back | up | right |
| `toe.R` | down | up | right |

Joints that bend in only one direction are the elbows and knees: use the flexion sign given in the Notes column.

## Poses

All poses are in `poses.json` and shown in `pose_sheet.png`. Only bones that differ from rest are listed.

| Pose | Description |
|---|---|
| `rest` | Default A-pose: arms 45 deg down, feet flat. All values zero. |
| `t_pose` | Arms straight out to the sides. |
| `wave` | Right arm raised, forearm up, as in a wave (static key pose). |
| `shrug` | Shoulders up, forearms out and forward, head tilted. |
| `surprised` | Both hands raised near the face, chest and head leaning back. |
| `thinking` | Right hand to the chin, head nodded slightly down (approximate). |
| `point` | Right arm extended forward, pointing. |
| `sit` | Seated: hips lowered 0.43 m, thighs forward, shins down. |
| `walk_contact` | Walk contact pose: left heel forward, right leg back, arms counter-swinging (approximate). |

Format of `poses.json`:

```json
{"poses": {"wave": {"description": "...", "bones": {"upper_arm.R": {"rotation_euler_deg": [x, y, z]}}}}}
```

`root` and `hips` may also have `"location_m": [x, y, z]`.

Apply a pose in Blender's Python console:

```python
import bpy, json, math
rig = bpy.data.objects["Humanoid_Rig"]
pose = json.load(open("poses.json"))["poses"]["wave"]["bones"]
for pb in rig.pose.bones:
    d = pose.get(pb.name, {})
    pb.rotation_euler = [math.radians(a) for a in d.get("rotation_euler_deg", (0, 0, 0))]
    pb.location = d.get("location_m", (0, 0, 0))
```

## Known limitations

- The body is a smooth mannequin with automatic weights: fine for testing keyframes, not production quality.
- No fingers, face or twist bones. The `nose` bump on the head shows which way the character faces.
- Some poses (`thinking`, `walk_contact`) are approximate and may need adjusting for contact or intersection.
