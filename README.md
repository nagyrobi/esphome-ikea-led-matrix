# ESPHome IKEA LED matrix components for Frekvens & Obegränsad
Both Frekvens and Obegränsad are 16 by 16 LED Matrixes from IKEA and they are Hackable for Usage with HomeAssistant / ESPHome.

This Repository is a custom component for ESPHome to interface these two Matrixes. 

I will create a custom PCB for both Frekvens Cube and Obegränsad Panel that you can build yourself for a sleek integration. ❗❗❗TODO❗❗❗

## Wiring

You have to wire your ESP32 or ESP8266 according to this Schematics.

### Frekvens

❗❗❗TODO❗❗❗

### Obegränsad

❗❗❗TODO❗❗❗

## Software

Here is a short config to demonstrate the usage to display time on Frekvens and Obegränsad:

### Frekvens
```yaml
esphome:
  name: frekvens-clock
  platform: ESP8266
  board: d1_mini
  platformio_options:
    lib_deps:
      - Wire                            # Also required by GFX.
      - SPI                             # Also required by GFX.
      - adafruit/Adafruit BusIO         # Required by GFX Library.
      - adafruit/Adafruit GFX Library   # Required for FrekvensPanel.
      - me-no-dev/ESPAsyncTCP

external_components:
  - source: github://sascha-hemi/esphome-ikea-led-matrix@master
    components: [ frekvens_panel ]

light:
  - platform: monochromatic
    name: 'Brightness'
    output: matrix_brightness
    restore_mode: RESTORE_DEFAULT_ON

output:
  - platform: esp8266_pwm
    # Enables brightness control.
    max_power: 0.05
    id: matrix_brightness
    pin:
      number: GPIO14
      inverted: True

time:
  - platform: sntp
    id: ntp_time
    timezone: 'Europe/Berlin'

font:
  - file: "04B03.ttf"
    id: b03
    size: 8

display:
  - platform: frekvens_panel
    latch_pin: 12
    clock_pin: 04
    data_pin: 05

    lambda: |-
      it.strftime(4, 0, id(b03), "%H", id(ntp_time).now());
      it.strftime(4, 8, id(b03), "%M", id(ntp_time).now());

```

### Obegränsad
```yaml
esphome:
  name: obegraensad-clock
  platform: ESP8266
  board: d1_mini
  platformio_options:
    lib_deps:
      - Wire                            # Also required by GFX.
      - SPI                             # Also required by GFX.
      - adafruit/Adafruit BusIO         # Required by GFX Library.
      - adafruit/Adafruit GFX Library   # Required for FrekvensPanel.
      - me-no-dev/ESPAsyncTCP

external_components:
  - source: github://sascha-hemi/esphome-ikea-led-matrix@master
    components: [ obegraensad_panel ]

light:
  - platform: monochromatic
    name: 'Brightness'
    output: matrix_brightness
    restore_mode: RESTORE_DEFAULT_ON

output:
  - platform: esp8266_pwm
    # Enables brightness control.
    id: matrix_brightness
    pin:
      number: GPIO14
      inverted: True

time:
  - platform: sntp
    id: ntp_time
    timezone: 'Europe/Berlin'

font:
  - file: "04B03.ttf"
    id: b03
    size: 8

display:
  - platform: obegraensad_panel
    rotation: 90
    latch_pin: 12
    clock_pin: 04
    data_pin: 05

    lambda: |-
      it.strftime(4, 0, id(b03), "%H", id(ntp_time).now());
      it.strftime(4, 8, id(b03), "%M", id(ntp_time).now());

```
