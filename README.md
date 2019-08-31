# sway-services

A collection of tools, utilities and mostly unit files to handle a systemd-user
sway session.

Usage: run `Sway Service` from your desktop manager or run `sway-user-service`.

## Provided targets

* `wayland-session.target`
* `wayland-session-pre.target`

These targets are directly mapped to `graphical-session{,-pre}.target` but for
wayland exclusive services

## Provided services

* `sway.service`
* `kanshi.service`
* `mako.service`
* `swayidle.service`

## Notes

The `/etc/sway/config.d/10-service.conf` file must be loaded by sway.

`swayidle.service` will load a configuration file, an example can be found in
`examples/idle.yaml` in this repository
