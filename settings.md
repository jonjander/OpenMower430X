# Settings on this mower

As the files and the mower stood on 2026-11-10. OpenMowerOS v2.1.1, firmware v1.4.3. `MOWER=CUSTOM`, so ROS loads `~/params/custom_params.yaml` only. `mower_params.yaml`, the file `openmower configure ros` opens, is not loaded. `FIRMWARE` in the env file is absent. If `custom_params.yaml` is missing, `open_mower_ros` exits before it sends `/ll/board`.

Dock coordinates live only in `custom_params.yaml` on the mower. They are not in this repository.

| Key | Value | Why it is settled |
|---|---|---|
| `ll.board` | `Universal-5S` | 5S pack. 7S or 8S would charge too high |
| `battery_full_voltage` | `21.0` | 5 × 4.2 V |
| `battery_empty_voltage` | `16.5` | 5 × 3.3 V |
| `battery_critical_voltage` | `15.0` | 5 × 3.0 V |
| `battery_critical_high_voltage` | `22.0` | a full 21.0 V pack is not an overvoltage |
| `charge_current` | `2.0` | firmware ceiling, above the computer's draw, under the pack's 4 A limit. The block is on the [battery](battery.md#charger) page |
| `charge_critical_high_current` | `2.5` | above `charge_current`. It does not program the charger |
| `enable_mower` | `false` | the cutter is not commissioned |
| `antenna_offset_x` | `0.3` | described as 30 cm forward of the axle center |
| `antenna_offset_y` | `0.0` | described as centered |
| `axis_config` | `-Z+Y+X` | measured, see [Logic board and inputs](logic-board.md) |

`wheel_distance_m` `0.325` and `ticks_per_m` `1600` are still the template. They have not been measured on this 430X. In firmware v1.4.3, `ticks_per_m` does not scale the duty command.

`board: "Universal-5S"` goes under `ll:` in `custom_params.yaml`. The Universal-guide template does not have that line yet.

Firmware v1.4.3 sends wheel commands as duty cycle, not meters per second. The drive wheels are clamped to ±1.0. The app joystick does not read the speed keys in `custom_params.yaml`. Its steps are 0.3, 0.65, and 1.0.

## xESC

`configs/xESC_App.xml` is the app config: `app_to_use` 3 (UART), 115200 baud, `permanent_uart_enabled` 1. The PPM ramp in that file is unused. Write app config once per ESC after the motor wizard. A later change of current or duty is Write motor configuration only.

The drive files that count are `configs/vesc_mcconf_drive_left.xml` and `configs/vesc_mcconf_drive_right.xml`, saved after the left Hall cable was connected on 2026-11-07. `configs/vesc_mcconf_drive.xml` is an older copy. Do not copy one ESC's XML onto the other. The Hall table and the current offset belong to that ESC.

Both drive files, the same:

| Parameter | Value |
|---|---|
| Motor Current Max / Brake | 2 A / −2 A |
| Battery Current Max / Regen | 2 A / −2 A |
| Absolute Maximum Current | 4.5 A |
| Maximum Duty Cycle | 0.30 |
| Minimum Duty Cycle | 0.005 |
| Sensor mode | Hall |

`l_slow_abs_current` stays 0. VESC developers have burned hardware by turning it on to hide a current fault.

`configs/vesc_mcconf_cutter.xml` is the first FOC run, not a finished configuration. The wizard values and the detection result are on the [motors](wheel-motor.md#foc-wizard-first-run) page. On this mower that detection behaved better with a load on the motors. `enable_mower` stays `false`. The cutter direction is not set.

Reach an ESC with `openmower stop`, then `openmower expose-xesc` and `left`, `right`, or `mower`. VESC Tool connects over TCP to the mower, port 65102 for the left wheel. Ctrl+C and `openmower start` afterwards. ROS and VESC Tool must not share the UART.
