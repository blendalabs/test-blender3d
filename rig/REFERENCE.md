# Boy rig reference

Starter rig for the Blenda Labs prompt-to-animation test. File: `Boy_rig.blend`.
Armature object `Boy_Rig`, mesh object `Boy_Mesh`. The file also has a camera, three area lights and a floor, so
it renders as it is (Eevee, 1920x1080, 24 fps, frames 1-96). Pose preview: `pose_sheet.png`. Pose data: `poses.json`.

## Conventions

- Units: metres. Z up. The character faces **-Y**. The character's **left side is +X** (bones ending in `.L`).
- Height about 1.44 m. The rig was generated with Rigify and has 269 bones. You only animate the **22 control
  bones** below. Do not animate the others (IK, tweak, MCH, ORG, DEF, finger and widget bones).
- Rest pose is a **T-pose with the arms hanging 30 degrees below horizontal**. Every control has zero rotation and
  zero location in the rest pose.
- The 22 controls use **Euler XYZ** rotation mode. Rotations are **local** (relative to the bone's rest pose), in degrees in
  `poses.json` and radians inside Blender. Other bones use quaternions, which is another reason to leave them alone.
- Only `root` and `torso` should be moved (location). Every other control should only be rotated.
- All four limbs are set to **FK** (`IK_FK` = 1 on `upper_arm_parent.L/.R` and `thigh_parent.L/.R`). Leave those
  properties alone: in IK mode the `*_fk` controls do nothing.
- Rotation limits are enforced with Limit Rotation constraints (local space, named `Limit Rotation`). Values outside
  the limits are clamped.
- The hands are mittens: two finger bones and a thumb per hand, not part of the 22 controls. The right mitten is one
  segment shorter than the left. There are no face bones.

## Controls and limits

Limits are local Euler angles in degrees.

| Control | Group | Limits (deg) | Notes |
|---|---|---|---|
| `root` | Root | X 0..0, Y 0..0, Z -180..180 | Moves and turns the whole character. Turn about Z only. |
| `torso` | Torso | X -20..20, Y -15..15, Z -25..25 | Moves the pelvis and spine. Lower it (location Z) for sitting. |
| `hips` | Torso | X -30..30, Y -20..20, Z -45..45 | Pelvis rotation. |
| `chest` | Torso | X -30..30, Y -20..20, Z -40..40 | Upper-body rotation. |
| `neck` | Torso | X -40..40, Y -60..60, Z -30..30 |  |
| `head` | Torso | X -50..50, Y -70..70, Z -35..35 |  |
| `shoulder.L` | Arm | X -10..25, Y -15..15, Z -30..30 | Shoulder girdle. +X lifts it (shrug). |
| `upper_arm_fk.L` | Arm | none | Ball joint, no limits. |
| `forearm_fk.L` | Arm | X 0..150, Y -90..90, Z -5..5 | Elbow hinge. Bend about X only; flexion is positive X. |
| `hand_fk.L` | Arm | X -70..70, Y -45..45, Z -30..30 |  |
| `thigh_fk.L` | Leg | none | Ball joint, no limits. |
| `shin_fk.L` | Leg | X 0..150, Y -35..35, Z -5..5 | Knee hinge. Bend about X only; flexion is positive X. |
| `foot_fk.L` | Leg | X -45..45, Y -25..25, Z -25..25 |  |
| `toe_fk.L` | Leg | X -45..45, Y 0..0, Z 0..0 |  |
| `shoulder.R` | Arm | X -10..25, Y -15..15, Z -30..30 | Shoulder girdle. +X lifts it (shrug). |
| `upper_arm_fk.R` | Arm | none | Ball joint, no limits. |
| `forearm_fk.R` | Arm | X 0..150, Y -90..90, Z -5..5 | Elbow hinge. Bend about X only; flexion is positive X. |
| `hand_fk.R` | Arm | X -70..70, Y -45..45, Z -30..30 |  |
| `thigh_fk.R` | Leg | none | Ball joint, no limits. |
| `shin_fk.R` | Leg | X 0..150, Y -35..35, Z -5..5 | Knee hinge. Bend about X only; flexion is positive X. |
| `foot_fk.R` | Leg | X -45..45, Y -25..25, Z -25..25 |  |
| `toe_fk.R` | Leg | X -45..45, Y 0..0, Z 0..0 |  |

**Elbows and knees bend in positive X.** Flexion is `+X` on `forearm_fk.*` (the forearm swings forward) and on
`shin_fk.*` (the shin swings back). This is the opposite sign from many rigs. Swinging a thigh forward is negative X.

## What a positive rotation does

Each cell shows where the control moves when rotated **+30 degrees** about that local axis from the rest pose.
For X and Z on limbs and head it is the bone tip that moves. For Y it is the bone's side face that turns toward the
direction shown. For `root`, `torso`, `hips` and `chest` the bones lie horizontally, so the cell shows where a point on
the body moves instead: a point above the pelvis for X and Y, and a point in front of it for Z.
Negative angles move the opposite way. Left and right limbs are mirrored in Z: the left arm rises with +Z, the right
arm with -Z.

| Control | +X moves toward | +Y (twist) turns toward | +Z moves toward |
|---|---|---|---|
| `root` | forward | left | left |
| `torso` | forward | left | left |
| `hips` | forward | left | left |
| `chest` | forward | left | left |
| `neck` | forward | back | right |
| `head` | forward | back | right |
| `shoulder.L` | up | down | back |
| `upper_arm_fk.L` | forward | back | up |
| `forearm_fk.L` | forward | back | up |
| `hand_fk.L` | forward | back | up |
| `thigh_fk.L` | back | forward | right |
| `shin_fk.L` | back | forward | right |
| `foot_fk.L` | down & back | up | right |
| `toe_fk.L` | up | down | left |
| `shoulder.R` | up | down | forward |
| `upper_arm_fk.R` | forward | back | left & down |
| `forearm_fk.R` | forward | back | left & down |
| `hand_fk.R` | forward | back | down & left |
| `thigh_fk.R` | back | forward | right |
| `shin_fk.R` | back | forward | right |
| `foot_fk.R` | down & back | up | right |
| `toe_fk.R` | up | down | left |

## Poses

All poses are in `poses.json` and shown in `pose_sheet.png`. Only controls that differ from rest are listed.

| Pose | Description |
|---|---|
| `rest` | Default T-pose: arms 30 deg below horizontal, feet flat. All values zero. |
| `a_pose` | Relaxed stance: arms down by the sides. |
| `wave` | Right arm out, forearm straight up, as in a wave (static key pose). |
| `shrug` | Shoulders up, forearms out and forward, head tilted. |
| `surprised` | Both hands raised near the face, chest and head leaning back. |
| `thinking` | Right hand to the chin, head nodded slightly down (approximate). |
| `point` | Right arm extended forward, pointing. |
| `sit` | Seated: torso lowered 0.31 m, thighs forward, shins down. |
| `walk_contact` | Walk contact pose: left heel forward, right leg back, arms counter-swinging (approximate). |

Format of `poses.json`:

```json
{"poses": {"wave": {"description": "...", "bones": {"upper_arm_fk.R": {"rotation_euler_deg": [x, y, z]}}}}}
```

`root` and `torso` may also have `"location_m": [x, y, z]`. The top-level `controls` list names the 22 controls.

Apply a pose and keyframe it in Blender's Python console:

```python
import bpy, json, math
rig = bpy.data.objects["Boy_Rig"]
data = json.load(open("rig/poses.json"))
pose = data["poses"]["wave"]["bones"]
for name in data["controls"]:
    pb = rig.pose.bones[name]
    d = pose.get(name, {})
    pb.rotation_euler = [math.radians(a) for a in d.get("rotation_euler_deg", (0, 0, 0))]
    pb.keyframe_insert("rotation_euler", frame=24)
    if name in ("root", "torso"):
        pb.location = d.get("location_m", (0, 0, 0))
        pb.keyframe_insert("location", frame=24)
```

## Known limitations

- The body is a stylised mannequin: fine for testing keyframes, not production quality.
- Fingers are not exposed, and the mitten hands cannot grip.
- Some poses (`thinking`, `walk_contact`) are approximate and may need adjusting for contact or intersection.
- The rig carries two embedded Rigify UI scripts. Blender only runs them if Auto Run Python Scripts is on; the
  controls work without them.
