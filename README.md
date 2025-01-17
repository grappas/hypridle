# hypridle
Hyprland's idle daemon

## Features
 - based on the `ext-idle-notify-v1` wayland protocol
 - support for dbus' loginctl commands (lock / unlock / before-sleep)
 - support for dbus' inhibit (used by e.g. firefox / steam)

## Configuration

Configuration is done via `~/.config/hypr/hypridle.conf` in the standard
hyprland syntax.

```ini
general {
    lock_cmd = notify-send "lock!"          # dbus/sysd lock command (loginctl lock-session) 
    unlock_cmd = notify-send "unlock!"      # same as above, but unlock
    before_sleep_cmd = notify-send "Zzz"    # command ran before sleep
    after_sleep_cmd = notify-send "Awake!"  # command ran after sleep
    ignore_dbus_inhibit = false             # whether to ignore dbus-sent idle-inhibit requests (used by e.g. firefox or steam)
}

listener {
    timeout = 500                            # in seconds
    on-timeout = notify-send "You are idle!" # command to run when timeout has passed
    on-resume = notify-send "Welcome back!"  # command to run when activity is detected after timeout has fired.
}
```

You can add as many listeners as you please. Omitting `on-timeout` or `on-resume` (or leaving them empty)
will make those events ignored.

## Dependencies
 - wayland
 - wayland-protocols
 - hyprlang >= 0.4.0
 - sdbus-c++

## Building & Installation

Building:
```sh
cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -S . -B ./build
cmake --build ./build --config Release --target hypridle -j`nproc 2>/dev/null || getconf NPROCESSORS_CONF`
```

Installation:
```sh
sudo cmake --install build
```
