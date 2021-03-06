# Speaker and Mic Setup
Author: Ramyad
Date: 7/25/2019

- List All devices: `cat /proc/asound/cards`

- List all Playback Devices `aplay -l`  
  See your card and device number  
  Note: you need to set volume `alsamixer -c <card_number>`  
  Note: you can also use `pacmd set-source-volume <index> <volume>`

- List all Recording Devices `arecord -l` or `pacmd list-sources`  
    See your card and device number

- Control devices with `alsamixer`, use F6 to select your device:wq

- Set your Recording and Playback Device as the Default PCM Devices, in `/etc/asound.conf`  

```
pcm.!default {
  type asym
  capture.pcm "mic"
  playback.pcm "speaker"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:<card number>,<device number>"
  }
}
pcm.speaker {
  type plug
  slave {
    pcm "hw:<card number>,<device number>"
  }
}
```

- Live Streaming  

```BASH
arecord --format=S16_LE --rate=16k -D sysdefault:CARD=1 | aplay --format=S16_LE --rate=16000
```

- Testing Playback `speaker-test -t wav -c 2`

- Test Recording Device  

```
arecord --format=S16_LE --duration=5 --rate=16000 --file-type=raw out.raw
aplay --format=S16_LE --rate=16000 out.raw
```

- Speaker and Mic Volume Control `alsamixer -c "card number"`
