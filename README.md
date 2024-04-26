# Arduino-coding-for-sugar-level-measurement
To measure sugar level using Ardiuno
// Include necessary libraries
#include <Fuzzy.h>

// Define input pins
int irSensorPin = A0;

// Define output pins
int glucoseIndicatorPin = 9;

// Initialize fuzzy logic objects
Fuzzy irSensorFuzzy;

void setup() {
  // Initialize serial communication
  Serial.begin(9600);

  // Initialize input and output pins
  pinMode(irSensorPin, INPUT);
  pinMode(glucoseIndicatorPin, OUTPUT);

  // Initialize fuzzy sets and membership functions for IR sensor readings
  irSensorFuzzy.addFuzzySet("Low", 0, 200, 400);
  irSensorFuzzy.addFuzzySet("Medium", 400, 800, 1200);
  irSensorFuzzy.addFuzzySet("High", 1200, 1600, 2000);

  // Define fuzzy rules
  irSensorFuzzy.addFuzzyRule("IF IR is Low THEN Glucose is Low");
  irSensorFuzzy.addFuzzyRule("IF IR is Medium THEN Glucose is Medium");
  irSensorFuzzy.addFuzzyRule("IF IR is High THEN Glucose is High");
}

void loop() {
  // Read IR sensor value
  int irSensorValue = analogRead(irSensorPin);

  // Perform fuzzy inference for IR sensor reading
  float glucoseLevel = irSensorFuzzy.fuzzify("IR", irSensorValue);

  // Output glucose level (for testing purposes)
  Serial.print("Glucose Level: ");
  Serial.println(glucoseLevel);

  // Adjust glucose indicator LED brightness based on glucose level
  int glucoseIndicatorBrightness = map(glucoseLevel, 0, 1, 0, 255);
  analogWrite(glucoseIndicatorPin, glucoseIndicatorBrightness);

  // Add delay to loop
  delay(1000);
}
