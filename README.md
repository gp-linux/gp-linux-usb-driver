[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](./CODE_OF_CONDUCT.md)

# Custom USB class driver for Linux

> Copyright (c) 2024, Greg PFISTER. MIT License.

## About

This is an implementation of a generic USB driver. It showcases the firmware 
that can be found [here](https://github.com/gp-nrf/gp-custom-usb-class-device),
which is turn is a generic driver.

The purpose is to take advantage of a CDC ACM interface and vendor specific 
class. The vendor specific class is rather minimal, it takes an input and
returns it with an interrupt.

A major reference to for this has been these series of tutorials:
- in general about [Kernel Driver development](https://youtube.com/playlist?list=PLCGpd0Do5-I3b5TtyqeF1UdyD4C-S-dMa&si=EE-RCoOJz7SmPOlm)
- in particular about [Kernel USB Driver development](https://youtube.com/playlist?list=PLCGpd0Do5-I0LUuFImUlkj2RhoiMCFPhe&si=Y7q5ukpzQf2t6X1n)

## Requirements

### Hardware

The following instruction have been tested on Ubuntu 24.04 and 24.10, using a 
Raspberry Pi 4 et 5.

There should be no reason why this wouldn't work on an Intel platform...

### Build environment

Install the build environment:

```zsh
sudo apt install -y build-essential           \
                    kmod                      \
                    dwarves                   \
                    linux-headers-$(uname -r) \
                    clang-format
```

Then reboot. Once rebooted, copy the vmlinux for BTF generation:

```zsh
sudo cp /sys/kernel/btf/vmlinux /usr/lib/modules/$(uname -r)/build/
```

### VSCode

You may set the following C/C++ properties in `.vscode/c_cpp_properties.json`:

```json
{
  "configurations": [
    {
      "name": "Linux",
      "includePath": [
        "${workspaceFolder}/**", 
        "/usr/src/linux-headers-6.8.0-51-generic/include/**", 
        "/usr/src/linux-headers-6.8.0-51-generic/arch/arm64/include/**"
      ],
      "defines": ["__KERNEL__", "MODULE"],
      "compilerPath": "/usr/bin/aarch64-linux-gnu-gcc-13",
      "cStandard": "c17",
      "intelliSenseMode": "linux-gcc-arm64",
      "configurationProvider": "ms-vscode.cmake-tools"
    }
  ],
  "version": 4
}
```

Do not forget to remplace `6.8.0-51-generic` by the result of `uname -r`.

## Build

Simply run the `make` command:

```zsh
make
```

You can adjust the `make` command if you get a warning
**the compiler differs from the one used to build the kernel**:

```
warning: the compiler differs from the one used to build the kernel
  The kernel was built by: aarch64-linux-gnu-gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
  You are using:           gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
```

For the above example, simple run:

```zsh
make CROSS_COMPILE=aarch64-linux-gnu-
```

## Load and test

To load the the module, simply run the `insmod` command:

```zsh
sudo insmod gp_usb.ko
```

Insert a compatible device (see
[here](https://github.com/gp-nrf/gp-custom-usb-class-device)), then check the
device is detected:

```zsh
sudo dmesg | grep gp_usb
```

It would diplay the following:

```
TO BE ADDED...
```

To unload the module, run:

```zsh
sudo rmmod gp_usb
```

## Contributions

Feel free to contribute, but please follow the
[contribution guidelines](./CONTRIBUTING.md).

When interacting, please adopt this [code of conduct](./CODE_OF_CONDUCT.md).

## License

Please read more about the license [here](./LICENSE.md).
