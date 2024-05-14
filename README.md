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

### Operation modes - Scripts

To achieve this, I have developed a list of scripts that expand what the Tesla app provides in terms of operation modes. There are individual scripts that setup the Powerwall up to operate as follows:

* Force grid charge (1.8kW or 5kW) - battery charges from the grid
* Hold charge - battery will not discharge, but it may charge
* Force export\* - battery dumps its charge onto the grid
* Time based solar - same as 'Time based control' on the app, with grid export disabled
* Self powered - default mode, grid export disabled

Additionally to the scripts above, a helper selector is used to set and query which operational mode the Powerwall has been set to.

### Toggle controls

(...)
