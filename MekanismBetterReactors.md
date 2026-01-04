# Better Fusion Reactor ComputerCraft Documentation

## Overview
Better Fusion Reactor is an addon for Mekanism Generators that makes fusion reactor management more complex and realistic. Instead of a simple "furnace generator," it requires controlling parameters like reactivity, efficiency, and error levels to maintain stable fusion reactions. The mod supports ComputerCraft integration for automation.

## Key Concepts
- **Reactivity**: Current reactivity must closely match target reactivity for high efficiency. Target reactivity changes dynamically based on plasma temperature.
- **Efficiency**: Calculated as a percentage (0-100%). Efficiency > 80% reduces error level; < 80% increases it. Efficiency affects energy, heat, and steam generation.
- **Error Level**: Accumulates if efficiency is low. At 100%, the reactor shuts down and requires re-ignition. Plasma temperature and cooling affect error accumulation rate.
- **Injection Rate**: Controls fuel injection (even numbers from 0 to 500). Higher rates allow higher temperatures but require more cooling.
- **Logic Modes**: The Logic Adapter can output redstone signals based on reactor state (READY, CAPACITY, DEPLETED, EFFICIENCY, ERROR_LEVEL) or accept inputs to adjust injection/reactivity (INJECTION_UP/DOWN, REACTIVITY_UP/DOWN).
- **Cooling**: Active cooling (with coolant) allows higher stable temperatures. Passive cooling limits performance.
- **Hohlraum**: Required to ignite the reactor. Contains fuel that vaporizes on ignition.

## ComputerCraft Integration
Connect a ComputerCraft computer or turtle to the **Fusion Reactor Logic Adapter** block. Use `peripheral.wrap(side)` or `peripheral.find("fusion_reactor_logic_adapter")` to access the peripheral.

### Available Methods
Call methods using `peripheral.call(methodName, args...)`.

#### Reactor State & Control
- `getEfficiency()`: Returns current efficiency (float, 0-100).
- `getErrorLevel()`: Returns current error level (float, 0-100).
- `adjustReactivity(val)`: Adjusts reactivity by val (float, -100 to 100). Throws exception if out of range.
- `getInjectionRate()`: Returns current injection rate (int).
- `setInjectionRate(rate)`: Sets injection rate (int, even number 0-500). Throws exception if invalid.
- `getMinInjectionRate(active)`: Returns minimum injection rate for stability (int). `active`: boolean (true if actively cooled).
- `getMaxPlasmaTemperature(active)`: Returns maximum plasma temperature (double). `active`: boolean.
- `getMaxCasingTemperature(active)`: Returns maximum casing temperature (double). `active`: boolean.
- `getIgnitionTemperature(active)`: Returns ignition temperature (double). `active`: boolean.
- `getPassiveGeneration(active)`: Returns passive energy generation rate (FloatingLong). `active`: boolean.
- `getProductionRate()`: Returns current production rate (FloatingLong).

#### Tanks
Tanks return {type, amount} tables or direct values. Methods include capacity, needed, and filled percentage.

- **Deuterium Tank**: `getDeuterium()`, `getDeuteriumCapacity()`, `getDeuteriumNeeded()`, `getDeuteriumFilledPercentage()`
- **Tritium Tank**: `getTritium()`, `getTritiumCapacity()`, `getTritiumNeeded()`, `getTritiumFilledPercentage()`
- **DT Fuel Tank**: `getDTFuel()`, `getDTFuelCapacity()`, `getDTFuelNeeded()`, `getDTFuelFilledPercentage()`
- **Gas Coolant Tank**: `getCoolant()`, `getCoolantCapacity()`, `getCoolantNeeded()`, `getCoolantFilledPercentage()`
- **Liquid Coolant Tank**: `getLiquidCoolant()`, `getLiquidCoolantCapacity()`, `getLiquidCoolantNeeded()`, `getLiquidCoolantFilledPercentage()`
- **Hot Coolant Tank (Steam)**: `getSteam()`, `getSteamCapacity()`, `getSteamNeeded()`, `getSteamFilledPercentage()`

#### Slots
- `getHohlraum()`: Returns the Hohlraum item stack (or nil).

#### Logic Adapter Specific
- `isActiveCooledLogic()`: Returns if active cooling is enabled (boolean).
- `getLogicMode()`: Returns current logic mode (FusionReactorLogic enum).
- `setLogicMode(mode)`: Sets logic mode (FusionReactorLogic).
- `setActiveCooledLogic(active)`: Enables/disables active cooling (boolean).

#### FusionReactorLogic Enum
- Output Modes: `READY`, `CAPACITY`, `DEPLETED`, `EFFICIENCY`, `ERROR_LEVEL`
- Input Modes: `INJECTION_UP`, `INJECTION_DOWN`, `REACTIVITY_UP`, `REACTIVITY_DOWN`

## Example Lua Script
```lua
local reactor = peripheral.find("fusion_reactor_logic_adapter")
if reactor then
    print("Efficiency: " .. reactor.getEfficiency())
    reactor.setInjectionRate(10)
    reactor.adjustReactivity(5.0)
end
```

## Monitor Display Script
```lua
-- Fusion Reactor Monitor Script for ComputerCraft
local reactor = peripheral.find("fusion_reactor_logic_adapter")
if not reactor then
    print("Fusion Reactor Logic Adapter not found!")
    return
end

local monitor = peripheral.find("monitor")
if monitor then
    term.redirect(monitor)
    monitor.setTextScale(0.5)
else
    print("Monitor not found, using terminal")
end

while true do
    term.clear()
    term.setCursorPos(1, 1)

    print("=== Better Fusion Reactor Monitor ===")
    print("")
    print("Efficiency: " .. tostring(reactor.getEfficiency()) .. "%")
    print("Error Level: " .. tostring(reactor.getErrorLevel()) .. "%")
    print("Injection Rate: " .. tostring(reactor.getInjectionRate()))
    -- Add more prints as needed...

    sleep(1)
end
```

## Fission Reactor Support
The mod also enhances Mekanism's fission reactors. Connect to the **Fission Reactor Logic Adapter** (`peripheral.find("fission_reactor_logic_adapter")`).

### Fission Methods
- `getFuel()`: Returns {gasType, amount}
- `getDamagePercent()`: Returns damage percentage (long)
- `activate()`: Starts the reactor
- `scram()`: Stops the reactor
- `setBurnRate(val)`: Sets burn rate (double, 0 to max)
- `getMaxBurnRate()`: Returns max burn rate (long)
- `isActive()`: Returns if active (boolean)
- `isForceDisabled()`: Returns if force disabled (boolean)
- `getHeatingRate()`: Returns heating rate (double)
- `getTemperature()`: Returns temperature (double)
- `getFuelSurfaceArea()`: Returns fuel surface area (double)
- `getEnvironmentalLoss()`: Returns environmental loss (double)
- `getBurnRate()`: Returns burn rate (double)
- `getActualBurnRate()`: Returns actual burn rate (double)
- `getBoilEfficiency()`: Returns boil efficiency (double)
- `getWaste()`: Returns {gasType, amount}
- `getCoolant()`: Returns {gasType, amount}
- `getLiquidCoolant()`: Returns {fluidType, amount}
- `getHeatedCoolant()`: Returns {gasType, amount}

For full Mekanism ComputerCraft documentation, refer to the official Mekanism wiki, as this mod builds upon it.
