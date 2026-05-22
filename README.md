# fire-extinguisher-robot
It is a robot which is able to detect fire and extinguish it automaticaly


project code :

#include <Servo.h>

// Sensors
#define ir_R A0
#define ir_F A1
#define ir_L A2

// Pump (ACTIVE LOW)
#define pump A5
#define PUMP_ON LOW
#define PUMP_OFF HIGH

// Motors
#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

Servo myServo;

int s1, s2, s3;
int currentAngle = 90;
int threshold = 300;

void setup() {
  Serial.begin(9600);

  pinMode(ir_R, INPUT);
  pinMode(ir_F, INPUT);
  pinMode(ir_L, INPUT);

  pinMode(pump, OUTPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  myServo.attach(A4);
  myServo.write(90);

  digitalWrite(pump, PUMP_OFF);
}

void loop() {
  s1 = analogRead(ir_R);
  s2 = analogRead(ir_F);
  s3 = analogRead(ir_L);

  Serial.print("R: "); Serial.print(s1);
  Serial.print(" F: "); Serial.print(s2);
  Serial.print(" L: "); Serial.println(s3);

  //  NO FIRE → KEEP MOVING
  if (s1 > threshold && s2 > threshold && s3 > threshold) {
    digitalWrite(pump, PUMP_OFF);
    myServo.write(90);

    forward();   // continuous movement
  }

  //  FIRE DETECTED
  else {
    stopCar();
    digitalWrite(pump, PUMP_ON);

    int minVal = s1;
    int direction = 1;

    if (s2 < minVal) {
      minVal = s2;
      direction = 2;
    }
    if (s3 < minVal) {
      minVal = s3;
      direction = 3;
    }

    if (direction == 1) moveServoSmooth(50);
    else if (direction == 2) moveServoSmooth(90);
    else if (direction == 3) moveServoSmooth(130);

    sweepAroundTarget();
  }
}

//  MOTOR FUNCTIONS
void forward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopCar() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

//  SERVO
void moveServoSmooth(int targetAngle) {
  while (currentAngle != targetAngle) {
    if (currentAngle < targetAngle) currentAngle++;
    else currentAngle--;

    myServo.write(currentAngle);
    delay(10);
  }
}

//  SPRAY
void sweepAroundTarget() {
  for (int i = currentAngle - 10; i <= currentAngle + 10; i++) {
    myServo.write(i);
    delay(20);
  }
  for (int i = currentAngle + 10; i >= currentAngle - 10; i--) {
    myServo.write(i);
    delay(20);
  }
}
