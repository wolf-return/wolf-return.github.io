# STM32中断
### 设置NVIC
![1.png](https://s2.loli.net/2024/12/01/JWbogsi1Ol38j4u.png)
> 如上图所见，我们要设置NVIC的优先级分组，中断通道，中断使能，分配优先级等级
> 以上大部分参数没啥，值得注意的是优先级分组大都选择NVIC_PriorityGroup_4
### 初始化管脚
![2.png](https://s2.loli.net/2024/12/01/NZS5PfIoHCamtFp.png)
> 这一步骤与正常初始化没啥大区别，只记得一提的是，AFIO时钟，因为是管脚复用，所以需要开启AFIO时钟
### 初始化中断
![3.png](https://s2.loli.net/2024/12/01/e1GaOJt7g56LTC9.png)
> 在这一步骤中我们要设置中断通道，（当然这一参数与设置NVIC的参数不是同一个），中断使能，中断模式，触发中断
> 值得一提的是我们还要设置中断源，不过GPIO_EXTILineConfig(GPIO_PortSourceGPIOA,GPIO_PinSource0);该函数的参数，
> 与传统的命名不同需要注意（因为我就在栽过跟头）