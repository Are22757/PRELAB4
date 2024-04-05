# PRELAB4
/*
 * PRELAB_4.c
 * Author : lisar
 */ 

#define F_CPU 16000000


#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>


uint8_t contador = 0;
uint8_t n;



void initINT0(void);


int main(void)
{
   cli();


   
   //Configuración pines de entrada	  
   DDRB &= ~(1 << DDB0); //Definiendo como entrada PB0 botón de decremento con PULL-UP
   PORTB |= (1 << PORTB0);
   
  
   DDRB &= ~(1 << DDB1); //Definiendo como entrada PB1 botón de incremento con PULL-UP
   PORTB |= (1 << PORTB1);
   
   
   //Configuración pines de salida      
   DDRD |= 0XFF;  //Todo el puerto D será una salida
   PORTD = 0;
   
   //Configuración de interrupciones 
   PCICR |= (1 << PCIE0);
   PCMSK0 |= (1 << PCINT0)| (1 << PCINT1);
   

   UCSR0B = 0;

   sei();
   
    while (1) 
    {
		if(contador > 255)
			contador = 0;
			
		if (contador < 0)
			contador = 255; 
			
		PORTD = contador;
		
			
		}
	}


ISR(PCINT0_vect){
	
	
	if ((PINB&(1<<PINB1))==0) //Reviso el botón de incremento
		contador++;
		

				 
    if ((PINB&(1<<PINB0))==0) //Reviso el botón de decremento

		contador--;
	}



