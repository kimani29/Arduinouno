here's the code and wiring
#include <DHT.h>

// Constants
#define DHTPIN 2           // Pin for the DHT sensor
#define DHTTYPE DHT11      // DHT 11 or DHT22 sensor model
#define RELAYPIN 7         // Relay control pin
#define TEMP_THRESHOLD 30  // Temperature threshold (adjust as needed)

// Create DHT object
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);        // Initialize serial communication for debugging
  dht.begin();               // Initialize DHT sensor
  pinMode(RELAYPIN, OUTPUT); // Set relay pin as output
  digitalWrite(RELAYPIN, HIGH); // Turn relay off (active-low relay)
}

void loop() {
  float temperature = dht.readTemperature(); // Read temperature
  
  if (isnan(temperature)) { // Check if reading failed
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" °C");

  // If the temperature is above the threshold, turn on the fan
  if (temperature >= TEMP_THRESHOLD) {
    digitalWrite(RELAYPIN, LOW);  // Turn on the fan (active-low)
    Serial.println("Fan ON");
  } else {
    digitalWrite(RELAYPIN, HIGH); // Turn off the fan
    Serial.println("Fan OFF");
  }
  
  delay(2000); // Wait for 2 seconds before next reading
}
