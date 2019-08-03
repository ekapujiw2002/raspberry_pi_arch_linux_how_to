# Alsa Preamp Sound

```
[enco1@qpi2 run]$ cat /etc/asound.conf
# THIS IS THE DEFAULT
# Use PulseAudio by default
#pcm.!default {
#  type pulse
#  fallback "sysdefault"
#  hint {
#    show on
#    description "Default ALSA Output (currently PulseAudio Sound Server)"
#  }
#}

#ctl.!default {
#  type pulse
#  fallback "sysdefault"
#}

# SET OTHER CARD AS DEFAULT DEVICE
#pcm.!default {
#    type hw
#    card 0
#}

ctl.!default {
    type hw
    card 0
}

# PREAMP
pcm.softvol {
    type            softvol
    slave {
        pcm         "sysdefault:CARD=ALSA"
    }
    control {
        name        "SoftMaster"
        card        0
    }
    max_dB 35.0
    min_dB -5.0
    resolution 250
}

pcm.!default {
    type            plug
#    type            hw
    slave.pcm       "softvol"
}

#MIXED SOUND
#pcm.dmixed {
#       type dmix
#       ipc_key 1024
#       ipc_key_add_uid 0
#       slave.pcm "hw:1,0"
#}

#pcm.duplex {
#       type asym
#       playback.pcm "dmixed"
#}

# Instruct ALSA to use pcm.duplex as the default device
#pcm.!default {
#       type plug
#       slave.pcm "duplex"
#}

#ctl.!default {
#       type hw
#       card 1
#}

# alsa equal
ctl.equal {
  type equal;
  controls "/tmp/.alsaequal.bin";
}

pcm.plugequal {
  type equal;
  # Modify the line below if you don't
  # want to use sound card 0.
  slave.pcm "plughw:0,0";
  # or if you want to use with multiple applications output to dmix
  # slave.pcm "plug:dmix"
}

pcm.equal {
  # Or if you want the equalizer to be your
  # default soundcard uncomment the following
  # line and comment the above line.
#pcm.!default {
  type plug;
  controls "/tmp/.alsaequal.bin";
  slave.pcm plugequal;
}

# vim:set ft=alsaconf:
```

Reference :
- https://alsa.opensrc.org/How_to_use_softvol_to_control_the_master_volume
- https://milkteafuzz.com/j/2013/03/16/increasing-maximum-volume-with-alsa
- https://alien.slackbook.org/blog/adding-an-alsa-software-pre-amp-to-fix-low-sound-levels/
