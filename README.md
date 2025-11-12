# AquaPi ePaper Display

An ESP32-based ePaper display showing AquaPi & aquarium data from Home Assistant.

This project provides a firmware for ESP32 ePaper displays that shows real-time aquarium sensor data including temperature, pH, salinity, ORP, dissolved oxygen, power consumption, and more.

## Features

- Real-time display of aquarium sensor data
- 7.5" ePaper display support
- WiFi connectivity with deep sleep for power efficiency
- Customizable Home Assistant sensor integration
- OTA updates support

## Customizing Sensor Entity IDs

All Home Assistant sensor entity IDs can be customized through substitutions in the configuration. To use your own sensors, simply override the substitution values in your ESPHome configuration:

### Available Sensor Substitutions

Add these to your `substitutions` section in your ESPHome YAML to customize which Home Assistant sensors are displayed:

```yaml
substitutions:
  # Temperature sensor
  sensor_marine_temp_entity: "sensor.your_temperature_sensor"
  
  # pH sensor
  sensor_marine_ph_entity: "sensor.your_ph_sensor"
  
  # Dissolved oxygen sensor
  sensor_marine_do_entity: "sensor.your_dissolved_oxygen_sensor"
  
  # ORP (Oxidation-Reduction Potential) sensor
  sensor_marine_orp_entity: "sensor.your_orp_sensor"
  
  # Salinity sensor
  sensor_marine_salinity_entity: "sensor.your_salinity_sensor"
  
  # Power consumption sensor
  sensor_marine_power_entity: "sensor.your_power_sensor"
  
  # Aquarium age text sensor
  sensor_marine_age_entity: "sensor.your_aquarium_age_sensor"
  
  # Overall analysis text sensor
  sensor_marine_analysis_entity: "sensor.your_analysis_sensor"
  
  # Water level text sensor
  sensor_marine_water_level_entity: "sensor.your_water_level_sensor"
```

### Example Configuration

If you want to use different sensors, create a custom YAML file that imports the base configuration and overrides the sensors:

```yaml
packages:
  core: !include aquapi_epaper_config.yaml

substitutions:
  sensor_marine_temp_entity: "sensor.my_aquarium_temperature"
  sensor_marine_ph_entity: "sensor.my_aquarium_ph"
  # ... add your other custom sensors here
```

## Installation

You can use the button on our [project page](https://therealfalsereality.github.io/AquaPi-ePaper-Display/) to install the pre-built firmware directly to your device via USB from the browser using ESP Web Tools.
