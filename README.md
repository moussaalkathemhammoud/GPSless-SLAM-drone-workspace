# GPS-less SLAM drone workspace

Everything needed to run the GPS-less SLAM drone stack in AirSim from WSL.

| Path | What it is |
|---|---|
| `slam_web/` | FastAPI backend, web frontend, mission planner and the AirSim Python flight controller (`slam_web/slam_web/app/drone_motion.py`) |
| `slam_web/orb_slam3_custom/` | Local ORB-SLAM3 changes (patch + the live AirSim stereo bridge) |
| `airsim_config/settings.json` | Copy of the AirSim settings used on Windows (`%USERPROFILE%\Documents\AirSim\settings.json`) |
| `ORB_SLAM3/` | Submodule: UZ-SLAMLab/ORB_SLAM3 (current SLAM backend) |
| `LIO-SAM/` | Submodule: TixiaoShan/LIO-SAM (planned SLAM backend) |
| `PX4-Autopilot/` | Submodule: PX4 firmware for SITL |
| `AirSim/` | Submodule: microsoft/AirSim (Python client, ROS 2 wrapper, PX4 scripts) |

## Setup

```bash
git clone --recurse-submodules <this repo> ~/drone_project
for d in slam_web AirSim PX4-Autopilot LIO-SAM ORB_SLAM3; do ln -s ~/drone_project/$d ~/$d; done
```

The code expects the folders at `~/<name>` (for example `~/ORB_SLAM3`), hence the symlinks.

Apply the local ORB-SLAM3 changes:

```bash
git -C ORB_SLAM3 apply ../slam_web/orb_slam3_custom/patches/orb_slam3_local_changes.patch
rsync -a slam_web/orb_slam3_custom/files/ ORB_SLAM3/
```

Copy `airsim_config/settings.json` to `%USERPROFILE%\Documents\AirSim\settings.json` on Windows.

The recorded datasets (`AirsimSeq/`, `Tdataset/`) are not part of the repository.
