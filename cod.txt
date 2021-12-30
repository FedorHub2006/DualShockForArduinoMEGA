//https://github.com/DenisGeek0/ps2gamepadrc
#define ena 3
#define enb 8
#define in1 4
#define in2 5
#define in3 6
#define in4 7
#define servopin1 2
#define servopin2 9
#include <PS2X_lib.h>                         /* PS2 Controller библиотека */
#include <Servo.h>
//#include <LiquidCrystal.h>                    /* LiquidCrystal библиотека дисплея 1602 */
#include <math.h>
PS2X ps2x;
bool state =0;                                  
bool Type = 0;
byte vibrate = 0;
int RX=0,RY=0,LX=0,LY=0, DLX=0, DLY=0, DRX=0, DRY=0;
int v1,v2,v,power;
int deg1=0,deg2=0;
//int servoPin = 3;
// Создаем объект4
//LiquidCrystal lcd(2,3,7, 6, 5, );            /* номерация пинов дисплея подключаемых к ардуино*/
Servo servo1;
Servo servo2;
void setup(){
  Serial.begin(9600);
   //lcd.begin(16, 2);                             /* 16X2 lcd display */
   ps2x.config_gamepad(13,11,10,12, true, true); /* инициализация пинов подключения геймпада:  GamePad(clock, command, attention, data, Pressures?, Rumble?) check for error*/
   /*Type = ps2x.readType();                       /* считывание данных с контроллера */
  /* if(Type==1){                                  
      lcd.setCursor(0, 0);                       
      lcd.print("   DualShock    ");             
      lcd.setCursor(0, 1) ;
      lcd.print("Controller Found");
      delay(1000);
      lcd.clear(); 
   }*/
   servo1.attach(servopin1);
   servo2.attach(servopin2);
   pinMode(in1,OUTPUT);
   pinMode(in2,OUTPUT);
   pinMode(in3,OUTPUT);
   pinMode(in4,OUTPUT);
   digitalWrite(in1,1);
   digitalWrite(in2,0);
   digitalWrite(in3,0);
   digitalWrite(in4,1);
}

void loop(){
     
   ps2x.read_gamepad(false, vibrate);   /* read controller and set large motor to spin at 'vibrate' speed */
 //  lcd.setCursor(0, 0);                 /* Position the LCD cursor */
  // lcd.print("Stick values:   ");       /* Display analog stick values */
   //lcd.setCursor(0, 1);
   LY = ps2x.Analog(PSS_LY);          /* Reading Left stick Y axis */
   LX = ps2x.Analog(PSS_LX);          /* Reading Left stick X axis */
  // RY = ps2x.Analog(PSS_RY);          /* Reading Right stick Y axis */
 //  RX = ps2x.Analog(PSS_RX);          /* Reading Right stick X axis */
   //DRY = abs(RX - 128);
 //  deg1 = map(LX,0,255,90,120);
 //  deg2 = 180 - deg1;
 deg1 = map(LX,0,256,175,65);
 deg2 = map(LX,0,256,210,90);
   v1 = 255;// - DLX;
   v2 = 255;// + DLX;
   //v1 = v1 * DRY / 128;
  // v1 = v2 * DRY / 128;
  // Servo1.write(v1);
   //Servo2.write(v2);
    if(ps2x.Button(PSB_BLUE)){
      analogWrite(ena,v1);
      analogWrite(enb,v2);
    }
    if(ps2x.Button(PSB_GREEN)){
      digitalWrite(in1,0);
      digitalWrite(in2,1);
      digitalWrite(in3,1);
      digitalWrite(in4,0);
      analogWrite(ena,v1);
      analogWrite(enb,v2);
    }
    if(ps2x.Button(PSB_BLUE)){
       digitalWrite(in1,1);
      digitalWrite(in2,0);
      digitalWrite(in3,0);
      digitalWrite(in4,1);
      analogWrite(ena,v1);
      analogWrite(enb,v2);
    }
    
     servo1.write(deg1);
      servo2.write(deg2);

   Serial.print(LY);
   Serial.print(" ");
   Serial.println(LX);
   Serial.print(deg1);
   Serial.print(" ");
   Serial.print(deg2);
   Serial.print(" ");
   Serial.print(ena);
   Serial.print(" ");
   Serial.print(enb);
   Serial.println();
   delay(10);
   digitalWrite(ena,0);   
   digitalWrite(enb,0);
}