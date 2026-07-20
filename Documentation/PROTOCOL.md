# HiveBoard: Data Collection Protocol

This document specifies the data each collaborating laboratory collects to validate HiveBoard, the modular dexterity board. The activity is operator-driven trials, robot-side performance characterization across attachments. Operators may control the manipulation system through a teleoperated arm or through a wearable interface or exoskeleton that drives a gripper or hand directly.

The objective is to demonstrate that the board is functional, reproducible, and discriminative across attachments and platforms. The protocol does not rank platforms against each other and does not characterize control interfaces or operators.

**Project resources:**
- Repository (STL, CAD, simulation assets, this protocol, trial logging template): https://github.com/EESC-LabRoM/HiveBoard
- Demonstration video (board walk-through and example trials): https://youtu.be/kaYB_Oc64nA

---

## 1. Materials

- One printed HiveBoard base.
- One full set of attachments across all three categories (torque, precision, assembly), including the four-piece friction ring accessory set for the ball valve.
- A manipulation system, consisting of an end-effector (gripper or hand) plus a control interface. The control interface may be:
  - a teleoperated robotic arm with a standard interface (e.g., SpaceMouse, leader arm, VR controller), or
  - a wearable interface or exoskeleton driving the end-effector directly.

## 2. Printing Specifications

The canonical print profile lives in the project README. Summary, for convenience:

- **Material:** PLA, 1.75 mm filament.
- **Nozzle:** 0.4 mm.
- **Layer height:** 0.20 mm.
- **Walls:** 4 perimeters.
- **Top / bottom layers:** 5 / 5.
- **Print speed:** 50 mm/s.
- **Nozzle temperature:** 200 to 220 °C.
- **Bed temperature:** 50 to 60 °C.
- **Cooling:** 100%.
- **Adhesion:** skirt or brim.
- **Supports:** only where required.

**Infill, per part type:**

| Part type | Infill |
|---|---|
| Base structure | 20% |
| Mechanical parts | 25% |
| Torque components | 30% |
| Threads | 25% |
| Decorative covers | 15% |

**Print orientation:**

| Component | Orientation |
|---|---|
| Threads / screws | vertical |
| Nuts | flat |
| Valves | handle up |
| Drawer | flat on the largest face |
| Pegs | vertical |
| Shock absorber parts | sideways |

**Post-processing.** Remove stringing and check that mating surfaces seat fully before testing. Minor sanding of threaded surfaces may improve performance when printer calibration produces tight tolerances. If sanding is applied to any attachment, record it in the trial `notes` column.

## 3. Physical Setup

1. Fix the HiveBoard base to a rigid surface within the operator's working volume. The base may be mounted horizontally on a tabletop or vertically on a fixture. Record the orientation used.
2. Insert the attachments into their designated cells. Each attachment seats by press-fit; verify all attachments are fully seated before starting trials.
3. Position the manipulation system so that all attachments are reachable without base repositioning during a trial.
4. Photograph the setup and store the image as `setup.jpg`.

## 4. Trial Procedure

For each `(platform, attachment)` pair:

1. **Familiarization.** Run unrecorded practice trials until completion times stabilize. Record only after three consecutive practice trials complete within 20% of each other or within the timeout, whichever comes first.
2. **Recorded trials.** Run **5 recorded trials** per attachment. The end-effector starts each trial from the same neutral pose for a given platform.
3. **Trial start.** Begin timing when the operator initiates the first commanded motion toward the attachment.
4. **Trial end.** Stop timing when the success criterion is met, when a safety event occurs, or when the timeout is reached.
5. **Reset.** Return the attachment to its starting state between trials. Re-seat the attachment in its cell if displaced.

The ball valve is run as two separate sets of 5 trials: once without the friction ring (`valve_ball`) and once with the ring fitted (`valve_ball_ring`).

No scripted assistance, replanning, or per-trial parameter tuning is permitted during recorded trials. The operator may restart their motion within a trial (this counts as a re-grasp, not a new trial).

## 5. Per-Attachment Success Criteria and Timeouts

**Torque category**

| Attachment | Success criterion | Timeout |
|---|---|---|
| Ball valve | Handle rotated 90° from closed to open | 60 s |
| Ball valve + friction ring | Handle rotated 90° from closed to open with the ring fitted | 90 s |
| Gate valve, small | Stem rotated from fully closed to fully open | 90 s |
| Gate valve, large | Stem rotated from fully closed to fully open | 120 s |
| Circuit breaker | Toggle moved from one state to the other and held | 60 s |

**Precision category**

| Attachment | Success criterion | Timeout |
|---|---|---|
| Light bulb | Bulb fully threaded until seated | 120 s |
| Thread M8 | Bolt fully threaded along available length | 120 s |
| Thread M30 | Bolt fully threaded along available length | 120 s |
| Peg insertion | Free peg fully seated in the empty socket | 120 s |

**Composed assembly category (stage-wise scoring)**

These attachments are scored at sub-task granularity. Record the highest reached stage within the timeout.

| Attachment | Stages | Timeout |
|---|---|---|
| Button | (1) Open the cover (2) Press the button until actuation | 60 s |
| Lock and key | (1) Grasp key (2) Insert key vertically (3) Rotate to unlock | 180 s |
| Drawer | (1) Grasp handle (2) Pull open (3) Push closed | 120 s |
| Shock absorber | (1) Grasp pin (2) Align with hole (3) Insert pin fully | 180 s |

A trial is a full success only if all stages complete within the timeout. Partial completions are recorded by their highest reached stage in the `stage_reached` column.

## 6. Data to Log per Trial

Each trial fills one row of `trials.csv` (template provided alongside this protocol). See `HOW_TO_FILL_TRIALS.md` for column-by-column instructions.

For every trial whose outcome is `fail`, `timeout`, or `safety_stop`, record the primary failure cause in the `failure_cause` column using this closed vocabulary:

| Cause | Meaning |
|---|---|
| `grasp_geometry` | The end-effector cannot engage the part in a stable grasp (shape, size, or access mismatch). |
| `kinematic_limit` | The platform cannot produce the motion the task requires (e.g., clean rotation about an axis). |
| `perception` | The operator could not see or judge alignment well enough to complete the task. |
| `slip` | A grasp was acquired but lost during execution. |
| `control_precision` | The available control resolution was too coarse for the clearance or force the task requires. |
| `other` | None of the above; describe in `notes`. |

Pick the single dominant cause per trial. Leave the column blank on `success` rows. The closed vocabulary is what makes failure modes comparable across laboratories, so use `other` plus a note only when nothing else fits.

Alongside `trials.csv`, each lab provides:

- `setup.jpg`, photograph of the physical setup.
- `platform.md`, short description of the manipulation system: end-effector (gripper or hand), control interface (teleoperated arm or wearable), and any relevant control mode or calibration notes.

## 7. Submission

Each collaborating lab returns a single archive containing:

- `trials.csv`
- `setup.jpg`
- `platform.md`

### Reproducibility checklist

- [ ] All attachments printed with the specified PLA profile and per-part-type infill.
- [ ] Each attachment seats fully in the base before trials begin.
- [ ] Familiarization completed for every attachment before recording.
- [ ] 5 recorded trials per attachment, all logged in `trials.csv`. The ball valve appears twice, once without the friction ring and once with it.
- [ ] Timeouts enforced as specified.
- [ ] `failure_cause` filled from the closed vocabulary for every `fail`, `timeout`, and `safety_stop` row.
- [ ] `setup.jpg` and `platform.md` included.
- [ ] No operator names or identifiers in any field.
