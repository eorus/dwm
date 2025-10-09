# Eorus dwm Build V6.6 - dynamic window manager

For Thinkpad X220 I've tried to keep dwm as minimal as I can to use screen real estate and keyboard actions.

## Patches I've Used

* [dwm-alpha](https://dwm.suckless.org/patches/alpha/)
* [dwm-fibonacci](https://dwm.suckless.org/patches/fibonacci/)
* [dwm-fullgaps](https://dwm.suckless.org/patches/fullgaps/)
* [dwm-statuscolors](https://dwm.suckless.org/patches/statuscolors/)
* XF86Keys <code>#include <X11/XF86keysym.h></code>

## Font

<pre>
static const char *fonts[] = {
    "Inter:size=12",
    "JetBrains Mono:size=12"
};
static const char dmenufont[] = "Inter:size=12";
</pre>

### Main Keybindings

MODKEY = Super

| Keybinding               | Action                                                        |
| ------------------------ | ------------------------------------------------------------- |
| MODKEY + RETURN          | opens terminal (st)                                           |
| MODKEY + d               | opens dmenu                                                   |
| MODKEY + SHIFT + d       | opens passmenu                                                |
| MODKEY + SHIFT + RETURN  | cycle master/stack                                            |
| MODKEY + q               | quits window/program                                          |
| MODKEY + BACKSPACE       | powermenu (opens sysact locak, logout, restart  dwm)          |
| MODKEY + SHIFT + q       | quits dwm                                                     |
| MODKEY + b               | hides the bar                                                 |
| MODKEY + 1-9             | switch focus to workspace (1-9)                               |
| MODKEY + SHIFT + 1-9     | send focused window to workspace (1-9)                        |
| MODKEY + j               | focus stack +1 (switches focus between windows in the stack)  |
| MODKEY + k               | focus stack -1 (switches focus between windows in the stack)  |
| MODKEY + h               | expands size of window                                        |
| MODKEY + l               | shrinks size of window                                        |
| MODKEY + z               | gapps -1  decrease the gap size                               |
| MODKEY + x               | gapps +1  increase the gap size                               |

### Layout Controls

| Keybinding              | Action                    |
| ----------------------- | ------------------------- |
| MODKEY + SHIFT + i      | row layout                |
| MODKEY + i              | column layout             |
| MODKEY + TAB            | cycle layout (-1)         |
| MODKEY + SHIFT + TAB    | cycle layout (+1)         |
| MODKEY + SPACE          | change layout             |
| MODKEY + SHIFT + SPACE  | toggle floating windows   |
| MODKEY + t              | layout 1                  |
| MODKEY + f              | layout 2                  |
| MODKEY + m              | layout 3                  |
| MODKEY + a              | layout 4                  |

### Application Controls

| Keybinding         | Action                                                                         |
| ------------------ | ------------------------------------------------------------------------------ |
| MODKEY + w         | open brave browser                                                             |
| MODKEY + SHIFT + w | open nmtui as root                                                             |
| MODKEY + r         | open ranger file manager                                                       |
| MODKEY + SHIFT + r | open htop system monitor                                                       |
| MODKEY + e         | open neomutt mail client                                                       |
| MODKEY + SHIFT + e | open abook addressbook                                                         |
| MODKEY + n         | open newsboat rss reader                                                       |
| MODKEY + v         | open vimwiki personal note taking wiki                                         |
| MODKEY + SHIFT + m | open ncmpcpp music player                                                      |
| MODKEY + c         | open calcurse personal calendar, organizer                                     |
| MODKEY + SHIFT + c | volume mute to default sink                                                    |
| MODKEY + p         | mpc toggle (mute for mpd)                                                      |


## Xsetroot Scripts for Status

Simple dwm statusbar shows brightness, volume, battery, date and time. Save it in an executable and start before <code>exec dwm</code> in xinitrc.

<pre><code>
#!/bin/sh

# dwm statusbar

#!/usr/bin/env bash
# Ultra-lightweight DWM statusbar for X220 (Nerd Font version)

while true; do
    # --- TIME ---
    TIME=" $(date '+%H:%M %d-%b')"  # Clock

    # --- BATTERY ---
    if [ -f /sys/class/power_supply/BAT0/capacity ]; then
        BAT_CAP=$(cat /sys/class/power_supply/BAT0/capacity)
        BAT_STAT=$(cat /sys/class/power_supply/BAT0/status)
        BAT_TEXT=" $BAT_CAP% $BAT_STAT"  # Battery
    else
        BAT_TEXT="No BAT"
    fi

    # --- CPU LOAD ---
    CPU_LOAD=$(awk '{print $1}' /proc/loadavg)
    CPU_TEXT=" $CPU_LOAD"  # CPU icon

    # --- VOLUME ---
    VOL_RAW=$(amixer get Master | awk -F'[][]' 'END{ print $2 " " $4 }')
    VOL_PERCENT=$(echo $VOL_RAW | awk '{print $1}' | tr -d '%')
    VOL_MUTE=$(echo $VOL_RAW | awk '{print $2}')
    if [ "$VOL_MUTE" = "off" ] || [ "$VOL_PERCENT" = "0" ]; then
        VOL_TEXT=" Mute"  # Muted speaker
    else
        VOL_TEXT=" $VOL_RAW"  # Speaker on
    fi

    # --- Wi-Fi / IP ---
    NET_TEXT=""
    for iface in /sys/class/net/*; do
        iface_name=$(basename "$iface")
        state=$(cat "$iface/operstate")
        if [[ "$iface_name" == w* && "$state" == "up" ]]; then
            IP=$(ip -4 addr show "$iface_name" | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
            NET_TEXT=" $IP"  # Wi-Fi icon
            break
        fi
    done

    # --- Root disk usage ---
    DISK=$(df -h --output=pcent /home | tail -1)
    DISK_TEXT=" $DISK"  # Hard drive icon

    # --- CPU temperature ---
    if [ -f /sys/class/thermal/thermal_zone0/temp ]; then
        TEMP=$(awk '{printf "%.1f", $1/1000}' /sys/class/thermal/thermal_zone0/temp)
        TEMP_TEXT=" $TEMP°C"  # Thermometer
    else
        TEMP_TEXT=""
    fi

    # --- Combine and set status ---
    STATUS="$BAT_TEXT | $CPU_TEXT | $VOL_TEXT | $DISK_TEXT | $TEMP_TEXT | $TIME"

    # DWM bar update
    xsetroot -name "$STATUS"

    # BSPWM Polybar update
    echo "$STATUS" > /tmp/dwm_status.txt

    sleep 5
done

</code></pre>

more will be added...
