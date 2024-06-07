// Ports connected to the Driver
const int MA1 = 6;
const int MA2 = 9;
const int MB1 = 10;
const int MB2 = 11;
 
// Defining Trig and Echo Pins
const int Trig = 7;
const int Echo = 8;
 
// Variables to store time and distance.
float t, d;
 
void setup() {
  pinMode(Trig, OUTPUT); // Trig is an output because it will transmit the wave.
  pinMode(Echo, INPUT);  // Echo is an input because it will receive the wave when it bounces.
  pinMode(MA1, OUTPUT);
  pinMode(MA2, OUTPUT);
  pinMode(MB1, OUTPUT);
  pinMode(MB2, OUTPUT);
  Serial.begin(9600);
}
 
void sensor_reading() {
  // Send a pulse to trigger the sensor
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
 
  // Measure the duration of the echo pulse
  t = pulseIn(Echo, HIGH);
  // Calculate the distance in cm
  d = t / 58.0;
  // Print the distance to the serial monitor
  Serial.print(d);
  Serial.print(" cm");
  Serial.println();
  // 100 ms delay between readings
  delay(100);
}
 
void stopVehicle() {
  digitalWrite(MA1, LOW);
  digitalWrite(MA2, LOW);
  digitalWrite(MB1, LOW);
  digitalWrite(MB2, LOW);
}
 
void moveForward() {
  digitalWrite(MA1, HIGH);
  digitalWrite(MA2, LOW);
  digitalWrite(MB1, HIGH);
  digitalWrite(MB2, LOW);
}
 
void moveBackward() {
  digitalWrite(MA1, LOW);
  digitalWrite(MA2, HIGH);
  digitalWrite(MB1, LOW);
  digitalWrite(MB2, HIGH);
}
 
void turnRight() {
  digitalWrite(MA1, LOW);
  digitalWrite(MA2, HIGH);
  digitalWrite(MB1, HIGH);
  digitalWrite(MB2, LOW);
}
 
void turnLeft() {
  digitalWrite(MA1, HIGH);
  digitalWrite(MA2, LOW);
  digitalWrite(MB1, LOW);
  digitalWrite(MB2, HIGH);
}
 
void loop() {
  sensor_reading();
  if (d <= 15) { // If an object is detected within 15 cm
    stopVehicle();
    delay(1000); // Wait for a second
    turnRight(); // Turn right
    delay(1000); // Adjust delay for the turn duration
    stopVehicle(); // Stop after turning
    delay(500); // Wait for a half second
  } else {
    moveForward();
  }
  // 100 ms delay between readings
  delay(100);
}
