# esphome-entry

A custom box to:

* detect if someone rings on my analog doorbell
* trigger a visual doorbell
* integrate with HomeAssistant

(*You can also use the Status LED of the ESP32 board as a proper light in HomeAssistant in case you want to use it as a notification or something.*)

## Building the Hardware

### Schematic

![Schematic built around an ESP32-C3 SuperMini](./docs/esphome-entry%20schematic%20export.png)

* The idea here is that we use the sound detector to trigger an event for HomeAssistant when it's loud enough (our primary audible doorbell rings)
* This also triggers the remote of a secondary (visual) doorbell (a cheap wireless doorbell set with two LED units/wall warts)
* The remote runs from a 12 V battery (A23), therefore the optocoupler instead of hooking us directly into it. The optocoupler's output is just connected to the two sides of the push button of the remote.
* Our ESP32 and sound sensor are powered by the ESP32 board's 3.3 V regulator. That regulator is powered by 5 V from the USB-C port.

### Sound Sensor

We're using one of these sound sensors which use an LM393 comparator for threshold detection:

![PCB layout of the sound sensors](./docs/sound%20sensor.png)

Reverse-engineered schematic from that (LEDs and 100n omitted):

![Sound sensor schematic around an LM393](./docs/sound%20sensor%20schematic%20export.png)

It's basically just an amplifier for the electret microphone and the comparator *compares* that amplified voltage to the reference voltage level set by the potentiometer.
The output (`OUT`) signal is driven low when suffiently loud sound is detected, otherwise it's driven high.
The green LED near the output lights when `OUT` is low. (The other LED is just always on and I removed it later to save power.)

## Building the Software

### Install Dependencies

```console
$ pipx install esphome
```

### Configuration

```console
# add your secrets
$ cp -i ./secrets.example.yaml secrets.yaml
$ vim ./secrets.yaml
```

### Compile + Flash

```console
$ esphome run esphome.yaml
```

## Configuration

For initial configuration, you can use the fallback WiFi AP, improv_serial, or just hard-code your WiFi.

### Fallback AP

You can connect to the fallback AP configured in `secrets.yaml` and set your proper WiFi credentials using the Web UI at port 80 of the gateway IP that you get via DHCP when connecting.

**NOTE**: For prebuilt GitHub releases this feature is *deactivated*, otherwise we would need to include a default password, which is insecure.

### HomeAssistant

This is plug & play with ESPHome's HomeAssistant integration. If the sensor is not auto-detected, just lookup its IP (e.g. from your router) and configure it manually.

## Development

```console
# install pre-commit python package
$ pipx install pre-commit

# install git commit hook in this repository
$ pre-commit install

# (optional) lint + fix all files in this repo
$ pre-commit run --all-files
```

## Troubleshooting

* If your doorbell remote isn't triggered reliably, play with the `switch.on_turn_on` delay parameter to vary how long the button is "pressed".

## Licenses

Schematics: CERN-OHL-S-2.0

Everything else (except .pdfs): GPL-3.0-only
