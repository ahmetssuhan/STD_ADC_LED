# STM32F4 Discovery - ADC Controlled LED Indicator

This project demonstrates the use of the STM32F407G-DISC1 microcontroller board to read analog input via ADC and control LEDs based on the converted digital value. A potentiometer is connected to pin `PA1`, and the onboard LEDs (`PD12` to `PD15`) indicate the current voltage level.

## Overview

The application performs the following tasks:
- Initializes GPIO for LEDs (output) and analog input (PA1).
- Configures ADC1 in independent mode with 8-bit resolution.
- Reads the analog voltage on PA1 using software-triggered conversion.
- Lights up different LEDs depending on the ADC value.

## Hardware Connections

| Component       | Pin         | Description             |
|----------------|-------------|-------------------------|
| Potentiometer   | PA1 (ADC1)  | Analog input voltage    |
| LED Green       | PD12        | Indicates ADC range     |
| LED Orange      | PD13        | Indicates ADC range     |
| LED Red         | PD14        | Indicates ADC range     |
| LED Blue        | PD15        | Indicates ADC range     |

## ADC Value to LED Mapping

| ADC Value Range | Active LED(s)      |
|-----------------|--------------------|
| 0–49            | PD12               |
| 50–99           | PD13               |
| 100–149         | PD14               |
| 150–199         | PD15               |
| 200–255         | PD12, PD13, PD14, PD15 |

## Configuration Summary

- **ADC Resolution**: 8 bits
- **ADC Channel**: Channel 1 (PA1)
- **ADC Mode**: Independent
- **Prescaler**: Divided by 4
- **Sampling Time**: 56 Cycles
- **LEDs GPIO Port**: GPIOD
- **Analog GPIO Port**: GPIOA

## File Structure
project/
├── main.c # Main application source
├── README.md # Project description
├── stm32f4xx.h # CMSIS headers
├── stm32f4_discovery.h # Board support headers


## How to Use

1. Connect a potentiometer output to pin PA1.
2. Load the project in STM32CubeIDE or Keil uVision.
3. Compile and flash the firmware to STM32F407G-DISC1.
4. Vary the potentiometer to change the input voltage.
5. Observe LED behavior in accordance with ADC value.

## Dependencies

- STM32 Standard Peripheral Library
- STM32F407G-DISC1 board
- ST-Link programmer/debugger

## License

This project is distributed for educational purposes. You may modify and use it freely for personal or academic use.



