#include<lpc21xx.h>

#define LED0 1<<17
#define LED1 1<<18
#define LED2 1<<19
#define SW1  1<<14
#define SW2  1<<15
#define SW3  1<<16

typedef unsigned int u32;
typedef struct can
{
	u32 id;
	u32 rtr;
	u32 dlc;
	u32 byteA;
	u32 byteB;
}can_d;


void CAN_INIT(void)
	{
		PINSEL1=0X00014000;
		VPBDIV=1;
		C2MOD=0X01;
		AFMR=0X02;
		C2BTR=0X001C001D; 
		C2MOD=0X00;
	}

	void CAN_TX(can_d m1)
	{
		C2TID1=m1.id;
		C2TFI1=m1.dlc<<16;
		if(m1.rtr==0)
		{
			C2TFI1&=~(1<<30);
			C2TDA1=m1.byteA;
			C2TDB1=m1.byteB;
		}
		else
			C2TFI1|=1<<30;
		
		C2CMR=(1<<0)|(1<<5);
		while((C2GSR&(1<<3))==0);
	}
	
		void CAN_RX(can_d *m1)
	{
		while((C2GSR&(1<<0))==0);
		
		m1->id=C2RID;
		m1->dlc=C2RFS>>16 &0XF;
		m1->rtr=C2RFS>>30 &0X1;
		
		if(m1->rtr==0)
		{
			    m1->byteA=C2RDA;
			    m1->byteB=C2RDB;
		}
		
		C2CMR=1<<2;
	}

/*	void delay_millisecond(int millisecond)
{
	T0PR=60000-1;
	T0TCR=0X01;
