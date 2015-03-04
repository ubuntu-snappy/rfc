# OEM snappy package

The `oem` snappy package is a snappy package `type` that is used to setup and
personalize the system according to an OEM.

It covers a broad range, such as the software stack with it’s configuration and
hardware enablement.

There can only be *one* snappy package of `type` `oem` and it can only be
installed during image provision.

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

Each snappy part can be configured per package or feeding a full configuration
with all the parts and types.

The `oem` package, in it’s initial version shall support providing a
`config.yaml` describing each part that is to be configured.

On first boot of the system, this configuration file will be processed to apply
such configuration. Take notice that a factory reset creates a first boot scenario.

Where possible and relevant, and specifically the `ubuntu-core` configuration
part will feed into `cloud-init`.

### Store ID

If a different sore is required, it can be set with the `store-id` entry,
`snappy` will use the store id set to reach the appropriate store.

### Branding

Branding can be set in the form of a slogan and an image, `snappy` and it’s
`webdm` counterpart will use this information to brand the system accordingly.

### Hardware

#### dtb

The default dtb can be overridden by a key entry point in the `package.yaml`
for the `oem`.

During provisioning if a dtb is specified, it will be selected as the dtb to
use for the system. When an update for the `oem` package arrives and in the
case of an AB partition layout, if the `dtb` is updated, the update will be
installed to the *other* partition and request a reboot.

An upgrade path must be calculated by `snappy` to determine the priority and
ordering for an `ubuntu-core` update and an `oem` one.

#### Bootloaders

Each system boots differently, assets that are currenly provided in
`flashassets` in the device part can be provided here, this is important if the
system desires to use the default `device` part which is the fully supported
Ubuntu kernel and initrds.

Examples of assets provided here are `MLO`, `u-boot`, `UEnv.txt` `script.bin`
or anything external to the system that allows for a system to boot.

These assets are meant to be used during provisioning, but can also be used against
a running system, updating these assets on a running system can lead to a broken
system as there may not be redundancy or fallback here (unless provided by the OEM).

#### Description

This is basically what is currently provided in the `hardware.yaml` from the
current device part sans init and kernel setup.


## Structure and layout

TODO
