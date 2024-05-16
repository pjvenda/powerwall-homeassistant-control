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

To achieve this, I have developed a list of **scripts** that expand what the Tesla app provides in terms of operation modes. The scripts can operated directly or via a selector and an automation. More detailed info can be found in the wiki [Operation modes](../../wiki/Custom-operation-modes).

| Mode | Description |
|------|-------------|
| [Force grid charge](scripts/force_charge.yaml) | Battery charges from the grid (1.8kW or 5kW) |
| [Hold charge](scripts/hold_charge.yaml) | Battery will not discharge, but it may charge |
| [Force export\*](scripts/force_export.yaml) | Battery dumps its charge onto the grid |
| [Time based solar](scripts/time_based_solar.yaml) | Same as 'Time based control' on the app, with grid export disabled |
| [Self powered](scripts/self_powered.yaml) | default mode, grid export disabled |

(*) The [Force export](../../wiki/Custom-operation-modes#force-export) mode relies on a utility rate plan on the app with a 'sell price' at least higher than the 'buy price' at the time you wish to export. In my case the sell price is over twice as high as the buy price, which is entirely artificial. But it is the only way I found that *coerces* the Powerwall into exporting to the grid. The way to control grid exports in addition to having the utility rate plan (maybe you don't always want to export, or not for the full period in which the utility rate plan makes this favourable...) is to switch the Powerwall's 'Energy exports' between 'Everything' and 'Solar'.

### Control knobs

More detailed info can be found in the wiki [Controls](../../wiki/Controls).

| Knob | Description |
|--------|-------------|
| Charge Rate Slow | sets the battery to charge at 1.8kW from the grid |
| Battery Export Charge | lets the battery dump charge onto the grid |
| Export Solar Surplus | prioritises exporting of solar energy vs feeding the house (i.e. battery discharges to support house consumption) |
| Force Full Charge | ensures that battery charges fully |
| Battery reserve percentage | Sets the reserve percentage on scripts that require manipulating this parameter |
| Battery Custom modes | Displays or selects the custom mode of operation, as determined by the mode scripts |
