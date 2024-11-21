## STM32踩坑指南二

> 这个是真的坑。一点假都没有

在FreeRTOS中消息队列是一种常用的设置，但是有以下注意

> 1  xQueueSend(TEST_Queue,&send_data,1); 和  xQueueReceive(TEST_Queue,&buffer,portMAX_DELAY);  只能发送和接受一条信息！！！！对，没错，一条信息，可以设置，接收或发送一串字符或数据，如下图
>
> ![ 2024-11-21 214840.png](https://s2.loli.net/2024/11/21/dPDoacBS352pgWO.png)

> ![ 2024-11-21 214858.png](https://s2.loli.net/2024/11/21/1dIZthTl6w4OXeb.png)

接下来就是本次我要揭露的最大的一个坑

> ![ 2024-11-21 215448.png](https://s2.loli.net/2024/11/21/TRJfliHsmkYdzbG.png)

> 很明显，图中我注释掉的就是出错的部分，vTaskDelay()与xQueueReceive(TEST_Queue,&buffer,porMAX_DELAY)有冲突，在接收一次消息队列后，就会产生很长很长时间的阻塞，至少我等了很久 都没结束阻塞，姑且就认为是永远阻塞吧！！！至于为什么出现这种情况，我认为可能与xQueueReceive()中等待时间有关吧！！！

不能想了，在想就要长脑子了！！！！！！！！！