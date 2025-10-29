---
title: Individal Block Diagram
tags:
- tag1
- tag2
---

## Overview
The subsystem provides user input and output feedback through one button, one LED, and one potentiometer. The button act as digital input, the LED serve as visual indicator, and the potentiometer provides an adjustable analog input for control functions.

All components operate on a regulated 5V, 1.5A supply from an LM7805T voltage regulator, powered by a 9V, 3A unregulated source. The central controller is the Microchip PIC18F57Q43 Curiosity Nano, which manages logic, inputs, and outputs. The potentiometer connects to RA0 via ADC1, the  green LED connect to RA5 using PWM, and the button connect to RC4 as digital inputs.

At the bottom of the diagram, three 8-pin connectors link this subsystem to others within the project. These connectors carry both digital and analog signals for inter-subsystem communication. 

## Workings
Our system follows a hub‑and‑spoke architecture: the hub reads a user‑defined soil‑moisture setpoint from a potentiometer, requests the current moisture value from the sensor subsystem, compares that reading to the setpoint of the potentiometer, and—if watering is required—activates the motor/valve (sprinkler) subsystem and cues the speaker; otherwise, it remains idle.


## Block Diagram 

![Example of Indivial Block diagram ](individual-block-diagram.png)
