# Eorus dwm Build - dynamic window manager

For Thinkpad X220 I've tried to keep dwm as minimal as I can to use screen real estate and keyboard actions.

## Patches I've Used

* [dwm-alpha](https://dwm.suckless.org/patches/alpha/)
* [dwm-fibonacci](https://dwm.suckless.org/patches/fibonacci/)
* [dwm-fullgaps](https://dwm.suckless.org/patches/fullgaps/)
* [dwm-statuscolors](https://dwm.suckless.org/patches/statuscolors/)
* XF86Keys <code>#include <X11/XF86keysym.h></code>

## Font

<code>static const char *fonts[]          = { "Source Code Pro:size=12:style=Bold:antialias=true:autohint=false" };</code>
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
status () {

	echo -n "BAT: $(acpi -b | awk '{ split($5,a,":"); print substr($3,0,2), $4, "["a[1]":"a[2]"]" }' | tr -d ',') | $(date '+%Y-%m-%d %H:%M:%S')"
}

while :; do

xsetroot -name "🞼 $(expr $(cat /sys/class/backlight/intel_backlight/brightness) / 49) 奄 $(pactl list sinks | grep '^[[:space:]]Volume:' | head -n $(( $SINK + 1 )) | tail -n 1 | sed -e 's,.* \([0-9][0-9]*\)%.*,\1,')% $(status)"

	sleep 1

done &
</code></pre>

more will be added...
