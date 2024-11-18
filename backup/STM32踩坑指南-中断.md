## STM32踩坑指南-中断

## 中断的概念

**中断是指CPU正在处理某个事件A，发生了另一件事件B，请求CPU迅速去处理（中断发生）；CPU暂时停止当前的工作（中断响应），转去处理事件B（中断服务）；待CPU处理事件B完成后，再回到原来的事件A（断点）继续执行，这一过程称之为中断。**

## NVIC的基本概念及其作用

NVIC 即嵌套向量中断控制器，全称 Nested vectored interrupt controller，属于是内核的器件。NVIC管理所有的中断，对没错，就是所有中断。

### NVIC常用寄存器

![ 2024-11-18 165934.png](https://s2.loli.net/2024/11/18/bAPNIVponskfLtS.png)

### NVIC的工作原理

![98097e9d76a1e3ba10587d4a2c7792f0.png](https://s2.loli.net/2024/11/18/ycfOqoSHKFGeEkh.png)

> 工作过程：
>
>         当外部被出发时，首先进入ICER、ISER寄存器，用于控制是否开对应的中断，打开的中断进入IPR寄存器，进行中断优先级的判断，IPR寄存器受AIRCR寄存器控制，最后按照中断优先级依次进入CPU被执行。
>         
>         内核中断由SHPR寄存器控制，SHPR与IPR寄存器属于同一级别； 

### NVIC的使用步骤

1.设置中断分组：AIRCR寄存器[10:8]

HAL_NVIC_SetPriorityGrouping();

> 虽然使用的AIRCR有3位，但是我们只取其中5组![ 2024-11-18 165248.png](https://s2.loli.net/2024/11/18/KyZUY4qJMmQa6Oc.png)

2、设置中断优先级：IPR寄存器[7:4]；

HAL_NVIC_SetPriority()；

3、使能中断：ISER寄存器；

HAL_NVIC_EnableIRQ();

4.根据中断向量表设置中断服务函数

总结：
> 所有中断大都大差不差，都需要设置**NVIC,及中断服务函数**