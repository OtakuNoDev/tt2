# Typing Tutor

Welcome to the Typing Tutor project! This project is designed to enhance typing speed and accuracy through interactive exercises, practice modes, and real-time feedback. It offers multiple difficulty levels, a leaderboard system, and personalized progress tracking to help users improve their typing efficiency.

## Installation

### Prerequisites
- HTML
- CSS
- Javascript

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/OtakuNoDev/tt2.git
   cd tt2
   ```

3. Open the project in VS Code (or any preferred IDE).

4. Run the index.html file in a web browser.

---

## Usage

```c
#include <SoftwareSerial.h>
#include <TinyGPS++.h>
#include <ESP8266WiFi.h>

#define RX_PIN 4  // GPS TX to ESP8266 RX (GPIO 4)
#define TX_PIN 5  // ESP8266 TX to GPS RX (GPIO 3, usually not used)

// Wi-Fi credentials
const char* ssid = "SSID";        // Replace with your Wi-Fi SSID
const char* password = "PASSWORD"; // Replace with your Wi-Fi password

// Create a SoftwareSerial object
SoftwareSerial GPS_SoftSerial(RX_PIN, TX_PIN);
TinyGPSPlus gps;
WiFiServer server(80); // Create a web server on port 80

void setup() {
    Serial.begin(115200);           // Start Serial Monitor
    WiFi.begin(ssid, password);     // Connect to Wi-Fi

    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("🌐 Connecting to WiFi...");
    }

    Serial.println("✅ Connected to WiFi");
    server.begin();                 // Start the server
    Serial.println("🚀 Server started");
    GPS_SoftSerial.begin(9600);     // Start SoftwareSerial for GPS
}

void loop() {
    while (GPS_SoftSerial.available()) {
        gps.encode(GPS_SoftSerial.read());
        if (gps.location.isUpdated()) {
            double latitude = gps.location.lat();
            double longitude = gps.location.lng();
            String url = "http://maps.google.com/?q=" + String(latitude, 6) + "," + String(longitude, 6);

            // Print location details to Serial Monitor with emojis
            Serial.println("📍 Location Updated:");
            Serial.print("   🗺️ Latitude: ");
            Serial.println(latitude, 6);
            Serial.print("   🗺️ Longitude: ");
            Serial.println(longitude, 6);
            Serial.println("🔗 Google Maps Link: " + url);

            // Handle client connections
            WiFiClient client = server.available();
            if (client) {
                Serial.println("🌟 New client connected");
                String response = "HTTP/1.1 302 Found\r\n";
                response += "Location: " + url + "\r\n";
                response += "Connection: close\r\n";
                response += "\r\n";
                client.print(response);
                delay(100); // Give the client time to receive the data
                client.stop(); // Close the connection
                Serial.println("👋 Client disconnected");
            }
        }
    }
}

```

This code retrieves GPS coordinates and sends them to the serial monitor or a connected web interface.

---

## Local Development

This local development setup is for testing and running the GPS tracking system on your ESP8266 module.

### Steps

1. **Install dependencies:**
   ```bash
   arduino-cli core install esp8266:esp8266
   ```

2. **Upload to ESP8266:**
   Open the `.ino` file in Arduino IDE, connect the ESP8266, and upload the code.

3. **Test the output:**
   Use a serial monitor to view the GPS coordinates in real time.

---

## Contribution

We welcome contributions! Feel free to fork this repository and submit a pull request.

---

## Contributors ✨

<div align="center">
  <table>
    <tr>
      <td align="center">
        <a href="https://github.com/OtakuNoDev">
          <img src="https://github.com/OtakuNoDev.png" width="100px" style="border-radius: 50%;" alt="Gaurav Jadhav"/><br />
          <sub><b>Gaurav Jadhav</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/Yash-codes2024">
          <img src="https://github.com/Yash-codes2024.png" width="100px" style="border-radius: 50%;" alt="Shreyash Mandlapure"/><br />
          <sub><b>Shreyash Mandlapure</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/Vinit1936">
          <img src="https://github.com/Vinit1936.png" width="100px" style="border-radius: 50%;" alt="Vinit Patil"/><br />
          <sub><b>Vinit Patil</b></sub>
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/Sarthakpatil23">
          <img src="https://github.com/Sarthakpatil23.png" width="100px" style="border-radius: 50%;" alt="Sarthak Patil"/><br />
          <sub><b>Sarthak Patil</b></sub>
        </a>
      </td>
    </tr>
  </table>
</div>
