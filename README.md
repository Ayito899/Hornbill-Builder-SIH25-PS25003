🌱 NodeMCU Multi-Sensor System with WiFi & ThingSpeak

A robust IoT sensor system using NodeMCU ESP8266 to monitor soil moisture, tilt/vibration, temperature, and upload data to ThingSpeak with reliable WiFi error handling.

📋 Features
✅ Multi-sensor monitoring: Soil moisture, MPU6050 (tilt/vibration/temperature)

✅ Reliable WiFi: Automatic reconnection with error -301 fix

✅ ThingSpeak Integration: 8-channel real-time data visualization

✅ Calibration System: Custom soil sensor calibration

✅ Motion Detection: Vibration-based motion state

✅ Serial Monitoring: Real-time data display

✅ Error Recovery: Robust error handling and automatic retry

✅ Open Source: MIT Licensed

🛠️ Hardware Requirements
Components List:
Component	Quantity	Notes
NodeMCU ESP8266	1	ESP-12E module
MPU6050	1	6-axis accelerometer/gyro
Soil Moisture Sensor	1	Capacitive recommended
Breadboard	1	400 points
Jumper Wires	10+	Male-to-Male
Micro USB Cable	1	For programming/power
Connection Diagram:
text
NodeMCU        MPU6050        Soil Sensor
==========================================
3.3V  ----->  VCC            VCC
GND   ----->  GND            GND
D1 (GPIO5) -> SCL
D2 (GPIO4) -> SDA
A0    ----------------------> AOUT

Clone and build:

bash
git clone https://github.com/yourusername/nodemcu-multi-sensor.git
cd nodemcu-multi-sensor
pio run  # Build project
pio run -t upload  # Upload to device
pio device monitor  # View serial output
Method 2: Arduino IDE
Add ESP8266 Board:

File → Preferences → Additional Board Manager URLs:

text
http://arduino.esp8266.com/stable/package_esp8266com_index.json
Install Libraries:

Tools → Board → Boards Manager → Search "esp8266" → Install

Sketch → Include Library → Manage Libraries → Search "ThingSpeak" → Install

Upload Code:

Open src/main.cpp

Select Board: NodeMCU 1.0 (ESP-12E Module)

Select Port

Click Upload

⚙️ Configuration
1. WiFi Credentials
Edit src/main.cpp:

cpp
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
2. ThingSpeak Configuration
Create account at thingspeak.com

Create new channel with 8 fields:

Field	Measurement	Unit
1	Soil Moisture	%
2	Pitch	°
3	Roll	°
4	Vibration	mg
5	Temperature	°C
6	Total Acceleration	g
7	WiFi Signal	dBm
8	Motion State	0/1
Update credentials:

cpp
const unsigned long channelID = 1234567;  // Your channel ID
const char* writeAPIKey = "YOUR_API_KEY";
3. Soil Sensor Calibration
Important: Calibrate for accurate readings:

cpp
// Dry value: Sensor in air (completely dry)
const int DRY_VALUE = 776;    // UPDATE THIS

// Wet value: Sensor in water
const int WET_VALUE = 275;    // UPDATE THIS
Calibration Process:

Open Serial Monitor (115200 baud)

Place sensor in dry air, note reading

Place sensor in water, note reading

Update values in code

Re-upload

📊 Data Visualization
ThingSpeak Dashboard:
Create widgets to display:

Soil moisture gauge (0-100%)

Tilt dial (pitch/roll)

Vibration graph

Temperature chart

WiFi signal strength

Motion indicator

Sample Dashboard Layout:
text
┌─────────────────┬─────────────────┐
│ Soil Moisture   │ Tilt Indicator  │
│      75%        │   Pitch: 15°    │
│                 │   Roll: -5°     │
├─────────────────┼─────────────────┤
│ Temperature     │ Vibration       │
│     25.3°C      │     245 mg     │
├─────────────────┼─────────────────┤
│ WiFi Signal     │ Motion State    │
│    -65 dBm      │      YES       │
└─────────────────┴─────────────────┘
🚀 Usage
Initial Setup:
Connect hardware as per diagram

Upload code to NodeMCU

Open Serial Monitor (115200 baud)

Wait for WiFi connection

Verify sensor readings

Serial Monitor Output:
text
================================
   NodeMCU Sensor System v2.0
   WiFi Error -301 Fix
================================

🔗 Connecting to WiFi: YOUR_NETWORK
✅ WiFi Connected!
   IP Address: 192.168.1.100
   Signal Strength: -65 dBm
✅ ThingSpeak initialized

================================
🌱 SOIL: 75% (RAW VALUE: 450)
📐 TILT: Pitch=15.5°, Roll=-3.2°
📳 VIB: 245mg | 🌡️ TEMP: 25.3°C
⚡ ACCEL: 1.02g | MOTION: NO
📶 WIFI: -65 dBm | IP: 192.168.1.100
📤 UPLOAD: 15s
================================
Upload Intervals:
Sensor reading: Every 50ms

Serial display: Every 1 second

ThingSpeak upload: Every 20 seconds

WiFi retry: Every 30 seconds if disconnected

🔧 Troubleshooting
Common Issues:
Problem	Solution
WiFi not connecting	Check credentials, signal strength
ThingSpeak error -301	WiFi disconnected, auto-retry enabled
MPU6050 not detected	Check I2C connections, 3.3V power
Soil readings erratic	Calibrate DRY/WET values
Compilation errors	Install correct libraries
Upload fails	Check COM port, install drivers
Debug Commands:
cpp
// Enable debug output in Serial Monitor
// Add to setup():
Serial.setDebugOutput(true);
WiFi Status Codes:
cpp
WL_CONNECTED = 3
WL_NO_SSID_AVAIL = 1
WL_CONNECT_FAILED = 4
WL_DISCONNECTED = 6
📁 Project Structure
text
nodemcu-multi-sensor/
├── .github/
│   └── workflows/
│       └── platformio.yml     # CI/CD pipeline
├── src/
│   ├── main.cpp              # Main application
│   ├── config.h.example      # Configuration template
│   ├── sensors.h            # Sensor functions
│   └── wifi_manager.h       # WiFi handling
├── test/                    # Unit tests
├── docs/                    # Documentation
├── platformio.ini          # PlatformIO configuration
├── .gitignore
├── LICENSE
└── README.md               # This file
🧪 Testing
Unit Tests:
bash
# Run tests with PlatformIO
pio test

# Test specific environment
pio test -e nodemcuv2
Sensor Validation:
Soil Sensor: Test with known dry/wet conditions

MPU6050: Check tilt readings when level

WiFi: Monitor signal strength vs distance

Upload: Verify ThingSpeak data matches serial output

📈 Performance
Memory Usage:
Flash: ~400KB of 4MB

RAM: ~40KB of 80KB

Uptime: Weeks with stable power

Power Consumption:
Active: ~80mA

Sleep (if implemented): ~20μA

Recommended PSU: 5V/1A USB adapter

🔄 Updates & Maintenance
Update Schedule:
Check for library updates monthly

Monitor ThingSpeak API changes

Review WiFi stability logs

Calibrate sensors quarterly

Backup Configuration:
bash
# Save your calibrated values
cp src/config.h src/config.h.backup
🤝 Contributing
Fork the repository

Create a feature branch

Make your changes

Test thoroughly

Submit a pull request

Development Setup:
bash
git clone https://github.com/yourusername/nodemcu-multi-sensor.git
cd nodemcu-multi-sensor
git checkout -b feature/new-sensor
# Make changes
pio run  # Verify compilation
pio test # Run tests
📄 License
MIT License - See LICENSE file for details.

🙏 Acknowledgments
ThingSpeak for IoT platform

PlatformIO for development tools

ESP8266 Community

MPU6050 Library

📞 Support
Issues: GitHub Issues

Documentation: Wiki

Email: your.email@example.com

📊 Real-time Monitoring
Access your data:

ThingSpeak: https://thingspeak.com/channels/YOUR_CHANNEL_ID

Serial Monitor: 115200 baud

Mobile App: ThingSpeak app for iOS/Android

Made with ❤️ for the IoT Community

https://api.star-history.com/svg?repos=yourusername/nodemcu-multi-sensor&type=Date

Quick Start Cheat Sheet:
bash
# 1. Clone & setup
git clone <repo-url>
cd nodemcu-multi-sensor

# 2. Configure
cp src/config.h.example src/config.h
# Edit config.h with your credentials

# 3. Build & upload
pio run -t upload

# 4. Monitor
pio device monitor

# 5. Check data
# Visit: https://thingspeak.com/channels/YOUR_ID

Happy Monitoring! 🌱📡
