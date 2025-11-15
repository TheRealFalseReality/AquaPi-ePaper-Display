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

All Home Assistant sensor entity IDs must be configured through substitutions. By default, generic placeholder values are set. Replace these with your actual Home Assistant sensor entity IDs.

### Default Configuration (Placeholders)

The default configuration uses generic placeholders that will display "Not Set" on the screen:

```yaml
substitutions:
  sensor_temp_entity: "sensor.my_temperature_entity"
  sensor_ph_entity: "sensor.my_ph_entity"
  sensor_do_entity: "sensor.my_dissolved_oxygen_saturation_entity"
  sensor_orp_entity: "sensor.my_orp_entity"
  sensor_salinity_entity: "sensor.my_salinity_entity"
  sensor_power_entity: "sensor.my_aquarium_power_entity"
  sensor_age_entity: "sensor.my_aquarium_age_entity"
  sensor_analysis_entity: "sensor.my_overall_analysis_entity"
  sensor_camera_analysis_entity: "sensor.my_camera_visual_analysis_entity"
  sensor_water_level_entity: "sensor.my_water_level_entity"
```

### Configuring Your Sensors

To use your own sensors, override these substitutions with your actual Home Assistant sensor entity IDs:

```yaml
packages:
  core: !include aquapi_epaper_config.yaml

substitutions:
  # Replace these with your actual Home Assistant sensor entity IDs
  sensor_temp_entity: "sensor.my_aquarium_temperature"
  sensor_ph_entity: "sensor.my_aquarium_ph"
  sensor_do_entity: "sensor.my_dissolved_oxygen"
  sensor_orp_entity: "sensor.my_orp"
  sensor_salinity_entity: "sensor.my_salinity"
  sensor_power_entity: "sensor.my_aquarium_power"
  sensor_age_entity: "sensor.my_aquarium_age"
  sensor_analysis_entity: "sensor.my_overall_analysis"
  sensor_camera_analysis_entity: "sensor.my_camera_visual_analysis"
  sensor_water_level_entity: "sensor.my_water_level"
```

**Note:** Any sensor that is not configured (left as default placeholder) will display "Not Set" on the ePaper display instead of a value and unit.

## Installation

You can use the button on our [project page](https://therealfalsereality.github.io/AquaPi-ePaper-Display/) to install the pre-built firmware directly to your device via USB from the browser using ESP Web Tools.
