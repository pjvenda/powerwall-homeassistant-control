# powerwall-homeassistant-control

This is a collection of scripts and automations that permit controlling your Tesla Powerwall battery system with Home Assistant. These permit, control operation modes, and automate energy management based on your preferences and energy usage patterns. Typical use cases for this set of scripts are to maximise financial efficiency or to maximise the positive impact on the environment.

## Requirements

* A Tesla Powerwall
* [Home Assistant](https://www.home-assistant.io/)
* [Tesla Powerwall integration](https://www.home-assistant.io/integrations/powerwall)
* [Tesla Powerwall Custom integration](https://github.com/alandtse/tesla)

## Rationale

The Powerwall itself permits a level of control via the app, however it is designed to be as automated as possible. And honestly it does a very good job at it. If you would like to take more control yourself or have Home Assistant control it further... this is not possible directly. **But** by using the level of control provided by Tesla, the Powerwall can be *led* into doing what you would like, which could be different than what the Tesla software decided it should do.

It is important to note that this method of control does not make the Powerwall do anything it is not allowed to do. Any restrictions imposed by Tesla will remain in place. For example if you are unable to export to the grid, then you will still be unable to export to the grid.

### Operation modes

To achieve this, I have developed a list of **scripts** that expand what the Tesla app provides in terms of operation modes. There are individual scripts that setup the Powerwall up to operate as follows:

| Mode | Description | HA name | Dependencies/control | 
|------|-------------|---------|----------------------|
| [Force grid charge](scripts/grid_charge.yaml) | Battery charges from the grid (1.8kW or 5kW) | `script.powerwall_mode_grid_charge` | `input_boolean.powerwall_battery_charge_rate_slow` `input_boolean.powerwall_force_full_charge` |
| [Hold charge](scripts/hold_charge.yaml) | Battery will not discharge, but it may charge | `script.powerwall_mode_no_discharge` | |
| [Force export\*](scripts/force_export.yaml) | Battery dumps its charge onto the grid | `script.powerwall_mode_force_export` | |
| [Time based solar](scripts/time_based_solar.yaml) | Same as 'Time based control' on the app, with grid export disabled | `script.powerwall_mode_time_based_solar` | `input_number.powerwall_battery_backup_reserve_operation` |
| [Self powered](scripts/self_powered.yaml) | default mode, grid export disabled | `script.powerwall_mode_default` | |

Additionally to the scripts above, a helper selector is used to set and query which operational mode the Powerwall has been set to.



### Toggle controls - input_boolean

These are **input_boolean** helper entities.

| Toggle | Description | HA name | Affects |
|--------|-------------|---------|---------|
| Charge Rate Slow | sets the battery to charge at 1.8kW from the grid | `input_boolean.powerwall_battery_charge_rate_slow` | `script.powerwall_mode_grid_charge` |
| Battery Export Charge | lets the battery dump charge onto the grid | `input_boolean.powerwall_battery_export_charge` | `script.powerwall_mode_time_based_solar` |
| Export Solar Surplus | prioritises exporting of solar energy vs feeding the house (i.e. battery discharges to support house consumption) | `input_boolean.powerwall_export_solar_surplus` | |
| Force Full Charge | ensures that battery charges fully | `input_boolean.powerwall_force_full_charge` | `script.powerwall_mode_grid_charge` |

### Helper controls - input_number

| Control | Description | HA name | Affects |
|---------|-------------|---------|---------|
| Battery reserve percentage | Sets the reserve percentage on scripts that require manipulating this parameter | `input_number.powerwall_battery_backup_reserve_operation` | `script.powerwall_mode_default` `script.powerwall_mode_time_based_solar` |

### Schedules

Energy providers will often provide time-dependent tariffs, usually in the shape of two time periods where one is higher demand so more expensive and the other is lower demand so cheaper. In other cases, providers will bill dynamically throughout the day, in half-hour periods, depending on many factors that influence the cost and carbon impact of buying, selling and supplying energy on the network.

Therefore, manually set schedules are required for automations, which is my case. Your automations may require other inputs for taking decisions.

### Automations

