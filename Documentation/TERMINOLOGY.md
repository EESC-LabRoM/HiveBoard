# Terminology

Use the task names from Table 2 of the HiveBoard paper in instructions, figures, result tables, and interfaces. The table below maps those names to the identifiers used in trial records.

## Task names and identifiers

| Task name | Trial identifier (`attachment_id`) | Category |
|---|---|---|
| Ball valve | `valve_ball` | Torque |
| Ball valve + ring | `valve_ball_ring` | Torque |
| Gate valve (small) | `valve_gate_small` | Torque |
| Gate valve (large) | `valve_gate_large` | Torque |
| Circuit breaker | `circuit_breaker` | Torque |
| Light bulb | `light_bulb` | Precision |
| Thread (M8) | `thread_m8` | Precision |
| Thread (M30) | `thread_m30` | Precision |
| Peg insertion | `peg_insertion` | Precision |
| Button | `button` | Composed assembly |
| Lock and key | `lock` | Composed assembly |
| Drawer | `drawer` | Composed assembly |
| Shock absorber | `shock_absorber` | Composed assembly |

HiveBoard has 12 attachment designs evaluated in 13 conditions. The ball valve is evaluated without and with a friction ring, giving two conditions for the same attachment design. Ball valve + ring is the condition with the ring fitted. Record five trials for each of the 13 conditions, including separate blocks for Ball valve and Ball valve + ring.

The category headings are **Torque-based tasks**, **Precision-based tasks**, and **Composed assembly tasks**. In tables, use **Torque**, **Precision**, and **Composed assembly**. The abbreviation “Assembly” in existing templates and the paper's Table 2 refers to Composed assembly.

## Hardware, tasks, and evaluation

| Term | Meaning |
|---|---|
| Honeycomb base | The structure into which the attachments are mounted. |
| Attachment | A physical mechanism mounted on the base, such as the drawer or a gate valve. |
| Task | The operation performed on an attachment, with a specified starting state and success criterion. |
| Condition | A scored task configuration. The two ball-valve configurations are separate conditions. |
| Trial | One recorded execution of a task, ending at success, failure, timeout, or a safety stop. |
| Platform | The end-effector and the system that positions and commands it. |
| End-effector | The gripper, hand, or other terminal device that interacts with the attachment. |
| Control interface | The interface used to command the platform. Record the control mode or policy separately where relevant. |
| Asset | A digital file or model, such as CAD geometry, a mesh, URDF, or USD. Use “attachment” for the physical mechanism. |
| Attempt | A discrete approach to the task within a trial; count from 1. Abandoning an approach and starting another adds one attempt. |
| Regrasp | A release followed by grasping the part again; count from 0. An initial grasp is not a regrasp. |
| Last completed stage | The highest completed stage of a composed assembly task, recorded as `stage_reached`; 0 means no stage was completed. |
| Dominant failure cause | The single cause recorded in `failure_cause` for an unsuccessful trial. |

## Existing names in files

Older labels and file paths may use the names below. They refer to the same attachments; descriptive details such as the button cover or the threaded peg remain part of the task instructions.

| Existing label or filename term | Task name |
|---|---|
| Ball valve with friction ring | Ball valve + ring |
| Small gate valve; Valve Gate (Small) | Gate valve (small) |
| Large gate valve; Valve Gate (Large) | Gate valve (large) |
| Light bulb socket; light bulb and socket; `Lamp/` | Light bulb |
| M8 threaded fastener; Thread M8 | Thread (M8) |
| M30 threaded fastener; Thread M30 | Thread (M30) |
| Threaded peg insertion; Peg Insertion Plate | Peg insertion |
| Covered button; Hidden Push Button | Button |
| `Key/` | Lock and key |
| Sliding Drawer; `box` in image/video filenames | Drawer |
| `spring` in image/video filenames | Shock absorber |

Keep existing identifiers, paths, and data fields unchanged. For example, `lock` remains the trial identifier for Lock and key, `light_bulb` remains the identifier for Light bulb, and `control_method` remains the runner's stored field for the control interface. The CSV outcome `fail` is displayed as “Failure” in the runner.

Thread (M14) is available in the 3D viewer but is not one of the 13 scored conditions.

See [the protocol](PROTOCOL.md) for task criteria and [the logging instructions](HOW_TO_FILL_TRIALS.md) for counting conventions.
