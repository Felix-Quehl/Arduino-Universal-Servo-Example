# Arduino Universal Servo Example

I tried to control a servo with the Arduino standard library. Unfortunately I found the existing documentation a bit hard to understand, so I publish my final code here as a universal example for other people struggling.

## Code


```cpp
// include the arduino servo library
#include <Servo.h>

// servo pulse width range specification
const int servo_pwm_min = 500;
const int servo_pwm_max = 2500;
// servo angle range specification
const int servo_angle_range = 270;
// calculating servo values
const int servo_pwm_range = servo_pwm_max - servo_pwm_min;

// servo pin
const double servo_pin = 10;

// servo object to control the servo
Servo servo;

void setup() {
  Serial.begin(9600);
  servo.attach(servo_pin, servo_pwm_min, servo_pwm_max);
}

void loop() {

  Serial.println("Resetting Servo Position");
  servo.writeMicroseconds(servo_pwm_min);
  delay(2000);
  Serial.println("Starting Sweep...");

  for (int pwm_position = servo_pwm_min; pwm_position <= servo_pwm_max; pwm_position = pwm_position + 10) {

    double pwm_off_set_from_pwm_min = pwm_position - servo_pwm_min;
    double precentage_of_pwm_range = pwm_off_set_from_pwm_min / servo_pwm_range;
    double angle_position = precentage_of_pwm_range * servo_angle_range;

    servo.writeMicroseconds(pwm_position);

    Serial.print("PWM Position: ");
    Serial.print(pwm_position);
    Serial.print(" / Angle Position: ");
    Serial.print(angle_position);
    Serial.println();

    int multiple_of_45 = ((int)angle_position) % 45;
    if (multiple_of_45) {
      delay(1000);
    }
  }
  Serial.println("Sweep end!");

  delay(2000);
}

```
