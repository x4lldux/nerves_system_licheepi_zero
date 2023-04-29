# Generic LicheePi Zero Support

<!-- [![Hex version](https://img.shields.io/hexpm/v/nerves_system_licheepi_zero.svg "Hex version")](https://hex.pm/packages/nerves_system_licheepi_zerolicheepi_zero) -->

This is the base Nerves System configuration for the
[LicheePi Zero](https://www.licheepizero.us/)

![LicheePi Zero](assets/images/licheepi_zero.jpg)

| Feature        | Description                                                 |
|----------------|-------------------------------------------------------------|
| CPU            | Allwinner V3S, up to 1.2 GHz ARM single Cortex-A7           |
| Memory         | up to 64 MiBi DRAM                                          |
| Storage        | MicroSD card, optional SPI FLASH                            |
| Linux Kernel   | 5.15                                                        |
| IEx terminal   | UART0                                                       |
| GPIO, I2C, SPI | Yes - [Elixir Circuits](https://github.com/elixir-circuits) |
| ADC            | No                                                          |
| LRADC          | Yes                                                         |
| PWM            | Yes, but no Elixir support                                  |
| UART           | 2                                                           |
| Camera         | None                                                        |
| Ethernet       | PHY, Requries                                               |
| WiFi           | Requires SDIO or USB WiFi dongle                            |
| Audio          | Yes                                                         |

# WARNING

This system is still considered in alpha state, expect backwards incompatible changes
to be made

# Using

The most common way of using this Nerves System is create a project with `mix
nerves.new` and to export `MIX_TARGET=licheepi_zero`. See the [Getting started
guide](https://hexdocs.pm/nerves/getting-started.html#creating-a-new-nerves-app)
for more information.

If you need custom modifications to this system for your device, clone this
repository and update as described in [Making custom
systems](https://hexdocs.pm/nerves/customizing-systems.html).

# Supported WiFi Devices

_WIP_

## Provisioning devices

This system supports storing provisioning information in a small key-value store
outside of any filesystem. Provisioning is an optional step and reasonable
defaults are provided if this is missing.

Provisioning information can be queried using the Nerves.Runtime KV store's
[`Nerves.Runtime.KV.get/1`](https://hexdocs.pm/nerves_runtime/Nerves.Runtime.KV.html#get/1)
function.

Keys used by this system are:

Key                    | Example Value     | Description
:--------------------- | :---------------- | :----------
`nerves_serial_number` | `"12345678"`      | By default, this string is used to create unique hostnames and Erlang node names.

The normal procedure would be to set these keys once in manufacturing or before
deployment and then leave them alone.

For example, to provision a serial number on a running device, run the following
and reboot:

```elixir
iex> cmd("fw_setenv nerves_serial_number 12345678")
```

This system supports setting the serial number offline. To do this, set the
`NERVES_SERIAL_NUMBER` environment variable when burning the firmware. If you're
programming MicroSD cards using `fwup`, the commandline is:

```sh
sudo NERVES_SERIAL_NUMBER=12345678 fwup path_to_firmware.fw
```

Serial numbers are stored on the MicroSD card so if the MicroSD card is
replaced, the serial number will need to be reprogrammed. The numbers are stored
in a U-boot environment block. This is a special region that is separate from
the application partition so reformatting the application partition will not
lose the serial number or any other data stored in this block.

Additional key value pairs can be provisioned by overriding the default
provisioning.conf file location by setting the environment variable
`NERVES_PROVISIONING=/path/to/provisioning.conf`. The default provisioning.conf
will set the `nerves_serial_number`, if you override the location to this file,
you will be responsible for setting this yourself.
