# How to fill `trials.csv`

The CSV is pre-populated with one row per trial, in the order trials should be run. Each block of 5 rows corresponds to one attachment.

**Do not change** `trial_id` or `attachment_id`. Fill the remaining columns as you run the trials.

For a video walk-through of the protocol see: https://youtu.be/kaYB_Oc64nA

## Columns to fill

| Column | What to enter |
|---|---|
| `lab_id` | Short identifier for your lab (e.g., `usp_crob`, `eth_rsl`, `iit_genoa`). Use the same value on every row. |
| `platform_id` | End-effector + control interface combination, lowercase, underscores. Examples: `franka_2f85` (Franka arm with Robotiq 2F-85, teleop), `ur5_robotiq_3f` (UR5 with Robotiq 3F), `anymal_arm_dynaarm`, `psyonic_ability_wearable` (Psyonic Ability Hand driven by a wearable interface). Use the same value on every row. |
| `date` | Trial date in `YYYY-MM-DD` format. |
| `outcome` | One of: `success`, `fail`, `timeout`, `safety_stop`. |
| `failure_cause` | **Required whenever outcome is not `success`.** One of: `grasp_geometry`, `kinematic_limit`, `perception`, `slip`, `force_limit`, `control_precision`, `other`. Pick the single dominant cause; see the quick reference below. Leave blank on `success` rows. |
| `completion_time_s` | **Decimal seconds** from trial start to success, as a plain number. Write `12.9`. Do not write milliseconds (`12923`), units (`12.9 s`), text (`timeout (120s+)`), or a date. **Leave blank** if outcome is not `success`. |
| `n_attempts` | Discrete attempts at the task within the trial, **counted from 1**. An attempt ends when the operator abandons the current approach and starts over. Corrective motions inside one continuous approach are not separate attempts. Fill this on unsuccessful trials too. |
| `n_regrasps` | Times the end-effector released the part and closed on it again, **counted from 0**. One grasp with no release is `0`, not `1`. A platform that cannot regrasp records `0`. Fill this on unsuccessful trials too. |
| `stage_reached` | **Only fill for assembly attachments** (button, lock, drawer, shock_absorber). The number of the **last stage completed**, from 0 to 3. A successful trial reaches its final stage. A trial that never completed stage 1 is `0`. Leave blank for all other attachments. |
| `strategy` | `prehensile` if the end-effector closed around the part and held it. `non_prehensile` if the part was driven by pushing or pressing, or by a finger or jaw entered into an opening without an enclosing grip. Fill on every row, successful or not. |
| `notes` | Free-form notes. ASCII only. No operator names. Record here any deviation from the success criterion, any sanding, and any part that broke or came loose. |

## Quick reference for failure causes

| Cause | Use when |
|---|---|
| `grasp_geometry` | The end-effector cannot engage the part in a stable grasp (shape, size, or access mismatch). |
| `kinematic_limit` | The platform cannot produce the motion the task requires (e.g., clean rotation about an axis). |
| `perception` | The operator could not see or judge alignment well enough to complete the task. |
| `slip` | A grasp was acquired but lost during execution. |
| `force_limit` | The platform cannot produce enough grasp or actuation force for the task, even though grasp and motion are otherwise correct. |
| `control_precision` | The control resolution was too coarse for the clearance or force the task requires. |
| `other` | Nothing above fits; explain in `notes`. |

## Quick reference for stage numbering

| Attachment | Stage 1 | Stage 2 | Stage 3 |
|---|---|---|---|
| button | Open the cover | Press the button | n/a |
| lock | Grasp key | Insert key vertically | Rotate to unlock |
| drawer | Grasp handle | Pull open | Push closed |
| shock_absorber | Grasp pin | Align with hole | Insert pin fully |

Enter the number, not the stage name. A shock-absorber trial in which the pin was grasped but never aligned is `1`. A trial in which the gripper never held the pin is `0`.

## Quick reference for timeouts

The ball valve is run in two configurations: without the friction ring (`valve_ball`) and with it fitted (`valve_ball_ring`). Each configuration is a separate set of 5 trials, treated as a distinct attachment.

| Attachment | Timeout |
|---|---|
| valve_ball | 60 s |
| valve_ball_ring | 90 s |
| valve_gate_small | 90 s |
| valve_gate_large | 120 s |
| circuit_breaker | 60 s |
| light_bulb | 120 s |
| thread_m8 | 120 s |
| thread_m30 | 120 s |
| peg_insertion | 120 s |
| button | 60 s |
| drawer | 120 s |
| lock | 180 s |
| shock_absorber | 180 s |

## Examples (for reference only, do not paste these into your CSV)

A torque attachment, mixed outcomes:

```
1,usp_crob,franka_2f85,valve_gate_small,2026-04-26,success,,34.2,1,0,,prehensile,
2,usp_crob,franka_2f85,valve_gate_small,2026-04-26,fail,slip,,3,2,,prehensile,handle slipped
3,usp_crob,franka_2f85,valve_gate_small,2026-04-26,timeout,control_precision,,4,3,,prehensile,
```

An assembly attachment with stage tracking:

```
56,usp_crob,franka_2f85,lock,2026-04-26,success,,142.3,2,1,3,prehensile,
57,usp_crob,franka_2f85,lock,2026-04-26,fail,slip,,5,4,2,prehensile,key dropped twice
```

## Common mistakes

Every entry below has appeared in a returned log and had to be reconciled by hand.

| Mistake | What to do instead |
|---|---|
| Times in milliseconds, or mixed milliseconds and seconds in the same column | One unit for the whole column, decimal seconds |
| `timeout (120s+)` in the time column | Leave the time blank and set the outcome to `timeout` |
| A date pasted into the time column by autofill | Format the column as plain number before filling |
| An outcome written into the `date` column | Keep every value in its own column |
| `n_regrasps` never below 1 | A single grasp with no release is `0` |
| `-` in `n_attempts` and `n_regrasps` on failed rows | Enter the counts; a failed trial still had attempts |
| Six rows for one attachment, or a repeated `trial_id` | 5 rows per attachment, 65 rows total, `trial_id` unchanged |
| `stage_reached` left blank on a failed assembly trial | Enter the last stage completed, `0` if none |
| A rerun after a part broke, logged as though nothing happened | Log the trial as it stood and describe the damage in `notes` |

## Before sending back

- All 65 data rows have `lab_id`, `platform_id`, `date`, and `outcome` filled.
- `failure_cause` is filled from the closed vocabulary for every row whose outcome is not `success`, and blank on `success` rows.
- `completion_time_s` is filled in decimal seconds for every `success` row and blank otherwise.
- `n_attempts`, `n_regrasps`, and `strategy` are filled on every row, failed rows included.
- `stage_reached` is filled only for `button`, `lock`, `drawer`, and `shock_absorber` rows.
- Exactly 65 rows, with `trial_id` and `attachment_id` unchanged from the template.
- The CSV is accompanied by `setup.jpg` and `platform.md`, and `platform.md` states the board mounting orientation.
