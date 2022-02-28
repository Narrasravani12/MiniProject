#include<reg51.h>

sbit r1 = P1^7;

sbit r2 = P1^6;

sbit r3 = P1^5;

sbit r4 = P1^4;

sbit c1 = P1^3;

sbit c2 = P1^2;

sbit c3 = P1^1;

sbit c4 = P1^0;

sfr lcd = 0xA0;

sbit rs = P3^0;

sbit rw = P3^1;

sbit en = P3^2;

sbit sw = P3^3;

unsigned int p,k,g=1;

unsigned char a[5];

unsigned char pass[5]={"14054"};

void delay(unsigned int x)

{

unsigned int i,j;


for(i=0;i<x;i++)

for(j=0;j<50;j++);
}

void lcd_cmd(unsigned char cmd)

{

lcd=cmd;

rs=0;

rw=0;

en=1;

delay(100);

en=0;

}

void lcd_data(unsigned char value)

{

lcd=value;

rs=1;

rw=0;

en=1;

delay(100);

en=0;

}

void lcd_string(unsigned char *msg)

{

 unsigned int i=0;

 for(i=0 ; msg[i]!='\0';i++)

 {
lcd_data(msg[i]);

}

}

lcd_init()

{

 lcd_cmd(0x38);

 lcd_cmd(0x0e);

 lcd_cmd(0x01);

 lcd_cmd(0x06);
}

unsigned char keyscan(void)

{

P2=0xff;

while(1)

{

	r1=0;

	r4=1;

	if(c1==0)

	{

		k='1';
  
		delay(500);
  
		return k;
  
	}

	else if(c2==0)

	{

		k='2';
  
		delay(500);
  
		return k;
  
	}

	else if(c3==0)

	{

		k='3';
  
		delay(500);
  
		return k;
  
	}

	else if(c4==0)

	{

		k='A';
  
		delay(500);
  
		return k;
  
	}

	r1=1;

	r2=0;

	if(c1==0)

	{

		k='4';
  
		delay(500);
  
		return k;
  
	}

	else if(c2==0)

	{

		k='5';
  
		delay(500);
  
		return k;
	}

	else if(c3==0)

	{

		k='6';
  
		delay(500);
  
		return k;
  
	}

	else if(c4==0)

	{

		k='B';
  
		delay(500);
  
		return k;
  
	}

	r2=1;

	r3=0;

	if(c1==0)

	{

		k='7';
  
		delay(500);
  
		return k;
  
	}
	else if(c2==0)

	{

		k='8';
  
		delay(500);
  
		return k;
	}
	else if(c3==0)

	{

		k='9';
  
		delay(500);
  
		return k;
  
	}

	else if(c4==0)

	{

		k='C';
  
		delay(500);
  
		return k;
	}

	
	r3=1;

	r4=0;

	if(c1==0)

	{ k='*';

		delay(500);
  
		return k;
  
	}
	else if(c2==0)

	{

		k='0';
  
		delay(500);
  
		return k;
  
	}
	else if(c3==0)
	{
		k='#';
		delay(500);
		return k;
	}
	else if(c4==0)

	{
		k='+';
  
		delay(500);
  
		return k;
  
	}
}
}

void main(void)

{ unsigned int i;sw=0;

lcd_init();
	
while(1)
{
	lcd_cmd(0x01);

	lcd_string(" ENTER YOUR");

	lcd_cmd(0xc0);

	lcd_string(" PASSWORD");


	for(i=0;i<6;i++)

	{
		
  keyscan();
  
		
		a[i]=k;
		
		if(i==0)
  
		{
		  lcd_cmd(0x01);
    
			lcd_data(' ');
    
		}
  
		lcd_data(k);
  
	}

		
	if(a[0]==pass[0] && a[1]==pass[1] && a[2]==pass[2] && a[3]==pass[3] && a[4]==pass[4])

	{
		g=1;
  
	}

  else

  {

		g=0;
  
	}	
if(g==1)

{

		lcd_cmd(0x01);
  
		lcd_string(" PASSWORD MATCHED");
  
		lcd_cmd(0xc0);
  
		lcd_string(" ACCESS GRANTED");	
  
  sw=1;	
  
	  p=0;
  
	  delay(2000);
}

else if(g==0)

	{
		p++;
  
		lcd_cmd(0x01);
  
		lcd_string(" WRONG PASSWORD");
  
		lcd_cmd(0xc0);
  
		lcd_string(" ACCESS DENIED");
  
		delay(2000);
  
		delay(2000);
  
		g=1;
  
		if(p==3)
  
		{
				lcd_cmd(0x01);
      
				lcd_string(" PLEASE CONTACT");
      
				lcd_cmd(0xc0);
      
				lcd_string("CUSTOMER CARE");
      
				while(p==3);
		}
} } }
