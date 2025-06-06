================
ST Nucleo F429ZI
================

.. tags:: chip:stm32, chip:stm32f4, chip:stm32f429

The Nucleo F429ZI is a member of the Nucleo-144 board family.

This page discusses issues unique to NuttX configurations for the ST
Nucleo F429ZI board from ST Micro.  See

  http://www.st.com/en/evaluation-tools/nucleo-f429zi.html

NucleoF429ZI:

- Microprocessor: 32-bit ARM Cortex M4 with 180 MHz max CPU frequency
- Memory: 2 MB Flash and 256+4 KB SRAM including 64-Kbyte of core coupled memory
- ADC: 3x12-bit ADC with 24 channels
- DMA: 16-stream DMA controller with FIFOs and burst support
- Timers: Up to 17 timers: up to 12 16-bit, 2 32-bit timers, two
  watchdog timers, and a SysTick timer
- GPIO: Up to 168 I/O ports with interrupt capability
- I2C: Up to 3 I2C interfaces
- USARTs: Up to 4 USARTs
- SPIs: Up to 6 SPIs (45 Mbit/s)


See:
    https://www.st.com/en/microcontrollers-microprocessors/stm32f429zi.html


Hardware
========

Buttons
-------

B1 USER: the user button is connected to the I/O PC13 (pin 2) of the STM32
microcontroller.

LEDs
----

The Nucleo F410RB provide a single user LED, LD2.  LD2
is the green LED connected to Arduino signal D13 corresponding to MCU I/O
PA5 (pin 21) or PB13 (pin 34) depending on the STM32target.

- When the I/O is HIGH value, the LED is on.
- When the I/O is LOW, the LED is off.

These LEDs are not used by the board port unless CONFIG_ARCH_LEDS is
defined.  In that case, the usage by the board port is defined in
include/board.h and src/sam_leds.c. The LEDs are used to encode OS-related
events as follows when the red LED (PE24) is available::

    SYMBOL                Meaning                   LD2
    -------------------  -----------------------  -----------
    LED_STARTED          NuttX has been started     OFF
    LED_HEAPALLOCATE     Heap has been allocated    OFF
    LED_IRQSENABLED      Interrupts enabled         OFF
    LED_STACKCREATED     Idle stack created         ON
    LED_INIRQ            In an interrupt            No change
    LED_SIGNAL           In a signal handler        No change
    LED_ASSERTION        An assertion failed        No change
    LED_PANIC            The system has crashed     Blinking
    LED_IDLE             MCU is is sleep mode       Not used

Thus if LD2, NuttX has successfully booted and is, apparently, running
normally.  If LD2 is flashing at approximately 2Hz, then a fatal error
has been detected and the system has halted.

Serial Consoles
===============

USART1
------

Pins and Connectors::

    RXD: PA11  CN10 pin 14
         PB7   CN7 pin 21
    TXD: PA10  CN9 pin 3, CN10 pin 33
         PB6   CN5 pin 3, CN10 pin 17

    NOTE:  You may need to edit the include/board.h to select different USART1
    pin selections.

TTL to RS-232 converter connection::

    Nucleo CN10 STM32F429ZI
    ----------- ------------
    Pin 21 PA9  USART1_RX   *Warning you make need to reverse RX/TX on
    Pin 33 PA10 USART1_TX    some RS-232 converters
    Pin 20 GND
    Pin 8  U5V

To configure USART1 as the console::

    CONFIG_STM32_USART1=y
    CONFIG_USART1_SERIALDRIVER=y
    CONFIG_USART1_SERIAL_CONSOLE=y
    CONFIG_USART1_RXBUFSIZE=256
    CONFIG_USART1_TXBUFSIZE=256
    CONFIG_USART1_BAUD=115200
    CONFIG_USART1_BITS=8
    CONFIG_USART1_PARITY=0
    CONFIG_USART1_2STOP=0

USART2
------

Pins and Connectors::

    RXD: PA3   CN9 pin 1 (See SB13, 14, 62, 63). CN10 pin 37
         PD6
    TXD: PA2   CN9 pin 2(See SB13, 14, 62, 63). CN10 pin 35
         PD5

    UART2 is the default in all of these configurations.

TTL to RS-232 converter connection::

    Nucleo CN9  STM32F429ZI
    ----------- ------------
    Pin 1  PA3  USART2_RX   *Warning you make need to reverse RX/TX on
    Pin 2  PA2  USART2_TX    some RS-232 converters

Solder Bridges.  This configuration requires:

- SB62 and SB63 Closed: PA2 and PA3 on STM32 MCU are connected to D1 and D0
  (pin 7 and pin 8) on Arduino connector CN9 and ST Morpho connector CN10
  as USART signals.  Thus SB13 and SB14 should be OFF.

- SB13 and SB14 Open:  PA2 and PA3 on STM32F103C8T6 (ST-LINK MCU) are
  disconnected to PA3 and PA2 on STM32 MCU.

To configure USART2 as the console::

    CONFIG_STM32_USART2=y
    CONFIG_USART2_SERIALDRIVER=y
    CONFIG_USART2_SERIAL_CONSOLE=y
    CONFIG_USART2_RXBUFSIZE=256
    CONFIG_USART2_TXBUFSIZE=256
    CONFIG_USART2_BAUD=115200
    CONFIG_USART2_BITS=8
    CONFIG_USART2_PARITY=0
    CONFIG_USART2_2STOP=0

USART6
------

Pins and Connectors::

    RXD: PC7    CN5 pin2, CN10 pin 19
         PA12   CN10, pin 12
    TXD: PC6    CN10, pin 4
         PA11   CN10, pin 14

To configure USART6 as the console::

    CONFIG_STM32_USART6=y
    CONFIG_USART6_SERIALDRIVER=y
    CONFIG_USART6_SERIAL_CONSOLE=y
    CONFIG_USART6_RXBUFSIZE=256
    CONFIG_USART6_TXBUFSIZE=256
    CONFIG_USART6_BAUD=115200
    CONFIG_USART6_BITS=8
    CONFIG_USART6_PARITY=0
    CONFIG_USART6_2STOP=0

Virtual COM Port
----------------

Yet another option is to use UART2 and the USB virtual COM port.  This
option may be more convenient for long term development, but is painful
to use during board bring-up.

Solder Bridges.  This configuration requires:

- SB62 and SB63 Open: PA2 and PA3 on STM32 MCU are disconnected to D1
  and D0 (pin 7 and pin 8) on Arduino connector CN9 and ST Morpho
  connector CN10.

- SB13 and SB14 Closed:  PA2 and PA3 on STM32F103C8T6 (ST-LINK MCU) are
  connected to PA3 and PA2 on STM32 MCU to have USART communication
  between them. Thus SB61, SB62 and SB63 should be OFF.

Configuring USART2 is the same as given above.

Question:  What BAUD should be configure to interface with the Virtual
COM port?  115200 8N1?

Default:
As shipped, SB62 and SB63 are open and SB13 and SB14 closed, so the
virtual COM port is enabled.

Configurations
==============

trace
---

Configures the trace located at apps/system/trace for the
Nucleo-F429ZI board.  The Configuration enables the serial interfaces
on UART2.  Support for builtin applications is enabled, and selected
application trace.

NOTES:

1. This configuration uses the mconf-based configuration tool.  To
   change this configuration using that tool, you should:

   a. Build and install the kconfig-mconf tool.  See nuttx/README.txt
      see additional README.txt files in the NuttX tools repository.

   b. Execute 'make menuconfig' in nuttx/ in order to start the
      reconfiguration process.
