---
title: Microcontroller Code
---

## Overview

This section documents the microcontroller firmware for the central hub subsystem. The code is written in C and compiled using MPLAB X IDE for the Microchip PIC18F57Q43 Curiosity Nano microcontroller. The firmware implements the hub-and-spoke architecture, managing user inputs (potentiometer, LDR, button), processing sensor data, and controlling outputs (LED, speaker, motor) based on the system logic.

The code utilizes various peripherals including:
- **ADCC**: For analog inputs from potentiometer (RA3) and LDR (RA2 via MCP6004 amplifier)
- **GPIO**: For digital I/O including button input (RB4), LED output (RD4), and motor/speaker control
- **PWM/CCP**: For motor and speaker control
- **EUSART/UART**: For inter-board communication

## Main Code

~~~c
//Ragul Raj
// Water Only when PWM Duty Cycle is above 175

#include "mcc_generated_files/system/system.h"
#include "mcc_generated_files/capture/ccp1.h"
#include "mcc_generated_files/adc/adc.h"
#include <xc.h>
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>

#ifndef _XTAL_FREQ
#define _XTAL_FREQ 16000000UL   // match your MCC System Clock
#endif

// Redirect printf() to UART1
void putch(char data)
{
    while (!UART1_IsTxReady());   // wait until UART ready
    UART1_Write((uint8_t)data);   // send one byte
}

/* Global variables */
volatile uint16_t rise_time = 0;
volatile uint16_t last_rise_time = 0;
volatile uint16_t pulse_width_ticks = 0;  // Raw Timer1 tick value (HIGH time)
volatile uint16_t period_ticks = 0;       // Raw Timer1 tick value (period)
volatile bool new_pulse = false;
volatile bool capture_active = false;  // Flag to control capture

/* CCP1 Callback - captures rising and falling edges to measure pulse width and period */
void CCP1_CallBack(uint16_t capturedValue)
{
    if (!capture_active) return;  // Only capture when function is active
    
    uint8_t mode = CCP1CON & 0x0F;
    
    if (mode == 0b0101)                     // Rising edge just happened
    {
        // Calculate period (time between this rising edge and previous rising edge)
        if (last_rise_time != 0)
        {
            uint16_t period_diff;
            if (capturedValue >= last_rise_time)
                period_diff = capturedValue - last_rise_time;
            else
                period_diff = (0xFFFFU - last_rise_time) + capturedValue + 1;
            
            period_ticks = period_diff;
        }
        
        last_rise_time = capturedValue;
        rise_time = capturedValue;
        CCP1CON = (CCP1CON & 0xF0) | 0b0100; // Next: capture falling edge
    }
    else if (mode == 0b0100)                // Falling edge just happened
    {
        uint16_t diff;
        if (capturedValue >= rise_time)
            diff = capturedValue - rise_time;
        else
            diff = (0xFFFFU - rise_time) + capturedValue + 1;
        
        // Store raw Timer1 tick value (pulse width)
        pulse_width_ticks = diff;
        new_pulse = true;
        
        CCP1CON = (CCP1CON & 0xF0) | 0b0101; // Re-arm for next rising edge
    }
}

/* -----------------------
   Initialize PWM capture hardware
   - Sets up Timer1 and CCP1 for PWM capture
   - Should be called once at startup
   ----------------------- */
void PWM_Capture_Init(void)
{
    // Force Timer1 to 16 MHz (Fosc/4) - required for correct timing
    T1CLKbits.CS = 0b00010;   // Timer1 clock = Fosc/4
    T1CONbits.CKPS = 0b00;    // 1:1 prescaler
    T1CONbits.RD16 = 1;
    T1CONbits.ON = 1;

    // Force CCP1 into "every rising edge" mode (overrides MCC setting)
    CCP1CON = 0x85;           // 0b10000101 = enabled + every rising edge

    // Register our callback
    CCP1_SetCallBack(CCP1_CallBack);

    // CORRECT interrupt enables for current MCC (2024-2025)
    INTCON0bits.GIEH = 1;     // Global Interrupt High Enable
    INTCON0bits.GIEL = 1;     // Global Interrupt Low Enable
    
    // Reset state
    rise_time = 0;
    last_rise_time = 0;
    pulse_width_ticks = 0;
    period_ticks = 0;
    new_pulse = false;
    capture_active = false;
}

/* -----------------------
   Read PWM pulse width
   - Waits for a PWM pulse and returns raw Timer1 tick value
   - Returns 0xFFFF if timeout occurs
   - timeout_ms: Maximum time to wait in milliseconds (0 = wait forever)
   ----------------------- */
uint16_t PWM_Read(uint16_t timeout_ms)
{
    uint16_t result = 0xFFFF;  // Error/Timeout value
    uint16_t timeout_count = 0;
    
    // Enable capture
    capture_active = true;
    new_pulse = false;
    
    // Wait for a pulse
    if (timeout_ms == 0)
    {
        // Wait forever
        while (!new_pulse)
        {
            __delay_ms(1);
        }
    }
    else
    {
        // Wait with timeout
        while (!new_pulse && timeout_count < timeout_ms)
        {
            __delay_ms(1);
            timeout_count++;
        }
    }
    
    // If we got a pulse, return the value
    if (new_pulse)
    {
        result = pulse_width_ticks;
        new_pulse = false;
    }
    
    // Disable capture until next call
    capture_active = false;
    
    return result;
}

/* -----------------------
   Read multiple PWM pulses and return average duty cycle (0-255)
   - Collects num_samples pulses and returns average duty cycle
   - timeout_ms: Maximum time to wait for all samples
   - Returns 0xFFFF if timeout or no samples collected
   ----------------------- */
uint16_t PWM_Read_DutyCycle(uint16_t num_samples, uint16_t timeout_ms)
{
    uint32_t sum_pulse_width = 0;
    uint32_t sum_period = 0;
    uint16_t count = 0;
    uint16_t timeout_count = 0;
    
    // Reset last_rise_time to start fresh period measurement
    last_rise_time = 0;
    period_ticks = 0;
    
    // Enable capture
    capture_active = true;
    
    // Collect samples
    while (count < num_samples && timeout_count < timeout_ms)
    {
        if (new_pulse)
        {
            // Only use sample if we have both pulse width and period
            if (period_ticks > 0 && pulse_width_ticks > 0)
            {
                sum_pulse_width += pulse_width_ticks;
                sum_period += period_ticks;
                count++;
            }
            new_pulse = false;
        }
        __delay_ms(1);
        timeout_count++;
    }
    
    // Disable capture until next call
    capture_active = false;
    
    if (count == 0 || sum_period == 0)
        return 0xFFFF;  // No samples collected or invalid period
    
    // Calculate average duty cycle: (pulse_width / period) * 255
    uint32_t avg_pulse_width = sum_pulse_width / count;
    uint32_t avg_period = sum_period / count;
    
    // Duty cycle = (pulse_width / period) * 255
    // Use 32-bit math to avoid overflow
    uint32_t duty_cycle = ((uint32_t)avg_pulse_width * 255UL) / avg_period;
    
    // Clamp to 0-255 range
    if (duty_cycle > 255)
        duty_cycle = 255;
    
    return (uint16_t)duty_cycle;
}

void main(void)
{
    SYSTEM_Initialize();      // MCC initializes everything

    Sensor_State_SetDigitalOutput();
    Sensor_State_SetLow();   // Start with sensor disabled
    Motor_Out_SetDigitalOutput();
    Motor_Out_SetLow();      // Start with motor off
    LED_SetDigitalOutput();
    LED_SetLow();    // Start with LED off
    Speaker_Out_SetDigitalOutput();
    Speaker_Out_SetLow();    // Start with speaker off
    BTN_SetDigitalInput();
    // BTN has pull-down resistor, so no pull-up needed
    
    // Initialize PWM capture hardware
    PWM_Capture_Init();

    // Initialize ADC (ensure it's ready)
    ADC_Initialize();
    
    printf("\033[2J\033[HPIC18F57Q43 PWM Capture Ready (RC0)\r\n");
    printf("Timer1: 16MHz (FOSC/4, 1:1 prescaler)\r\n");
    printf("Press BTN to check LDR and read PWM\r\n\r\n");

    while(1)
    {
        if (BTN_GetValue() == 1)   // Button pressed (goes HIGH when pressed)
        {
            __delay_ms(30);        // Debounce
            if (BTN_GetValue() == 1)
            {
                // Read LDR ADC value
                uint16_t ldr_value = ADC_ChannelSelectAndConvert(LDR);
                
                printf("LDR ADC: %u\r\n", ldr_value);
                
                // Check if LDR value is above threshold
                if (ldr_value > 2000)
                {
                    // Dark enough - enable sensor and read PWM
                    Sensor_State_SetHigh();
                    __delay_ms(100);   // Give sensor time to stabilize
                    
                    // Read duty cycle from 50 samples
                    uint16_t duty_cycle = PWM_Read_DutyCycle(50, 5000); // 50 samples, 5 second timeout
                    
                    // Disable sensor
                    Sensor_State_SetLow();
                    
                    // Check duty cycle and act accordingly
                    if (duty_cycle != 0xFFFF)
                    {
                        printf("Duty cycle: %u (0-255)\r\n", duty_cycle);
                        
                        // Check soil moisture level
                        if (duty_cycle < 175)
                        {
                            // Soil is sufficiently moist
                            printf("Soil is Sufficiently Moistured\r\n");
                        }
                        else
                        {
                            // Soil needs water - activate motor and speaker
                            printf("Watering...\r\n");
                            
                            // Set motor and speaker outputs HIGH
                            Motor_Out_SetHigh();
                            Speaker_Out_SetHigh();
                            LED_SetHigh();
                            
                            // Wait 1 second (1000ms)
                            __delay_ms(1000);
                            
                            // Set motor and speaker outputs LOW
                            Motor_Out_SetLow();
                            Speaker_Out_SetLow();
                            LED_SetLow();
                            printf("Watering complete\r\n");
                        }
                    }
                    else
                    {
                        printf("Timeout - no PWM detected\r\n");
                    }
                }
                else
                {
                    // Too bright - don't water
                    printf("It is too bright to water\r\n");
                }

                while (BTN_GetValue() == 1) { __delay_ms(20); }  // wait for release
                __delay_ms(50);   // release debounce
            }
        }
        
        __delay_ms(10);  // Small delay in main loop
    }
}
~~~

## Resources

The complete MPLAB X project files are available for download:

- **MPLAB X Project Zip**: [*Download here*](https://github.com/rrangasa/EGR304raj.github.io/raw/refs/heads/main/docs/07-Microcontroller-Code/FinalTest1.zip)
