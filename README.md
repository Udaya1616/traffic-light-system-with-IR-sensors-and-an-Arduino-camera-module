# traffic-light-system-with-IR-sensors-and-an-Arduino-camera-module
 Creating a traffic light system with IR sensors and Arduino cameras to manage traffic 
// Define IR sensor pins
#define IR_SENSOR1_PIN 2
#define IR_SENSOR2_PIN 3

// Define traffic light pins
#define RED_LIGHT_PIN 4
#define YELLOW_LIGHT_PIN 5
#define GREEN_LIGHT_PIN 6

void setup() {
  // Initialize IR sensor pins as inputs
  pinMode(IR_SENSOR1_PIN, INPUT);
  pinMode(IR_SENSOR2_PIN, INPUT);
  
  // Initialize traffic light pins as outputs
  pinMode(RED_LIGHT_PIN, OUTPUT);
  pinMode(YELLOW_LIGHT_PIN, OUTPUT);
  pinMode(GREEN_LIGHT_PIN, OUTPUT);
}

void loop() {
  // Check IR sensor readings
  bool isVehicleDetected1 = digitalRead(IR_SENSOR1_PIN);
  bool isVehicleDetected2 = digitalRead(IR_SENSOR2_PIN);
  
  // Based on sensor readings, manage traffic lights
  if (isVehicleDetected1 && isVehicleDetected2) {
    // Both intersections have vehicles, stop traffic
    setTrafficLights(RED_LIGHT_PIN, GREEN_LIGHT_PIN, 10000); // Red light for 10 seconds
  } else if (isVehicleDetected1) {
    // Only intersection 1 has vehicles, allow traffic from intersection 2
    setTrafficLights(RED_LIGHT_PIN, GREEN_LIGHT_PIN, 5000); // Red light for 5 seconds
    delay(1000); // Delay to ensure smooth transition
    setTrafficLights(GREEN_LIGHT_PIN, RED_LIGHT_PIN, 10000); // Green light for 10 seconds
  } else if (isVehicleDetected2) {
    // Only intersection 2 has vehicles, allow traffic from intersection 1
    setTrafficLights(GREEN_LIGHT_PIN, RED_LIGHT_PIN, 5000); // Green light for 5 seconds
    delay(1000); // Delay to ensure smooth transition
    setTrafficLights(RED_LIGHT_PIN, GREEN_LIGHT_PIN, 10000); // Red light for 10 seconds
  } else {
    // No vehicles detected, allow traffic from both intersections
    setTrafficLights(GREEN_LIGHT_PIN, RED_LIGHT_PIN, 10000); // Green light for 10 seconds
  }
}

void setTrafficLights(int pin1, int pin2, unsigned long duration) {
  digitalWrite(pin1, HIGH); // Turn on first light
  digitalWrite(pin2, LOW); // Turn off second light
  delay(duration); // Wait for the specified duration
}
