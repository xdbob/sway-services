# sway-services

A collection of tools, utilities and mostly unit files to handle a systemd-user
sway session.

## Usage

Run `Sway Service` from your desktop manager or run `sway-user-service`.

## Provided targets

* `sway-session.target`
* `sway-session-pre.target`
* `wayland-session.target`
* `wayland-session-pre.target`

These targets are directly mapped to `graphical-session{,-pre}.target` but for
wayland exclusive services

## Provided services

* `sway.service`
* `kanshi.service`
* `mako.service`
* `swayidle.service`

## How this works

Here's a summary of the flow of how this works. See the linked files for details.

A [`sway.desktop`](./wayland-sessions/sway-session.desktop) file will be installed in a `wayland-sessions` directory. Desktop managers
like `gdm` will detect this and use it to present Sway Service as a menu option.

Once selected, the provided [`sway-user-service`](./bin/sway-user-service) script will be executed.
This script:
   1. sources `/etc/profile`
   2. sets `XDG_CURRENT_DESKTOP=sway`
   3. exports environment variables to the user's systemd instance
   4. Starts the provided `sway.service`

[`sway.service`](./systemd/sway.service) does the following:

  1. Runs all the units associated with `sway-session-pre.target`
  2. Sources ~/.config/sway/env
  3. Runs `sway` binary
  4. Runs all units associated with `sway-session.target` (See "Provided services" above.)

When the `sway` binary runs, it will run the [`config/sway/10-service.conf`] file that you must
load yourself. This handles loading environment variables that Sway sets back into `systemd`'s user
environment.

The provided services are not enabled by default. If you want enable `mako` example,
use:

    systemctl --user enable mako

Once enabled, the services will start when launch Sway.

## How to enable additional systemd user services

You can install additional systemd user services designed for use with Sway in
`~/.config/systemd/user` or packages may install them in  `/usr/lib/systemd/user`.

The systemd unit files should contain an `[Install]` section like this:


```
# Run for all Wayland sessions
[Install]
WantedBy=wayland-session.target
```

```
# Run just for Sway sessions
[Install]
WantedBy=sway-session.target
```

There is also `sway-session-pre.target` and `wayland-session.target` if you want to run something
just before Sway or another Wayland window manager. For example, this would be a good time
to launch a GPG or SSH agent that wants to set it's own environment variables to share with
later apps.

## Notes

The `/etc/sway/config.d/10-service.conf` file must be loaded by Sway.

`swayidle.service` will load a configuration file, an example can be found in
`examples/idle.yaml` in this repository.
