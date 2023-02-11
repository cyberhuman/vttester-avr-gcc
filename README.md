# Initial configuration

## Set up build directories for required chip(s)

### ATmega16
```sh
meson setup --cross-file atmega16-cross.ini build16
```

### ATmega32
```sh
meson setup --cross-file atmega32-cross.ini build32
```

# Build

## Go to the build directory for the required chip

### ATmega16

```sh
cd build16
```

### ATmega32

```sh
cd build32
```

## Build

```sh
ninja
```

The above command will produce two files: `firmware.hex` (Flash data) and `firmware.eep` (EEPROM data).
Flash them to the microcontroller using the programmer of your choice.
