---
title: "DIY 10 Channel PWM Controller - Build Log Part 0"
category: Projects
tags: Controller-Build-Log
excerpt: "Thoughts, plans, and hardware for a 10-channel Arduino-based PWM Controller"
header:
  overlay_color: #1DE9B6
---
## Motivation
Recently within the PC building space, the majority of fan vendors have come to support [PWM Control](https://knowledge.ni.com/KnowledgeArticleDetails?id=kA00Z0000019OkFSAU), a standard used for fan and temperature management.
Compared to DC control, PWM control is nice to have because it tends to provide a larger effective operating range and smoother speed adjustments for the fan itself, which helps a lot for management of acoustics. 
It's also particularly useful for anyone interested in watercooling, as most speed-controllable pumps exclusively use PWM for speed control.

However, as-is, many [motherboards fail to fully support the "true" PWM standard](http://www.overclock.net/t/1462770/4-pin-fan-headers-on-motherboard-that-arent-pwm) and dedicated PWM controllers like the [Aquaero 6 LT](http://www.performance-pcs.com/aquacomputer-faceplate-for-aquaero-5-and-6-pro-aluminium-black.html) tend to be fairly expensive, so over the next couple months I'll be attempting to design and build my own alternative to such controllers.  
My hopeful end goal will be to package all the circuitry and necessary electronics inside a 3D printed or CNC'd 5.25" enclosure, with firmware to support controlling 9-10 fans simultaneously.  
I'll be using this build log as a way to document the various steps/difficulties I run into along the way, and hopefully someone else will find it helpful :)

Currently, preliminary plans are as follows:

## Hardware
Microcontroller: [Arduino Mega 2560](https://www.arduino.cc/en/Main/ArduinoBoardMega2560)   
Fan(s): [Noctua NF-F12](http://noctua.at/en/nf-f12-pwm/specification)  
Other misc. electronic components (resistors, solderless breadboard extension cables, etc?)  
And here's the (hopefully official) [Intel 4-Pin PWM Fan Specification](http://www.formfactors.org/developer/specs/4_wire_pwm_spec.pdf)  
Notably, I'll be using an Arduino Mega over the other Arduino products due to it exposing 15+ PWM outputs rather than the 6 or fewer available on the other boards.

## Overall Plans
Moving forward, I'll be looking to complete this project as follows:
- Build firmware, circuitry, etc. for functioning one-fan controller
- Extend software above support multiple (i.e. 10) fan channels
- Build desktop client for serial communications, test with circuit
- Mill PCB at UNR, CNC enclosure, mount finished product