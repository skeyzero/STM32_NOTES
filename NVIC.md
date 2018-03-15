# STM32_NVIC


[返回主页](./README.md)
----------
向量中断控制器，简称NVIC.负责中断处理的一个东西。所有外部中断，都需要由它管理。   

NVIC主要工作：   
1.设置优先级分组--主要用于配置n个抢占优先级和m个子优先级。只需要配置一次即可。   
2.为中断配置优先级--上面说的两个   
3.使能中断   
4.没了.....    

NVIC真的没多少东西，当然如果要深入到最底层的话，又是一堆东西。总之不用太纠结，完成以上配置就可以使用了。    

直奔主题：   
KEIL


	/*配置分组*/
	void NVIC_Priority_Group(u8 group)
	{
		switch(group)
		{
			case 0:SCB->AIRCR = 0x05FA0000 | 0x700;break;
			case 1:SCB->AIRCR = 0x05FA0000 | 0x600;break;
			case 2:SCB->AIRCR = 0x05FA0000 | 0x500;break;
			case 3:SCB->AIRCR = 0x05FA0000 | 0x400;break;
			case 4:SCB->AIRCR = 0x05FA0000 | 0x300;break;
		}
	}
	/*
	说明:NVIC配置
	参数:
		NVIC_PreemptionPriority:抢占优先级
		NVIC_SubPriority:子优先级
		NVIC_Channel:中断号
		NVIC_Group:分组
	*/
	
	void NVIC_Config(u8 NVIC_PreemptionPriority,u8 NVIC_SubPriority,u8 NVIC_Channel,u8 NVIC_Group)	 
	{   
		u32 temp;	
		u8 IPRADDR=NVIC_Channel/4;   
		u8 IPROFFSET=NVIC_Channel%4;   
		IPROFFSET=IPROFFSET*8+4;      
		temp=NVIC_PreemptionPriority<<(4-NVIC_Group);	    
		temp|=NVIC_SubPriority&(0x0f>>NVIC_Group);   
		temp&=0xf;   

		if(NVIC_Channel<32)NVIC->ISER[0]|=1<<NVIC_Channel;  
		else NVIC->ISER[1]|=1<<(NVIC_Channel-32);      
		NVIC->IP[IPRADDR]|=temp<<IPROFFSET; 	    				   
	}

	未完，以后再更新
	
------    
如果对你有帮助，欢迎转发，转发请注明出处哦。     
------   
了解更多:[我的博客](https://skeyzero.github.io/)
