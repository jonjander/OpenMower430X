# Logic board and inputs

Checked against the `hw-module-logic` schematic and PCB on 2026-11-04, and against firmware v1.4.3. The three ESC sockets sit in a row. Which outer socket is left depends on which way the board faces.

| Socket | UART | Role | TCP |
|---|---|---|---|
| J21 | UART4 | right wheel | 65104 |
| J20 | UART2 | cutter | 65103 |
| J11 | UART1 | left wheel | 65102 |

The ports open only after ROS has sent `Universal-5S`. Until then `openmower expose-xesc` on the Pi gets `Connection refused` from `172.16.78.150`.

J5 pin 1 is the square pad in the corner of the 16-pin header nearest the four Hall sockets.

| J5 | Net | Use |
|---|---|---|
| 1 and 2 | 5 V | sensor supply |
| 3 and 4 | 3.3 V | pull-ups only |
| 5 and 6 | GND | ground |
| 7 | `GPIO0` | left lift |
| 9 | `GPIO7` | right lift |
| 11 | `GPIO2` | collision |
| 13 | `GPIO3` | spare, until a second collision sensor is identified |
| 15 | `GPIO4` | stop button |
| 8, 10, 12, 14, 16 | I2C and SPI | not GPIO |

`GPIO0`, `GPIO7`, `GPIO2`, `GPIO3`, and `GPIO4` go straight into the STM32. The driver sets them as inputs with no internal pull-up. Each signal gets 10 kΩ to 3.3 V on the adapter. A sensor must not drive these pins to 5 V. An open-drain output only pulls to ground, so the pin rests at 3.3 V and the STM32 never sees the sensor's 5 V supply.

Lift and collision are active-low. One active lift input is `lift`. Two at once is `lift_multiple`, and that one is latched. Collision follows the same pattern, which is why the spare input exists.

The adapter drawing is the working file [perfboard/boards/sensor-adapter.perf.json](../perfboard/boards/sensor-adapter.perf.json). Hole coordinates there are the current layout, not a second copy of these tables.

On this mower the logic board stands upright, USB and Ethernet up, components toward the front. The photographs are on the [layout](layout.md) page. `axis_config` was set to `-Z+Y+X` on 2026-11-10. After restart, gravity was on Z at 9.79 m/s². The string is three pairs: forward, left, up.
