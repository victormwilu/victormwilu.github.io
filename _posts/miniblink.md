--- 
title: MINIBLINK
date: 2024-11-07
categories: [Embedded Systems Programming, STM32]
tags: [stm32, arm]
toc: false
---

This is a project on how I flashed the STM32f103c8t6 and programmed the microcontroller to make the LED connected to PC13 to blink. It is a basic program that introduces beginners to the world of embedded system programming.

The source code is as shown.

```cpp
#include <libopencm3/stm32/gpio.h>
#include <libopencm3/stm32/rcc.h>

static void;
gpio_setup(void) {
  // enable the GPIOC clock
  rcc_periph_clock_enable(RCC_GPIOC);

  gpio_set_mode(GPIOC, GPIO_MODE_OUTPUT_2_MHZ, GPIO_CNF_OUTPUT_PUSHPULL, GPIO13);
}

int main(void) {
  gpio_setup();
  
  //switching the LED on and off

  for (;;) {
    gpio_clear(GPIOC, GPIO13);
    for (int i = 0; i < 1500000; i++) {
      __asm__("nop");
    }

    gpio_set(GPIOC, GPIO13);
    for (int i = 0; i < 500000; i++) {
      __asm__("nop");
    }
  }

  return 0;
}
```

## libraries
```cpp
#include <libopencm3/stm32/gpio.h>
#include <libopencm3/stm32/rcc.h>
```
```
```
These two libraries are included which provide functions to control the GPIO (General-Purpose Input/Output) pins and RCC (Reset and Clock Control) on the microcontroller.


## GPIO setup configuration

```cpp
gpio_setup(void) {
  // enable the GPIOC clock
  rcc_periph_clock_enable(RCC_GPIOC);

  gpio_set_mode(GPIOC, GPIO_MODE_OUTPUT_2_MHZ, GPIO_CNF_OUTPUT_PUSHPULL, GPIO13);
}
```

This function is used to setup the GPIO. 
`rcc_periph_clock_enable(RCC_GPIOC);` enables the clock for GPIO port C so that it can be used by the program.
`gpio_set_mode(GPIOC, GPIO_MODE_OUTPUT_2_MHZ, GPIO_CNF_OUTPUT_PUSHPULLe, GPIO13);` sets the mode of pin 13 on port C which is on the blue pill board to output mode with a frequency of 2MHZ and a push-pull configuration.


## switching the LED on and off

```cpp
for (;;) {
    gpio_clear(GPIOC, GPIO13);
    for (int i = 0; i < 1500000; i++) {
      __asm__("nop");
    }

    gpio_set(GPIOC, GPIO13);
    for (int i = 0; i < 500000; i++) {
      __asm__("nop");
    }
  }
```

`for (;;)` is an infinite loop, meaning it will run until the microcontroller is powered off. 
The `gpio_clear` function turns on the LED while the `gpio_set` turns off the LED since the LED is in an active low configuration.
`__asm__("nop");` is a busy-wait loop that creates a delay. When used for both the on and off part it creates delays to control the on and off timing of the LED.


Below is an illustration of the miniblink program in action.
{% include embed/video.html src='{/home/neutron/Videos/projects/miniblink.mp4}' %}

