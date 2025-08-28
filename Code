#include <SoftwareSerial.h>
#include <TinyGPS++.h>

// Pin Definitions
#define gpsRX 8
#define gpsTX 9
SoftwareSerial gps(gpsRX, gpsTX);   // NEO-6M GPS (RX, TX)

// TinyGPS++ instance
TinyGPSPlus gpsParser;

void setup() {
  Serial.begin(9600);   // Serial Monitor (also used for GSM AT commands if connected to pins 0 & 1)
  gps.begin(9600);      // Start GPS module

  Serial.println("System Ready. Waiting for GPS fix...");
}

void loop() {
  // Continuously read GPS data
  while (gps.available() > 0) {
    gpsParser.encode(gps.read());

    // If a new valid location is available
    if (gpsParser.location.isUpdated() && gpsParser.location.isValid()) {
      String latitude  = String(gpsParser.location.lat(), 6);
      String longitude = String(gpsParser.location.lng(), 6);

      Serial.println("Latitude: " + latitude);
      Serial.println("Longitude: " + longitude);

      String message = "Location: " + latitude + ", " + longitude;
      sendSMS("+1234567890", message); // Replace with recipient number

      Serial.println("Message Sent: " + message);
      delay(60000); // wait 1 minute before sending the next update
    }
  }
}

// Function to send SMS using GSM (connected on Serial 0/1)
void sendSMS(String number, String message) {
  Serial.println("AT+CMGF=1");              // Set SMS mode to text
  delay(500);
  Serial.print("AT+CMGS=\"");
  Serial.print(number);
  Serial.println("\"");
  delay(500);
  Serial.print(message);
  delay(500);
  Serial.write(26); // Ctrl+Z to send SMS
  delay(5000);      // Allow time for sending
}
