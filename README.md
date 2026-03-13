# RammsAssets

Asset-only Unreal Engine plugin containing 3D models, skeletal meshes, materials,
physics assets, and data assets for the RAMMS robotics UI platform.

## Overview

RammsAssets provides all visual and physical representations of the robots and
sensors used in the RAMMS system. Assets are primarily imported from CAD sources
via Datasmith and organized by hardware system.

This plugin has **no C++ modules** — it is purely a content plugin
(`CanContainContent: true`).

## Content Structure

```
Content/
├── Robots/
│   ├── KinovaGen3/          Kinova Gen3 6-DOF robotic arm
│   │   ├── Arm/             Arm skeletal mesh, skeleton, physics, control rig
│   │   │   ├── SkeletalMeshes/   gen3_6dof + skeleton + physics asset + ctrl rig
│   │   │   └── Materials/        24 color/imported materials
│   │   └── Gripper/         2-finger adaptive gripper
│   │       ├── SkeletalMeshes/   kinova_gripper + skeleton + physics asset
│   │       └── Materials/        13 color materials
│   └── RAMMP/               RAMMP mobile manipulator platform
│       ├── Base/            Mebot chassis, drive wheels, casters, elevator
│       │   ├── SkeletalMeshes/   mebot + skeleton + physics asset
│       │   └── Materials/        19 mechanical part materials
│       └── Seat/            Operator seating module
│           └── Materials/        37 materials + SM_seat_2 static mesh
└── Sensors/
    ├── Gemini_336L/         ORBBEC Gemini 336L RGB-D camera
    │   └── Materials/            SM_Orbbec_Gemini_336L + color/depth data assets
    └── LUCI/                LUCI camera system
        └── Materials/            SM_LUCI_FL_F, SM_LUCI_FR_F + intrinsics data asset
```

## Robot Assets

### Kinova Gen3 6-DOF Arm

Skeletal mesh with full bone hierarchy for the 6-DOF arm. Includes a physics
asset with per-joint constraints (twist and swing limits) used by
`URammsSkeletalPoseComponent` to auto-detect joint types, axes, and limits.

| Asset | Description |
|-------|-------------|
| `gen3_6dof` | Rigged skeletal mesh (6 revolute joints) |
| `gen3_6dof_Skeleton` | Bone hierarchy |
| `gen3_6dof_PhysicsAsset` | Joint constraints and collision bodies |
| `gen3_6dof_CtrlRig` | Control rig for animation |
| `Kinova_Static_Poses` | Predefined arm pose data asset |

### Kinova 2-Finger Gripper

Separate skeletal mesh for the adaptive gripper, with its own physics asset
defining finger joint constraints.

| Asset | Description |
|-------|-------------|
| `kinova_gripper` | Rigged skeletal mesh (finger links) |
| `kinova_gripper_PhysicsAsset` | Finger joint constraints |

### RAMMP Mobile Base (Mebot)

The Mebot chassis includes drive wheels, caster assemblies with suspension
linkages, and an elevator mechanism — all as a single skeletal mesh with
physics constraints.

| Asset | Description |
|-------|-------------|
| `mebot` | Rigged skeletal mesh (wheels, casters, elevator) |
| `mebot_PhysicsAsset` | Drive, caster, and elevator constraints |

### Operator Seat

Static mesh for the operator seating module with associated materials.

## Sensor Assets

### ORBBEC Gemini 336L

Static mesh model of the RGB-D camera with data assets defining color and depth
channel parameters.

| Asset | Description |
|-------|-------------|
| `SM_Orbbec_Gemini_336L` | Camera housing static mesh |
| `ORBBEC_Gemini_336L_Color` | Color channel data asset |
| `ORBBEC_Gemini_336L_Depth` | Depth channel data asset |

### LUCI Cameras

Static mesh models for the LUCI front-left and front-right cameras with
intrinsic calibration data.

| Asset | Description |
|-------|-------------|
| `SM_LUCI_FL_F` / `SM_LUCI_FR_F` | Camera static meshes |
| `LUCI_Camera_Intrinsics` | Camera calibration parameters |

## Materials

Materials are organized alongside their parent meshes. Most are auto-generated
color materials (named `color_<RGBA hex>`) or Datasmith CAD import materials
(`M_DatasmithCAD_*`, `XID_Opaque_*`). A few are hand-authored material
instances (`MI_SeatBack`, `MI_Tube`, `MAT_LUCI`).

## Usage

Add `RammsAssets` to your project's `.uproject` plugin dependencies:

```json
{
    "Name": "RammsAssets",
    "Enabled": true
}
```

Assets can then be referenced from Blueprints or C++ using their content paths,
e.g. `/RammsAssets/Robots/KinovaGen3/Arm/SkeletalMeshes/gen3_6dof`.

## License

MIT — see [LICENSE](LICENSE) for details.
