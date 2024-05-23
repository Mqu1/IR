#include <LiquidCrystal_I2C.h>

#define Led1 2
#define Led2 3
#define Led3 4
#define Led4 5

#define Motor 10
#define A 11
#define B 12

#define Buzzer 6
#define Button 13

#define IRsensor1 7
#define IRsensor2 8

int count = 0;
String sequence = "";
int timeoutCounter = 0;

int isObstacle1 = HIGH;
int isObstacle2 = HIGH;

LiquidCrystal_I2C lcd(0x27, 16, 2);


void setup() {
  lcd.init();
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Visitor  Fan Led");
  lcd.setCursor(0, 1);
  lcd.print(count);
  lcd.print("         0%  0");

  pinMode(IRsensor1, INPUT);
  pinMode(IRsensor2, INPUT);

  pinMode(Led1, OUTPUT);
  pinMode(Led2, OUTPUT);
  pinMode(Led3, OUTPUT);
  pinMode(Led4, OUTPUT);

  pinMode(Motor, OUTPUT);
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);

  pinMode(Buzzer, OUTPUT);
  pinMode(Button, INPUT_PULLUP);
}

void loop() {
  delay(200);  // reading will be taken after 200 milliseconds

  int LastState1 = isObstacle1;
  isObstacle1 = digitalRead(IRsensor1);
  int LastState2 = isObstacle2;
  isObstacle2 = digitalRead(IRsensor2);


  if (isObstacle1 == LOW && isObstacle1 != LastState1 && sequence.charAt(0) != '1') {
    sequence += "1";
  }

  else if (isObstacle2 == LOW && isObstacle2 != LastState2 && sequence.charAt(0) != '2') {
    sequence += "2";
  }


  if (sequence.equals("12")) {
    count++;
    sequence = "";
    digitalWrite(Buzzer, HIGH);
    delay(100);
    digitalWrite(Buzzer, LOW);
    delay(450);
    lcd.setCursor(0, 1);
    lcd.print(count);
  } else if (sequence.equals("21") && count > 0) {
    count--;
    sequence = "";
    digitalWrite(Buzzer, HIGH);
    delay(100);
    digitalWrite(Buzzer, LOW);
    delay(450);
    lcd.setCursor(0, 1);
    lcd.print(count);
  }

  if (sequence.length() > 2 || sequence.equals("11") || sequence.equals("22") || timeoutCounter > 200) {
    sequence = "";
  }

  if (sequence.length() == 1) {
    timeoutCounter++;
  } else {
    timeoutCounter = 0;
  }




  if (count <= 0) {
    digitalWrite(Led1, LOW);
    digitalWrite(Led2, LOW);
    digitalWrite(Led3, LOW);
    digitalWrite(Led4, LOW);
    digitalWrite(A, HIGH);
    digitalWrite(B, LOW);
    analogWrite(Motor, 0);
    lcd.setCursor(0, 1);
    lcd.print(count);
    lcd.print("         0%  0");
  }

  else if (count <= 5) {
    digitalWrite(Led1, HIGH);
    digitalWrite(Led2, LOW);
    digitalWrite(Led3, LOW);
    digitalWrite(Led4, LOW);
    digitalWrite(A, HIGH);
    digitalWrite(B, LOW);
    analogWrite(Motor, 60);
    lcd.setCursor(0, 1);
    lcd.print(count);
    lcd.print("        25%  1");
  }

  else if (count <= 10) {
    digitalWrite(Led1, HIGH);
    digitalWrite(Led2, HIGH);
    digitalWrite(Led3, LOW);
    digitalWrite(Led4, LOW);
    digitalWrite(A, HIGH);
    digitalWrite(B, LOW);
    analogWrite(Motor, 120);
    lcd.setCursor(0, 1);
    lcd.print(count);
    lcd.print("        50%  2");
  }

  else if (count <= 15) {
    digitalWrite(Led1, HIGH);
    digitalWrite(Led2, HIGH);
    digitalWrite(Led3, HIGH);
    digitalWrite(Led4, LOW);
    digitalWrite(A, HIGH);
    digitalWrite(B, LOW);
    analogWrite(Motor, 180);
    lcd.setCursor(0, 1);
    lcd.print(count);
    lcd.print("        75%  3");
  }

  else if (count <= 20) {
    digitalWrite(Led1, HIGH);
    digitalWrite(Led2, HIGH);
    digitalWrite(Led3, HIGH);
    digitalWrite(Led4, HIGH);
    digitalWrite(A, HIGH);
    digitalWrite(B, LOW);
    analogWrite(Motor, 230);
    lcd.setCursor(0, 1);
    lcd.print(count);
    lcd.print("        90%  4");
  }

  else {
    count = 0;
  }
  if (digitalRead(Button) == 0) {
    count = 0;
     lcd.clear();
       lcd.setCursor(0, 0);
  lcd.print("Visitor  Fan Led");
    lcd.setCursor(0, 1);
    lcd.print(count);
    
  }
  else{

  }
}
