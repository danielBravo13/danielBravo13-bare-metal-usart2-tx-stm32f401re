#bare-metal-usart2-tx-stm32f401re
Bare-metal UART transmission on the STM32F401RE using USART2, with direct register configuration, GPIO alternate functions, clock/baud-rate calculations, and 8N1 serial communication.

Overview

This project implements UART transmission on the STM32F401RE without using STM32 HAL or LL drivers.

The USART2 peripheral is configured directly through memory-mapped registers, providing a hands-on implementation of the hardware and clock configuration required for serial communication.

Features
Bare-metal STM32 peripheral configuration
USART2 TX implementation
GPIO alternate-function configuration
APB1 / PCLK1 clock calculation
Baud-rate register (BRR) configuration
8N1 UART framing
Polling-based transmission using the TXE status flag
Character and string transmission
