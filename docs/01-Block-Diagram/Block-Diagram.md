---
title: Individal Block Diagram
tags:
- tag1
- tag2
---

## Overview
The subsystem provides user input and output feedback through one button, one LED, one potentiometer, and a light-dependent resistor (LDR). The button acts as a digital input, the LED serves as a visual indicator, the potentiometer provides an adjustable analog input for control functions, and the LDR enables light sensing capabilities.

All components operate on a regulated 5V, 1.5A supply from an LM7805CT voltage regulator (Texas Instruments #LM7805CT/NOPB). The central controller is the Microchip PIC18F57Q43 Curiosity Nano, which manages logic, inputs, and outputs. The potentiometer connects to RA3 as an analog input (0-5V), the LDR signal is amplified through an MCP6004 operational amplifier (Microchip Technology #MCP6004-I/P) and connects to RA2 as an analog input (0-5V), the green LED connects to RD4 as a digital output (5V), and the button connects to RB4 as a digital input.

The subsystem interfaces with external modules through three 8-pin connectors. Connector 1 provides digital output (RC7) to a speaker module. Connector 2 interfaces with a motor module, receiving digital input (RD6) and providing digital output (RD7). Connector 3 receives PWM input (RC0) from a humidity sensor module. Each connector includes ground connections for proper signal referencing.




## Workings
Our system follows a hub‑and‑spoke architecture: the hub reads a user‑defined soil‑moisture setpoint from a potentiometer, requests the current moisture value from the sensor subsystem, compares that reading to the setpoint of the potentiometer, and—if watering is required—activates the motor/valve (sprinkler) subsystem and cues the speaker; otherwise, it remains idle.


## Block Diagram 

![Block diagram ](individual-block-diagram.png)
