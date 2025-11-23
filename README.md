# AquaPi ePaper Display

<img src="/assets/PXL_20251123_184247543.png" alt="display"/>

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
  sensor_water_level_entity: "sensor.my_water_level_entity"
  sensor_weather_temp_entity: "sensor.my_weather_temperature_entity"
  sensor_evaluation_entity: "sensor.my_evaluation_entity"
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
  sensor_water_level_entity: "sensor.my_water_level"
  sensor_weather_temp_entity: "sensor.my_weather_temperature"  # Optional weather temperature
  sensor_evaluation_entity: "sensor.my_evaluation"  # Optional quick evaluation
```

**Note:** Any sensor that is not configured (left as default placeholder) will display "Not Set" on the ePaper display instead of a value and unit.

### Weather Temperature Display

The display includes an optional weather temperature sensor that appears to the left of the time in the header with a subtle separator. If the `sensor_weather_temp_entity` is configured with a valid Home Assistant weather temperature sensor, the current temperature will be displayed with the unit of measurement. If not configured or unavailable, the area remains blank to maintain a clean display layout.

### Quick Evaluation Display

The display includes an optional quick evaluation sensor that appears next to the aquarium name with a bullet separator (•). If the `sensor_evaluation_entity` is configured with a valid Home Assistant sensor, a one-word evaluation (e.g., "Excellent", "Good", "Fair") will be displayed. If not configured or unavailable, only the aquarium name is shown.

## Creating an Aquarium Age Sensor

To display the age of your aquarium (e.g., "2 years, 3 months"), you need to create a custom sensor in Home Assistant. This involves two steps:

### Step 1: Create an Input DateTime Helper

First, create an `input_datetime` helper to store your aquarium's start date:

1. In Home Assistant, go to **Settings** → **Devices & Services** → **Helpers**
2. Click **+ Create Helper** and select **Date and/or time**
3. Configure the helper:
   - **Name**: "Aquarium Start Date" (or your preferred name)
   - **Icon**: `mdi:calendar` (optional)
   - Enable **Date** (time is not needed)
4. Save the helper and set it to your aquarium's start date

This will create an entity like `input_datetime.aquarium_start_date`.

### Step 2: Create a Template Sensor

Next, create a template sensor that calculates and formats the aquarium age. Add the following to your Home Assistant `configuration.yaml` file:

```yaml
template:
  - sensor:
      - name: "Aquarium Age"
        unique_id: aquarium_age
        state: >
          {% set sensor = states('input_datetime.<AQUARIUM-STARTDATE>') | as_datetime %}
          {% set now = now() %}
          {% set nowTime = now.replace(tzinfo=None) %}
          {% set diff = nowTime - sensor %}
          {% set years = diff.days // 365 %}
          {% set remaining_days = diff.days % 365 %}
          {% set months = remaining_days // 30 %}

          {% if years > 0 %}
            {{ years }} year{{ 's' if years > 1 else '' }}{% endif %}{% if months > 0 %}{% if years > 0 %}, {% endif %}{{ months }} month{{ 's' if months > 1 else '' }}
          {% endif %}
```

**Important**: Replace `<AQUARIUM-STARTDATE>` with the actual entity ID of your input_datetime helper (e.g., `aquarium_start_date`).

**Note**: This template uses approximate calculations (365 days per year, 30 days per month) which is suitable for display purposes.

After adding this configuration:
1. Restart Home Assistant to load the new template sensor
2. The sensor will be created as `sensor.aquarium_age`
3. Configure your ePaper display to use this sensor by setting `sensor_age_entity: "sensor.aquarium_age"` in your substitutions

## Installation

You can use the button on our [project page](https://therealfalsereality.github.io/AquaPi-ePaper-Display/) to install the pre-built firmware directly to your device via USB from the browser using ESP Web Tools.
