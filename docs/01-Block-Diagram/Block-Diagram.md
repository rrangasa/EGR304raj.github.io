---
title: Individal Block Diagram
tags:
- tag1
- tag2
---

## Overview
The subsystem provides user input and output feedback through one button, one LED, one potentiometer, and a light-dependent resistor (LDR). The button acts as a digital input, the LED serves as a visual indicator.

All components operate on a regulated 5V, 1.5A supply from an LM7805CT voltage regulator (Texas Instruments #LM7805CT/NOPB). The central controller is the Microchip PIC18F57Q43 Curiosity Nano, which manages logic, inputs, and outputs. The potentiometer connects to RA3 as an analog input (0-5V), the LDR signal is amplified through an MCP6004 operational amplifier (Microchip Technology #MCP6004-I/P) and connects to RA2 as an analog input (0-5V), the green LED connects to RD4 as a digital output (5V), and the button connects to RB4 as a digital input.

The subsystem interfaces with external modules through three 8-pin connectors. Connector 1 provides digital output (RC7) to a speaker module. Connector 2 interfaces with a motor module, receiving digital input (RD6) and providing digital output (RD7). Connector 3 receives PWM input (RC0) from a humidity sensor module. Each connector includes ground connections for proper signal referencing.




## Workings
Our system follows a hub-and-spoke architecture: the hub first checks whether the ambient sunlight level is appropriate for watering using a light-resistive sensor. When the brightness conditions are suitable, the hub requests the current soil-moisture reading from the sensor subsystem. It then analyzes this data and determines whether watering is required. Based on this decision, the hub sends signals to the motor/valve (sprinkler) subsystem and the speaker to initiate watering, or remains idle if no action is needed.

## Block Diagram Elements and Product Requirements

The following elements of the block diagram contribute to meeting the team's product requirements:

- **Microchip PIC18F57Q43 Curiosity Nano (Central Hub)**: Central hub.

- **LDR with MCP6004 Operational Amplifier (RA2)**: Detects ambient light conditions to avoid inappropriate watering.

- **Potentiometer (RA3)**: Provides simple analog input for testing. May be used to manually set desired moisture by the user in the future.

- **Push Button (RB4)**: Provides simple digital input for testing. May be used for user insteraction in the future.

- **Green LED (RD4)**: Visual status indicator for system state, faults, connectivity, etc.

- **5V 1.5A Voltage Regulator (LM7805CT)**: Ensures reliable power supply, Converts 9 V to 5V.


## Block Diagram 

![Block diagram ](individual-block-diagram.png)

## Resources

The block diagram as a draw.io XML file is available for download [*here*](raj_teambutIndividualblock.drawio.xml).
