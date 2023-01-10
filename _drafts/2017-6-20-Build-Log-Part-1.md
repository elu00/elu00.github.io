---
title: "DIY 10 Channel PWM Controller - Build Log Part 1"
category: Projects
tags: Controller-Build-Log
excerpt: "How to build a (somewhat functional) Arduino firmware"
header:
  overlay_color: #1DE9B6
---
Here's a quick update on the status of the project; most of the relevant code can be found on [the project Github](https://github.com/elu00/PWMduino).
# Firmware and Arduino-PWM oddities
Based on several posts made on the official Arduino forums, the default Arduino PWM implementation isn't completely compliant with the Intel PWM fan spec as a consequence of the lower frequency used by default.
In order to get around this, I ended up using  

- [This PWM fan library](https://google.com)

As already 

# Demo video
