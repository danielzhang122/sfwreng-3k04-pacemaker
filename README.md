# SFWRENG 3K04 Pacemaker

Pacemaker firmware modelled in Simulink and Stateflow for McMaster's SFWRENG / MECHTRON 3K04
(Software Development), Fall 2025. It was a team project. The model generates C code that runs on an
**NXP FRDM-K64F** board fitted with the course's pacemaker shield.

## Pacing modes

| Mode | Paces | Senses | Response to a sensed beat |
|---|---|---|---|
| AOO | Atrium | — | — |
| VOO | Ventricle | — | — |
| AAI | Atrium | Atrium | Inhibit |
| VVI | Ventricle | Ventricle | Inhibit |
| AOOR / VOOR / AAIR / VVIR | as above | as above | as above, with the pacing rate raised toward a maximum sensor rate as the board's FXOS8700 accelerometer detects activity (tuned by activity threshold, reaction time and recovery time) |

The model has serial receive and transmit blocks so a Device Controller-Monitor (DCM) front end could set the
pacing parameters (mode, lower rate, pulse amplitude and width, sensitivity, refractory periods) and read
data back. The DCM never got fully working, so it isn't included here; the serial side of the model is
untested end to end.

## Files

- **`Pacemaker.slx`**: the latest model (saved 2025-12-01). Top level: hardware input pins, serial
  `COM_IN` / `COM_OUT`, `Pacemaker Logic` (the Stateflow mode chart), `Pacemaker Outputs` and the LEDs.
- **`archive/`**: earlier versions, oldest first, kept to show how the model grew:

| File | Saved | What's in it |
|---|---|---|
| `v1_2025_10_20_D1_first_team_draft.slx` | 2025-10-20 | First team draft: AOO, VOO, AAI, VVI |
| `v2_2025_10_22_D1_chart_skeleton.slx` | 2025-10-22 | A bare Stateflow chart skeleton |
| `v3_2025_10_25_D1_final.slx` | 2025-10-25 | Deliverable 1: the four basic modes, parameters as constants |
| `v4_2025_11_23_D2_rate_adaptive_modes.slx` | 2025-11-23 | Adds the four rate-adaptive modes |
| `v5_2025_11_26_D2_rate_adaptive_revised.slx` | 2025-11-26 | Revised rate-adaptive version |
| `v6_2025_11_26_D2_serial_comms_added.slx` | 2025-11-26 | Adds serial receive and transmit for the DCM |
| `v7_2025_11_28_D2_work_in_progress.slx` | 2025-11-28 | Work in progress on serial data types (not fully working) |

## Running it

1. MATLAB **R2025a** with Simulink, Stateflow, Embedded Coder and the
   *Simulink Coder Support Package for NXP FRDM-K64F Board*.
2. Open `Pacemaker.slx`, plug in the board, and use **Build, Deploy & Start** on the Hardware tab.

Generated code and build output (`slprj/`, `*_ert_rtw/`, `.slxc`, `.elf`/`.hex`/`.bin`) are not in the repo; Simulink recreates them on build.
