# OEM snappy package

The `oem` snappy package is a snappy package `type` that is used to setup and
personalize the system according to an OEM.

It covers a broad range, such as the software stack with it’s configuration and
hardware enablement.

There can only be *one* snappy package of `type` `oem` and it can only be
installed during image provision.

TODO: define that the oem snap may setup snappy `parts` and define what a
`part` is.

## Customization entry points

### Default packages

The `oem` snap can provide a set of default packages to be installed during
either provisioning or first boot. The former is interesting to IoT scenarios
while the latter is useful for cloud deployments (although `cloud-init`
directly also serves the purpose for pure clouds).

In the case of first boot provisioning, the data is just fed into cloud-init for
the appropriate actions to take place.

For the preinstalled case, the provisioning tool will download the package
selection from the store and provision onto the system.

### Snappy package configuration

Each snappy part can be configured per package or by feeding a full
configuration with all the parts and types.

The `oem` package shall initially support providing a `config.yaml` describing
each part that is to be configured.

On first boot of the system, this `config.yaml` file will be processed and the
described configuration will be applied. Note: factory resetting the system
will create a first boot scenario and therefore `config.yaml` will be
processed.

FIXME (confusing): Where possible and relevant, and specifically the
`ubuntu-core` configuration part will feed into `cloud-init`.

### Store ID

If a non-default store is required, one may use the `store/id` entry and
`snappy` will use it to reach the appropriate store.

### Branding

Branding can be set in the form of a slogan and an image. `snappy` and it’s
`webdm` counterpart will use this information to brand the system accordingly.

### Hardware

#### dtb

The default dtb (device tree blob) can be overridden by a key entry point in
the `package.yaml` for the `oem`.

If a dtb is specified during provisioning, it will be selected as the dtb to
use for the system. If using an AB partition layout, when an update for the
`oem` package is installed which updates the `dtb`, the update will be
installed to the *other* partition and a reboot will be requested.

An upgrade path must be calculated by `snappy` to determine the priority and
ordering for an `ubuntu-core` update and an `oem` snap update.

#### Bootloaders

Since each system boots differently, assets that are currently provided in
`flashassets` of the device part can be provided in the `oem` snap instead.
This is useful for systems that use the default `device` part (ie, one which
uses the officially supported Ubuntu kernel and initrd).

Examples of assets that may be provided via the `oem` snap are `MLO`, `u-boot`,
`UEnv.txt` `script.bin` or anything external to the system that allows for a
system to boot.

While these assets are typically used during provisioning, they may also be
used against a running system. Caution: updating these assets on a running
system may lead to a broken system unless redundancy or fallback machanisms
aren provided by the OEM.

#### Description

FIXME (do not use 'basically' here): This is basically what is currently provided in the `hardware.yaml` from the
current device part sans init and kernel setup.


## Structure and layout

TODO
