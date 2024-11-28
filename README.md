#define BLYNK_TEMPLATE_ID "TMPL3COLzS4H9"      // Replace with your actual Blynk Template ID
#define BLYNK_TEMPLATE_NAME "ac on and off"  // Replace with your actual Blynk Template Name

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <TimeLib.h>  // Time library for time-related functions

// Blynk and Wi-Fi credentials
char auth[] = "rfn3XtEPA9idSsp1mquQUBhnhH00Wq53";  // Replace with your Blynk Auth Token
char ssid[] = "SB-TBI";       // Replace with your Wi-Fi SSID
char pass[] = "sbu123!@#";   // Replace with your Wi-Fi Password

// IR Sensor LED pin
const int irSensorPin = 2;

// Variables for time control
int ledOnHour = 15, ledOnMinute = 38;
int ledOffHour = 15, ledOffMinute = 39;
bool isLedOn = false;

void setup() {
  pinMode(irSensorPin, OUTPUT);
  digitalWrite(irSensorPin, LOW);  // Turn off LED initially

  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);

  Serial.println("Connected to Wi-Fi and Blynk initialized...");
}

// Blynk app sends on time
BLYNK_WRITE(V0) {
  ledOnHour = param[0].asInt();
  ledOnMinute = param[1].asInt();
  Serial.print("LED On Time set to: ");
  Serial.print(ledOnHour);
  Serial.print(":");
  Serial.println(ledOnMinute);
}

// Blynk app sends off time
BLYNK_WRITE(V1) {
  ledOffHour = param[0].asInt();
  ledOffMinute = param[1].asInt();
  Serial.print("LED Off Time set to: ");
  Serial.print(ledOffHour);
  Serial.print(":");
  Serial.println(ledOffMinute);
}

void loop() {
  Blynk.run();

  // Get the current time (replace with Blynk RTC for accurate time sync)
  int currentHour = hour();
  int currentMinute = minute();

  // Check if it's time to turn on the LED
  if (currentHour == ledOnHour && currentMinute == ledOnMinute && !isLedOn) {
    digitalWrite(irSensorPin, HIGH);
    isLedOn = true;
    Serial.println("LED turned ON");
  }

  // Check if it's time to turn off the LED
  if (currentHour == ledOffHour && currentMinute == ledOffMinute && isLedOn) {
    digitalWrite(irSensorPin, LOW);
    isLedOn = false;
    Serial.println("LED turned OFF");
  }
}
