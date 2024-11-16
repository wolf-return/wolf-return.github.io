# STM32踩坑指南一

STM32开发相关的库有很多，它们都是为了方便开发者使用STM32微控制器而提供的软件工具。根据不同的功能和层次，可以将它们分为以下几类：

CMSIS库（准确来说是CMSIS pack）（Cortex Microcontroller Software Interface Standard）是ARM公司推出的一种标准化的微控制器软件接口，它定义了一些通用的数据类型、寄存器访问、中断处理、内核功能等，方便开发者使用Cortex-M内核的各种功能。CMSIS库还包括了一些中间件组件，如RTOS、DSP、Driver、Pack、SVD、DAP和NN等，提供了丰富的软件功能。CMSIS库不是HAL库，也不是标准库，它是一种与厂商（比如ST公司）无关的软件层，可以在不同的微控制器上使用。

```c
#include "stm32f4xx.h"

int main(void) {
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOBEN;  // 使能GPIOB时钟
    GPIOB->MODER |= GPIO_MODER_MODER0_0;  // 设置PB0为输出模式
    
    while(1) {
        GPIOB->ODR |= GPIO_ODR_OD0;  // 点亮LED
    }
}

```

> CMSIS 的作用则是将寄存器进行宏定义，例如上文:RCC->AHB1ENR ,GPIOB->MODER,GPIOB->ODR诸如此类等等

HAL库（Hardware Abstraction Layer，硬件抽象层）是ST公司推出的一种硬件抽象层库，它提供了一套统一、简洁、易用的API函数接口，方便开发者使用STM32的各种外设功能。HAL库支持STM32全系列产品，具有可移植性、易用性和可靠性等优点。HAL库还提供了一些中间件组件，如RTOS，USB，TCP/IP和图形等，可以快速实现复杂的功能。

```c
#include "stm32f4xx_hal.h"

GPIO_InitTypeDef GPIO_InitStructure;

int main(void) {
    __HAL_RCC_GPIOB_CLK_ENABLE();

    GPIO_InitStructure.Pin = GPIO_PIN_0;
    GPIO_InitStructure.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStructure.Pull = GPIO_NOPULL;
    GPIO_InitStructure.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOB, &GPIO_InitStructure);

    while(1) {
        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GPIO_PIN_SET);  // 点亮LED
    }
}
```

> HAL库，标准库的变种，在标准库的基础上大量判断，再加上STM32CUBEIDE自动化生成代码，使得开发更加便利

标准库（Standard Peripheral Libraries）是ST公司为STM32微控制器提供的一种固件函数包，它封装了STM32所有外设的寄存器操作和中断处理，提供了一套统一、简洁、易用的API函数接口，方便开发者使用STM32的各种外设功能。标准库支持STM32全系列产品，具有可移植性、易用性和可靠性等优点。不过，ST官方已经不再更新STM32标准固件库，而是力推新的固件库:HAL库。

```c
#include "stm32f4xx_gpio.h"
#include "stm32f4xx_rcc.h"

int main(void) {
    GPIO_InitTypeDef GPIO_InitStructure;

    RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOB, ENABLE);

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_OUT;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_InitStructure);

    while(1) {
        GPIO_SetBits(GPIOB, GPIO_Pin_0);  // 点亮LED
    }
}

```

> 所谓SPL库(标准库)就是在CMSIS的基础上，对一些常用的函数进行封装，在开发程序时，直接调用，这就极大的方便了用户

LL库（Low-Layer，底层）是ST公司最近（也不是最近，六七年了）新增的一种底层库，它与HAL库捆绑发布，文档也是和HAL文档在一起的。 LL库更接近硬件层，对需要复杂上层协议栈的外设不适用，直接操作寄存器。LL库可以独立使用，也可以和HAL库结合使用。

其他第三方或开源库：除了ST公司提供的官方库外，还有许多第三方或开源的软件库可以用于STM32开发，如FreeRTOS、uCOS、FatFs、LwIP等。这些软件库通常提供了一些特定领域或功能的解决方案，如实时操作系统、文件系统、网络协议等。

> STM32的LL库（（Low Layer））是STM32Cube库的一部分，旨在为STM32外设提供一个简化的API集，同时保持最大的灵活性。LL库是一个**更接近硬件**的编程接口，与HAL（硬件抽象层）库相比，它提供了更低的抽象级别。

总结：

**使用什么开发库不重要，重要的是了解内部的运行原理才重要**

