# Battery

Decided 2026-11-08. The OpenMower guide describes one pack and `Universal-5S`. It does not describe two packs. The 430X has no model guide for this.

The original pack is 18 V / 5.2 Ah, 5S2P lithium-ion. Full charge is 21.0 V (5 × 4.2 V). One pack on this mower measured 20.14 V on 2026-10-26. Two packs of the same model go in parallel, plus to plus and minus to minus. Pack voltage stays 5S. Capacity is about 10.4 Ah. `ll.board` stays `Universal-5S`.

Each pack keeps its own BMS. The parallel connection is on the outputs. The data pins of the original connector are not mapped on this 430X and are not tied between the packs.

```
Pack A + —— 10 A fuse ——+
                        +—— mower positive
Pack B + —— 10 A fuse ——+

Pack A − ——+
           +—— mower negative
Pack B − ——+
```

The fuses are on the positive leads, before the leads join, as close to each positive pole as practical. Both fuses are the same automotive blade fuse, 32 V, for example ATO/ATC. The negatives join with no fuse. Before the positives join, the resting voltages should differ by no more than about 0.1 V. The wire from each pack to the junction is at least 1.5 mm².

The 10 A value is calculated from the saved VESC files, not taken from a guide. Each of the three ESCs has `l_in_current_max` at 2 A, plus about 1 A for the boards. 10 A is the next standard size up. Each fuse must carry the whole mower, because one BMS can open and leave the other pack supplying everything.

## Charger

Only the S5 jumper is closed. Closing 6S, 7S, or 8S sets the cell voltage to 25.2 V, 29.4 V, or 33.6 V. Firmware does not set the charge voltage. `Power_GetDefaultChargeVoltage()` returns `-1`, which leaves the hardware divider alone. Do not set `charge_voltage` in the parameters. That line would overwrite the S5 divider.

With firmware v1.4.3 running, `Universal-5S` writes the charge current itself. The default is 0.5 A and the firmware ceiling is 2 A. The current jumper is the fallback when the charger watchdog lets go. On 2026-11-10 the choice in `custom_params.yaml` was 2 A, above the computer's draw and under the pack's 4 A charge limit. `openmower configure ros` opens the wrong file on this mower. Edit `~/params/custom_params.yaml` and run `openmower restart`.

The two lines sit under `ll.services.power`. Do not add `charge_voltage`. Do not set `dangerously_override_hardware_charge_current_limit`. Without that override the firmware will not program more than 2 A.

```yaml
ll:
  services:
    power:
      charge_current: 2.0
      charge_critical_high_current: 2.5
```

`charge_critical_high_current` does not program the charger. It is the level at which Charge Current counts as too high, so it stays above the setpoint. The other battery keys are in [Settings on this mower](settings.md).

In the app, V charge is the adapter voltage. Charge Current is the current into the battery. Cell voltage is V battery and should stop at 21.0 V. The dock supply sold for a 430X is listed as 28 V / 4.2 A. That listing was not measured on this dock. The charger IC, not the supply's rating, sets the current into the cells.
