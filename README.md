# fire-extinguisher-robot
It is a robot which is able to detect fire and extinguish it automaticaly


project code :

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

    
