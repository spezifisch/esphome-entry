# esphome-entry

A custom box to detect when someone rings on my analog doorbell.

## Building the Hardware

### Sound Sensor

We're using one of these sound sensors which use an LM393 comparator for threshold detection:

![PCB layout of the sound sensors](./docs/sound%20sensor.png)

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

### Development

```console
# install pre-commit python package
$ pipx install pre-commit

# install git commit hook in this repository
$ pre-commit install

# (optional) lint + fix all files in this repo
$ pre-commit run --all-files
```
