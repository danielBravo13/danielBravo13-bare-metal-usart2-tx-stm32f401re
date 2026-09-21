# bare-metal-usart2-tx-stm32f401re
Bare-metal UART transmission on the STM32F401RE using USART2, with direct register configuration, GPIO alternate functions, clock/baud-rate calculations, and 8N1 serial communication.

## Overview

This project implements UART transmission on the STM32F401RE without using STM32 HAL or LL drivers.

The USART2 peripheral is configured directly through memory-mapped registers, providing a hands-on implementation of the hardware and clock configuration required for serial communication.

## Features
- Bare-metal STM32 peripheral configuration
- USART2 TX implementation
- GPIO alternate-function configuration
- APB1 / PCLK1 clock calculation
- Baud-rate register (BRR) configuration
- 8N1 UART framing
- Polling-based transmission using the TXE status flag
- Character and string transmission

## UART Configuration 
- MCU: STM32F401RE
- Peripheral: USART2
- TX Pin: PA2
- Alternate Function: AF7
- Baud Rate: 115200
- Data Bits: 8
- Parity: None
- Stop Bits: 1
- Flow Control:  None

## Clock and Baud Rate Configuration
USART2 is connected to the APB1 bus, so the USART clock is derived from **PCLK1**

SYSCLK

   ↓
  
AHB Prescaler

   ↓
  
HCLK

   ↓
  
APB Prescaler

   ↓
  
PCLK1

   ↓
  
USART2

The firmware reads the **PPRE1** field from **RCC->CFGR** to determine the APB1 prescaler and calculate the clock available to USART2.
The resulting clock is then used to calculate the USART baud-rate register value.

## GPIO Configuration 
USART2 TX is routed to PA2 through the GPIO alternate-function multiplexer.

PA2

 ↓
 
Alternate Function Mode

 ↓
 
AF7

 ↓
 
USART2_TX

The GPIO configuration is performed directly through the GPIO registers:

GPIOA->MODER

GPIOA->AFR[0]

GPIOA->OTYPER

GPIOA->OSPEEDR

GPIOA->PUPDR

## USART Configuration
USART2 is enabled through the APB1 peripheral clock register and configured through its control and baud-rate registers.

USART2->CR1 |= USART_CR1_TE;

USART2->CR1 |= USART_CR1_UE;

The transmitter is enabled with **TE**, and the USART peripheral itself is enabled with **UE**.

## Data Transmission
The transmitter waits for the **TXE** flag before writing the next byte to the USART data register.

while (!(USART2->SR & USART_SR_TXE))

Strings are transmitted one character at a time:

UART2_SendChar(*s++);

This project was built to understand the relationship between the STM32 clock system, peripheral registers, and UART communication.

## Key Concepts

The main concepts explored were:

- Memory-mapped peripheral registers
- RCC clock configuration
- AHB and APB buses
- APB1 prescaling
- GPIO alternate-function multiplexing
- Baud-rate generation
- UART framing
- Least-significant-bit-first transmission
- Status-flag polling
- Embedded C
