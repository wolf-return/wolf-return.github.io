## STM32踩坑指南二

> 这个是真的坑。一点假都没有

> ![ 2024-11-21 215448.png](https://s2.loli.net/2024/11/21/TRJfliHsmkYdzbG.png)

> 很明显，图中我注释掉的就是出错的部分，vTaskDelay()与xQueueReceive(TEST_Queue,&buffer,porMAX_DELAY)有冲突，在接收一次消息队列后，就会产生很长很长时间的阻塞，至少我等了很久 都没结束阻塞，姑且就认为是永远阻塞吧！！！至于为什么出现这种情况，我认为可能与xQueueReceive()中等待时间有关吧！！！

不能想了，在想就要长脑子了！！！！！！！！！