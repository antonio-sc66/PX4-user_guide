# Mission Mode (VTOL)

[<img src="../../assets/site/position_fixed.svg" title="Global position fix required (e.g. GPS)" width="30px" />](../getting_started/flight_modes.md#key_position_fixed)

_Mission mode_ causes the vehicle to execute a predefined autonomous [mission](../flying/missions.md) (flight plan) that has been uploaded to the flight controller.
The mission is typically created and uploaded with a Ground Control Station (GCS) application like [QGroundControl](https://docs.qgroundcontrol.com/master/en/) (QGC).

VTOL vehicles follow the behavior and parameters of fixed-wing when in FW mode, and of multicopter when in MC mode.
For more information see the specific docs for each mode:

- [Mission Mode (MC)](../flight_modes_mc/mission.md)
- [Mission Mode (FW)](../flight_modes_fw/mission.md)

The following sections outline mission mode behaviour that is VTOL specificL.

## Mission Commands

The following VTOL-specific commands are as defined in the MAVLink specification.

- [MAV\_CMD\_NAV\_VTOL\_TAKEOFF](https://mavlink.io/en/messages/common.html#MAV_CMD_NAV_VTOL_TAKEOFF)
  - `MAV_CMD_NAV_VTOL_TAKEOFF.param2` (transition heading) is ignored.
    Instead the heading to the next waypoint is used for the transition heading. <!-- at LEAST until PX4 v1.13: https://github.com/PX4/PX4-Autopilot/issues/12660 -->
- [MAV\_CMD\_NAV\_VTOL\_LAND](https://mavlink.io/en/messages/common.html#MAV_CMD_NAV_VTOL_LAND)
- [MAV\_CMD\_DO\_VTOL\_TRANSITION](https://mavlink.io/en/messages/common.html#MAV_CMD_DO_VTOL_TRANSITION)

Otherwise PX4 generally "accepts" the mission commands for fixed wing or multicopter when in the respective modes.

In fixed-wing mode there are the following exceptions:

- [MAV\_CMD\_NAV\_LAND](https://mavlink.io/en/messages/common.html#MAV_CMD_NAV_LAND) is transformed into [MAV\_CMD\_NAV\_VTOL\_LAND](https://mavlink.io/en/messages/common.html#MAV_CMD_NAV_VTOL_LAND) unless [NAV\_FORCE\_VT](../advanced_config/parameter_reference.md#NAV_FORCE_VT) is set to `0` (disabled).
- [MAV\_CMD\_NAV\_TAKEOFF](https://mavlink.io/en/messages/common.html#MAV_CMD_NAV_TAKEOFF) is not supported.

## Mission Takeoff

Plan a VTOL mission takeoff by adding a `VTOL Takeoff` mission item to the map.

During mission execution the vehicle will ascend vertically to the minimum takeoff altitude defined in the [MIS\_TAKEOFF\_ALT](../advanced_config/parameter_reference.md#MIS_TAKEOFF_ALT) parameter, then transition to fixed-wing mode with the heading defined in the mission item.
After transitioning the vehicle heads towards the 3D position defined in the mission item.

A VTOL mission requires a `VTOL Takeoff` mission item (`MAV_CMD_NAV_VTOL_TAKEOFF`) to takeoff (or a `MAV_CMD_NAV_TAKEOFF` when the vehicle is in MC mode); if however the vehicle is already flying when the mission is started the takeoff item will be treated as a normal waypoint.

## See Also

- [Mission Mode (MC)](../flight_modes_mc/mission.md)
- [Mission Mode (FW)](../flight_modes_fw/mission.md)
- [Missions](../flying/missions.md)
  - [Package Delivery Mission](../flying/package_delivery_mission.md)