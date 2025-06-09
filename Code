#define BLYNK_TEMPLATE_ID "Template_ID"
#define BLYNK_TEMPLATE_NAME "Smart Safety Helmet"
#define BLYNK_AUTH_TOKEN "Auth_Token"


#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DHT.h>


// Blynk Auth Token
char auth[] = "Auth_Token";


// WiFi credentials
char ssid[] = "Username";
char pass[] = "Password";

// DHT11 pin and type
#define DHTPIN D5         // Temperature and Humidity 
#define DHTTYPE DHT11
const int Methane_Pin=A0; //MQ4 Methane Gas 
const int CO_Pin=D2;      //MQ7 Carbon Mono-oxide Gas


DHT dht(DHTPIN, DHTTYPE);

void setup() {
  // Start serial communication
  Serial.begin(9600);
  
  // Initialize Blynk
  Blynk.begin(auth, ssid, pass);
  
  // Initialize DHT sensor
  dht.begin();
}

void loop() {
  // Run Blynk
  Blynk.run();
  
  // Get temperature and humidity values
  float h = dht.readHumidity();
  float t = dht.readTemperature(); // or dht.readTemperature(true) for Fahrenheit
  float Methane_Pin =analogRead(A0);
  float CO_Pin= digitalRead(D2);


  // Check if any reads failed
  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  
  // Print values to Serial Monitor
  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperature: ");
  Serial.print(t);
  Serial.println(" *C \t");
  Serial.println("Methane Gas: ");
  Serial.print(Methane_Pin);
  Serial.println(" PPB \t");

  
  int coStatus = digitalRead(D2); // Read digital output from MQ7
  if (coStatus == HIGH) {
    Serial.println("CO level is below the threshold.");
  } else {
    Serial.println("CO level is above the threshold!");
  }
  delay(1000); // Delay for readability
  
  
  // Send values to Blynk
  Blynk.virtualWrite(V2, t);
  Blynk.virtualWrite(V3, h);
  Blynk.virtualWrite(V1,Methane_Pin);
  Blynk.virtualWrite(V0,CO_Pin);
  
  if (t > 35) {
    Blynk.logEvent("temperature_");
  }
  if (h > 35) {
    Blynk.logEvent("humidity_");
  }
  if (Methane_Pin > 100) {
    Blynk.logEvent("Methane_");
  }
  if (CO_Pin > 100) {
    Blynk.logEvent("Carbon Mono-oxide_");
  }
  // Wait a few seconds between measurements
  delay(2000);
}
