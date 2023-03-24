# Eorus dwm Build - dynamic window manager


## Patches I've Used

* dwm-alpha
* dwm-fullgaps
* dwm-statuscolors

## Xsetroot Scripts for Status

Simple dwm statusbar shows brightness, volume, battery, date and time. Save it in an executable and start before <code>exec dwm</code> in xinitrc.

<pre><code>
#!/bin/sh

# dwm statusbar - show current date and time
status () {

	echo -n "BAT: $(acpi -b | awk '{ split($5,a,":"); print substr($3,0,2), $4, "["a[1]":"a[2]"]" }' | tr -d ',') | $(date '+%Y-%m-%d %H:%M:%S')"
}

while :; do

xsetroot -name "🞼 $(expr $(cat /sys/class/backlight/intel_backlight/brightness) / 49) 奄 $(pactl list sinks | grep '^[[:space:]]Volume:' | head -n $(( $SINK + 1 )) | tail -n 1 | sed -e 's,.* \([0-9][0-9]*\)%.*,\1,')% $(status)"

	sleep 1

done &
</code></pre>
