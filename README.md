#include <Arduino.h>
#include <Wifi.h>

#define BLYNK_TEMPLATE_ID "TMPL69InrAQ9r"
#define BLYNK_TEMPLATE_NAME "LED ESP32"
#define BLYNK_AUTH_TOKEN "IMo3tESBbZ9QooNAQPWkVWrG-VI9UCJ2"

#include <BlynkSimpleEsp32.h>
#include <TimeLib.h>

char ssid[] = "Phong 308";
char pass[] = "Matkhaucu";

int LED = 32;
int Sensor_input = 33;
BlynkTimer timer;

void sendSensor()
{
  int sensor_Aout = analogRead(Sensor_input);
  Serial.print("Gas Sensor: ");
  Serial.print(sensor_Aout);
  Serial.print("\t");
  Blynk.virtuaWrite(V1, sensor_Aout);
  if (sensor_Aout > 1000)
  {
    Serial.print("Gas");
    Blynk.virtualWrite(V0, HIGH);
    digitalWrite(LED, HIGH);
    Blynk.logEvent("over_gas", "Gas Warning");
  }
  else
  {
    Serial.println("No Gas");
    digitalWrite(LED, LOW);
    Blynk.virtualWrite(V0, LOW);
  }
}

void setup()
{
  Serial.begin(9600);
  pinMode(LED, OUTPUT);
  delay(1000);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(1000L, sendSensor);
}

void loop()
{
  Blynk.run();
  timer.run();
}
    
  




