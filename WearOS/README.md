# WearOS

---

* [Disable screen off for WearOS 5](#9feecfea-1a8b-4343-a284-0e85f03b6076)

---




<div id="9feecfea-1a8b-4343-a284-0e85f03b6076">

## Disable screen off for WearOS 5

</div>

Access watch with wearOS5 using ADB, then execute command:

    adb shell settings put global ungaze_sleep_enabled 0

Can also use Tasker+AutoWear to toggle it: https://www.reddit.com/r/tasker/comments/tw6c6k/gw4_autowear_turn_off_ungaze
