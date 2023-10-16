# communication_between_multiplexer_and_demultiplexer
## practica de comunicacion entre un pic multiplexor y un pic demultiplexor
_Envio de dato un dato de binario a letra por el multiplexor y regreso de dato de letra a binario por el demultiplexor_

## Codigo de multiplexor pic 1

``` C++
#include <16f877a.h> //libreria del microcontrolador
#DEVICE adc=10 // resolucion de trabajo
#fuses xt, nowdt 
#use delay (clock = 4M) //reloj interno
#use rs232 (baud=9600, xmit=pin_C6, rcv=pin_C7, parity=N, bits=8) //libreria de trabajo para comunicacion rs232     
#use i2c(Master,Fast=100000, sda=PIN_C4, scl=PIN_C3,force_sw)  // libreríá para manejar la comunicación I2C
#include "i2c_Flex_LCD.c" //driver para manejar la interface LCD I2C
#use standard_io (A,D,E) // declaracion de entradas en los pines A,B,E  

int letra = 0;
int regreso=0;

void main()
{
   lcd_init(0x4E,16,2); //inicializacion de la lcd
   lcd_backlight_led(ON); //encendido de la luz de fondo
   lcd_putc("PIC T"); //mensaje inicial del codigo
   while (TRUE)
   { 
    int SEND = INPUT(PIN_D0);//boton para enviar la cadena de caracteres
    int RC = INPUT(PIN_D1);
    
    //LEER EL ESTADO DE LOS BOTONES
    int BT1 = INPUT(PIN_A0);
    int BT2 = INPUT(PIN_A1);
    int BT3 = INPUT(PIN_A2);
    int BT4 = INPUT(PIN_A3);
    int BT5 = INPUT(PIN_A5);
    int BT6 = INPUT(PIN_E0);
    int BT7 = INPUT(PIN_E1);
    int BT8 = INPUT(PIN_E2);
   
    //GENERAR EL CODIGO BINARIO   
    if((BT1,BT3,BT4,BT5,BT6,BT7)== 0  &&  (BT2,BT8)== 1){
      letra = 65;//A
    }else if((BT1,BT3,BT4,BT5,BT6,BT8)== 0  &&  (BT2,BT7)== 1){
      letra = 66;//B 
    }else if((BT1,BT3,BT4,BT5,BT6)== 0  &&  (BT2,BT7,BT8)== 1){
      letra = 67;//C
    }else if((BT1,BT3,BT4,BT5,BT8,BT7)== 0  &&  (BT2,BT6)== 1){
      letra = 68;   //D 
    }else if((BT1,BT3,BT4,BT5,BT7)== 0  &&  (BT2,BT6,BT8)== 1){
      letra = 69;//E
    }else if((BT1,BT3,BT4,BT5,BT8)== 0  &&  (BT2,BT6,BT7)== 1){
      letra = 70;//F
    }else if((BT1,BT3,BT4,BT5)== 0  &&  (BT2,BT6,BT7,BT8)== 1){
      letra = 71;//G
    }else if((BT1,BT3,BT4,BT6,BT7,BT8)== 0  &&  (BT2,BT5)== 1){
      letra = 72;//H
    }else if((BT1,BT3,BT4,BT6,BT7)== 0  &&  (BT2,BT5,BT8)== 1){
      letra = 73;//I
    }else if((BT1,BT3,BT4,BT6,BT8)== 0  &&  (BT2,BT5,BT7)== 1){
      letra = 74;//J
    }else if((BT1,BT3,BT4,BT6)== 0  &&  (BT2,BT5,BT7,BT8)== 1 ){
      letra = 75;//K
    }else if((BT1,BT3,BT4,BT7,BT8)== 0  &&  (BT2,BT6,BT5)== 1){
      letra = 76;//L
    }else if((BT1,BT3,BT4,BT7)== 0  &&  (BT2,BT5,BT6,BT8)== 1){
      letra = 77;//M
    }else if((BT1,BT3,BT4,BT8)== 0  &&  (BT2,BT5,BT6,BT7)== 1){
      letra = 78;//N
    }else if((BT1,BT3,BT4,BT6,BT7)== 0  &&  (BT2,BT3,BT5,BT8)== 1){
      letra = 110;//Ñ simulada con n 
    }else if((BT1,BT3,BT4)== 0  &&  (BT2,BT5,BT6,BT7,BT8)== 1){
      letra = 79;//O
    }else if((BT1,BT3,BT6,BT5,BT7,BT8)== 0  &&  (BT2,BT4)== 1){
      letra = 80;//P
    }else if((BT1,BT3,BT6,BT5,BT7)== 0  &&  (BT2,BT4,BT8)== 1){
      letra = 81;//Q
    }else if((BT1,BT3,BT6,BT5,BT8)== 0  &&  (BT2,BT4,BT7)== 1){
      letra = 82;//R
    }else if((BT1,BT3,BT6,BT5)== 0  &&  (BT2,BT4,BT7,BT8)== 1){
      letra = 83;//S
    }else if((BT1,BT3,BT6,BT5,BT7,BT8)== 0  &&  (BT2,BT4,BT6)== 1){
      letra = 84;//T
    }else if((BT1,BT3,BT5,BT7)== 0  &&  (BT2,BT4,BT6,BT8)== 1){
      letra = 85;//U
    }else if((BT1,BT3,BT5,BT8)== 0  &&  (BT2,BT4,BT6,BT7)== 1){
      letra = 86;//V
    }else if((BT1,BT3,BT5)== 0  &&  (BT2,BT4,BT6,BT7,BT8)== 1){
      letra = 87;//W
    }else if((BT1,BT3,BT6,BT7,BT8)== 0  &&  (BT2,BT4,BT5)== 1){
      letra = 88;//X
    }else if((BT1,BT3,BT6,BT7)== 0  &&  (BT2,BT4,BT5)== 1){
      letra = 89;//Y   
    }else if((BT1,BT3,BT6,BT8)== 0  &&  (BT2,BT4,BT5,BT7)== 1){
      letra = 90;//Z
    }else if((BT1,BT2,BT4,BT5,BT6,BT7,BT8)== 0  &&  (BT3)== 1){
      letra = 32;//ESPACIO     
    }else{
      letra = 0;
    }

    if (SEND == 1) 
    {
      lcd_putc("\f");
      lcd_gotoxy(1,1);
      lcd_putc("PIC T");
      PUTC(letra);
      lcd_gotoxy(1,2);
      printf(lcd_putc,"Send= %c",letra);
      printf(lcd_putc," #= %d",letra);
      delay_ms(500);
    }
    
    if(RC == 1){     
      lcd_putc("\f");
      lcd_gotoxy(1,1);
      lcd_putc("PIC R");
      regreso = getc();
      lcd_gotoxy(1,2);
      printf(lcd_putc,"dato=%c",regreso);
      printf(lcd_putc," #=%d",regreso);
    
      if(regreso == 65)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);        
      }
      else if(regreso == 66)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);         
      }
      else if(regreso == 67)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 68)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 69)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 70)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 71)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 72)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 73)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 74)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 75)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 76)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 77)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 78)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 110)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_high(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 79)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_high(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 80)
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 'Q')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 'R')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 'S')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 'T')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 'U')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 'V')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 'W')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_low(PIN_B4);
         output_high(PIN_B5);
         output_high(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 'X')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == 'Y')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_high(PIN_B7);
      }
      else if(regreso == 'Z')
      {
         output_low(PIN_B0);
         output_high(PIN_B1);
         output_low(PIN_B2);
         output_high(PIN_B3);
         output_high(PIN_B4);
         output_low(PIN_B5);
         output_high(PIN_B6);
         output_low(PIN_B7);
      }
      else if(regreso == ' ')
      {
         output_low(PIN_B0);
         output_low(PIN_B1);
         output_high(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
      else
      {
         output_low(PIN_B0);
         output_low(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
      }
         delay_ms(10000);
         output_low(PIN_B0);
         output_low(PIN_B1);
         output_low(PIN_B2);
         output_low(PIN_B3);
         output_low(PIN_B4);
         output_low(PIN_B5);
         output_low(PIN_B6);
         output_low(PIN_B7);
         continue;
   }
  }
}  
```

## Codigo de demultiplexor pic 2

``` C++
#include <16f877a.h> //libreria del microcontrolador
#DEVICE adc=10 //resolucion de trabajo
#use delay (clock = 4M) //tiempo del reloj
#fuses xt, nowdt 
#use rs232 (baud=9600, xmit=pin_C6, rcv=pin_C7, parity=N, bits=8) //
#use i2c(Master,Fast=100000, sda=PIN_C4, scl=PIN_C3,force_sw)  // libreríá para manejar la comunicación I2C
#include "i2c_Flex_LCD.c" //driver para manejar la interface LCD I2C
#use standard_io(A)

int letra;
//int i=1;

void main ()
{
   lcd_init(0x4E,16,2); //inicializacion de la lcd
   lcd_backlight_led(ON); //encendido de la luz de fondo
   lcd_putc("PIC R"); //mensaje de pic recibidor
  while (TRUE)
  {
   int RET = INPUT(PIN_A0);//Variable de para activar el retorno de dato
    letra=getc();
    lcd_putc("\f");
    lcd_putc("PIC R");
    //lcd_gotoxy(i,2);
    lcd_gotoxy(1,2);
    printf(lcd_putc,"%c",letra);
    //i++; 
    delay_ms(500);
      
   if(RET == 1)
   {
      lcd_putc("\f");
      lcd_gotoxy(1,1);
      lcd_putc("PIC T");
      lcd_gotoxy(1,2);
      printf(lcd_putc,"enviando %c",letra);
      PUTC(letra);
      delay_ms(500);     
   }  
    continue;
  }
 }    
```
## Diagrama de conexiones en proteus
![image](https://github.com/Gaddiel0710/communication_between_multiplexer_and_demultiplexer/assets/135661300/2ca76c14-67ab-47ed-b67c-70c4fd8e5919)

## demostracion de funcionamiento en proteus
### _envio de datos_

![image](https://github.com/Gaddiel0710/communication_between_multiplexer_and_demultiplexer/assets/135661300/b741d7ef-ac9b-4f35-be62-34baa7378af2)


### _regreso de datos_

![image](https://github.com/Gaddiel0710/communication_between_multiplexer_and_demultiplexer/assets/135661300/86048056-ffee-427b-b5d7-d540d7a7fbd1)
![image](https://github.com/Gaddiel0710/communication_between_multiplexer_and_demultiplexer/assets/135661300/0b9c6819-6590-4a1b-947e-a3d10f99d73e)


## video de demostracion

https://github.com/Gaddiel0710/communication_between_multiplexer_and_demultiplexer/assets/135661300/dabd40cb-1f64-4c37-bda3-da1ddf5e821b

