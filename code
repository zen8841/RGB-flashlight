#include <LiquidCrystal.h>
LiquidCrystal lcd(PB9, PB8, PB7, PB6, PB5, PB4);
unsigned char colorsw;//switch color
int colorR;
int colorG;
int colorB;
char tcolor[3][4];//0=R 1=G 2=B
unsigned char digit;//0 or 1 or 2
unsigned char sw;//emter or switch mode
short modeswcount;
short oldmodeswcount;
bool mode = 0;//false=digtal true=analog
bool backspace =0;
void setup() {
  pinMode(PA10,PWM);//led pwm R
  pinMode(PA9,PWM);//G
  pinMode(PA8,PWM);//B
  pinMode(PA4,OUTPUT);//dirtive LED
  pinMode(PA3,OUTPUT);//analog input 
  pinMode(PA0,INPUT_ANALOG);//R
  pinMode(PA1,INPUT_ANALOG);//G
  pinMode(PA2,INPUT_ANALOG);//B
  pinMode(PA5,OUTPUT);//3x4 keypad
  pinMode(PA6,OUTPUT);
  pinMode(PA7,OUTPUT);
  pinMode(PB0,OUTPUT);
  pinMode(PB1,INPUT_PULLUP);
  pinMode(PB10,INPUT_PULLUP);
  pinMode(PB11,INPUT_PULLUP); 
  lcd.begin(16, 2);
  lcd.setCursor(2, 0);
  lcd.print("R");
  lcd.setCursor(7, 0);
  lcd.print("G");
  lcd.setCursor(12, 0);
  lcd.print("B");
}

void loop() {
  input();
  pwm();
  numberreset();
}
void input(){
  //input
  //mode=1;
  if(mode==0){
    digitalWrite(PA4, LOW);//diretive LED
    lcd.setCursor(15, 0);
    lcd.print("D");
         
    leerpad();
    
    colorR=atoi(tcolor[0]);
    colorG=atoi(tcolor[1]);
    colorB=atoi(tcolor[2]);
    if((modeswcount>12)){
      mode=1;
      modeswcount=0;
    }
    else if((modeswcount>0)&&(modeswcount==oldmodeswcount)){
      modeswcount=0;
      colorsw++;
    }
    oldmodeswcount=modeswcount;
  }
  else if(mode==1){
    digitalWrite(PA3, HIGH);
    digitalWrite(PA4, HIGH);//diretive LED
    lcd.setCursor(15, 0);
    lcd.print("A");
    colorR=(int)(analogRead(PA0)/16);
    colorG=(int)(analogRead(PA1)/16);
    colorB=(int)(analogRead(PA2)/16);
    //swmode
    digitalWrite(PA5,HIGH);
    digitalWrite(PA6,HIGH);
    digitalWrite(PA7,HIGH);
    digitalWrite(PB0,LOW);
    if (digitalRead(PB1) == LOW) modeswcount++;
    digitalWrite(PB0,HIGH);
    delay(160);
    if(modeswcount>12){
      mode=0;
      modeswcount=0;
      }
    else if((modeswcount>0)&&(modeswcount==oldmodeswcount)){
      modeswcount=0;
      }
    oldmodeswcount=modeswcount;
    }
}
void pwm(){
  //pwm
  if(colorR>255){
    colorR=0;
    lcd.setCursor(0, 1);
    lcd.print("ERROR");
    delay(1500);
    lcd.setCursor(0, 1);
    lcd.print("     ");
  }
  if(colorG>255){
    colorG=0;
    lcd.setCursor(5, 1);
    lcd.print("ERROR");
    delay(1500);
    lcd.setCursor(5, 1);
    lcd.print("     ");
  }
  if(colorB>255){
    colorB=0;
    lcd.setCursor(10, 1);
    lcd.print("ERROR");
    delay(1500);
    lcd.setCursor(10, 1);
    lcd.print("     ");
  }
  pwmWrite(PA10, ((colorR+1)*(colorR+1))-1);
  pwmWrite(PA9, ((colorG+1)*(colorG+1))-1);
  pwmWrite(PA8, ((colorB+1)*(colorB+1))-1);
  lcd.setCursor(1, 1);
  lcd.print(colorR,DEC);
  lcd.setCursor(6, 1);
  lcd.print(colorG,DEC);
  lcd.setCursor(11, 1);
  lcd.print(colorB,DEC);
  }
void numberreset(){
  //number reset
  if(tcolor[colorsw][digit]!=0){
    digit++;
  }
  if(digit>2){
    digit=0;
    colorsw++;
  }
  if(colorsw>2){
    colorsw=0;
  }  
  if(backspace==1){
    backspace=0;
  }
  for(int i=0;i<3;i++){
        tcolor[i][4]=0;
  }
}
void leerpad(){
  digitalWrite(PA5,LOW) ;
  if (digitalRead(PB1) == LOW) tcolor[colorsw][digit]=1;
  if (digitalRead(PB10) == LOW) tcolor[colorsw][digit]=2; 
  if (digitalRead(PB11) == LOW) tcolor[colorsw][digit]=3;
  digitalWrite(PA5,HIGH);
  digitalWrite(PA6,LOW);
  if (digitalRead(PB1) == LOW) tcolor[colorsw][digit]=4;
  if (digitalRead(PB10) == LOW) tcolor[colorsw][digit]=5;
  if (digitalRead(PB11) == LOW) tcolor[colorsw][digit]=6;
  digitalWrite(PA6,HIGH);
  digitalWrite(PA7,LOW);
  if (digitalRead(PB1) == LOW) tcolor[colorsw][digit]=7;
  if (digitalRead(PB10) == LOW) tcolor[colorsw][digit]=8;
  if (digitalRead(PB11) == LOW) tcolor[colorsw][digit]=9;
  digitalWrite(PA7,HIGH);
  digitalWrite(PB0,LOW);
  if (digitalRead(PB1) == LOW) modeswcount++;
  if (digitalRead(PB10) == LOW) tcolor[colorsw][digit]=0;
  if (digitalRead(PB11) == LOW) backspace++;
  digitalWrite(PB0,HIGH);
  delay(160);
}
