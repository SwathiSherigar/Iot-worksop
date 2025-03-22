# Chapter-3

## 1. Setting Up **Arduino**
### Step 1: Install Arduino IDE
1. Download the **Arduino IDE** from [Arduino's official website](https://www.arduino.cc/en/software).
2. Install the software and open the IDE.

### Step 2: Connect Arduino Board
1. Plug the Arduino board into your PC via **USB cable**.
2. Open **Arduino IDE**, go to **Tools > Board**, and select **Arduino Uno**.

### Step 3: Write and Upload a Program
Upload this **LED blinking program**:

```cpp
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
  digitalWrite(13, HIGH);
  delay(1000);
  digitalWrite(13, LOW);
  delay(1000);
}
```
# Setting Up Raspberry Pi  

## Step 1: Install Raspberry Pi OS  
1. Download **Raspberry Pi OS** from the [official Raspberry Pi website](https://www.raspberrypi.com/software/).  
2. Use **Raspberry Pi Imager** to write the OS to an SD card.  

## Step 2: Boot Up Raspberry Pi  
1. Insert the **SD card** into the Raspberry Pi.  
2. Connect a **monitor, keyboard, and power supply**.  
3. Turn on the Raspberry Pi and complete the setup.  

## Step 3: Run Your First Python Program  
1. Open the **Terminal** on your Raspberry Pi.  
2. Type the following command to open the Python interpreter:  
   ```bash
   python3
 
3. Run a simple Python program:  
   ```python
   print("Hello, Raspberry Pi!")
   ```
4. Press **Enter**, and you should see:  
   ```
   Hello, Raspberry Pi!
   ```

# Setting Up ESP8266 WiFi Module

## **Step 1: Connect ESP8266 with Arduino**
Ensure that you make the correct connections between **ESP8266** and **Arduino**:

| ESP8266 Pin | Arduino Pin |
|------------|-------------|
| **VCC** | 3.3V |
| **GND** | GND |
| **TX** | RX (through a **voltage divider** to avoid 5V damage) |
| **RX** | TX |

⚠ **Important Note:** The ESP8266 operates at **3.3V**, while Arduino Uno operates at **5V**. Directly connecting TX from Arduino to RX of ESP8266 **may damage it**. Use a **voltage divider** (two resistors) to step down the 5V signal.

---

## **Step 2: Upload a Basic WiFi Connection Program**
Use the following improved code to connect ESP8266 to WiFi:

```cpp
#include <ESP8266WiFi.h>

const char* ssid = "yourSSID";       // Replace with your WiFi network name
const char* password = "yourPASSWORD";  // Replace with your WiFi password

void setup() {
  Serial.begin(115200);
  delay(1000);

  Serial.println("\nConnecting to WiFi...");
  WiFi.begin(ssid, password);

  int attempt = 0;
  while (WiFi.status() != WL_CONNECTED && attempt < 20) {
    delay(1000);
    Serial.print(".");
    attempt++;
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nWiFi Connected!");
    Serial.print("IP Address: ");
    Serial.println(WiFi.localIP());
  } else {
    Serial.println("\nFailed to connect. Check your credentials.");
  }
}

void loop() {
  // Keeping loop empty as we just want to connect to WiFi
}
```
# How It Works

- The ESP8266 **attempts to connect** to the given WiFi credentials.
- It **waits for up to 20 seconds** to establish the connection.
- If successful, it prints the **assigned IP address**.
- If it fails, it prints an **error message**.

---

## **Step 3: Checking Serial Monitor Output**
1. Open the **Serial Monitor** in Arduino IDE.
2. Set the **baud rate to 115200**.
3. If the connection is successful, you should see:

```arduino
Connecting to WiFi...
.....
WiFi Connected!
IP Address: 192.168.1.100
.....
```
# Interfacing LED/Buzzer with Arduino and Raspberry Pi

In this will show you how to interface an LED (or a buzzer) with both an Arduino and a Raspberry Pi. We will also write a simple program that turns the LED on for 1 second every 2 seconds.

---

## **1. Using Arduino**

### **Hardware Setup**
- **LED/Buzzer:** Connect the positive (longer) leg of the LED (or the positive terminal of the buzzer) to digital pin **13** of the Arduino.
- **Resistor:** For an LED, connect a current-limiting resistor (typically 220Ω to 330Ω) in series.
- **Ground:** Connect the negative leg of the LED (or the negative terminal of the buzzer) to the GND pin of the Arduino.

### **Arduino Code**
Upload the following code using the Arduino IDE:

```cpp
void setup() {
  // Initialize digital pin 13 as an output.
  pinMode(13, OUTPUT);
}

void loop() {
  // Turn the LED/Buzzer on
  digitalWrite(13, HIGH);
  delay(1000); // Wait for 1 second

  // Turn the LED/Buzzer off
  digitalWrite(13, LOW);
  delay(1000); // Wait for 1 second (total cycle = 2 seconds)
}
```
## **2. Using Raspberry Pi**

### **Hardware Setup**
- **LED/Buzzer:** Connect the positive leg of the LED (or buzzer) to a GPIO pin (e.g., **GPIO 18**).
- **Resistor:** For an LED, use a current-limiting resistor (typically 220Ω to 330Ω).
- **Ground:** Connect the negative leg of the LED (or buzzer) to one of the Raspberry Pi's GND pins.

### **Python Code**
Install the **RPi.GPIO** library if not already installed:

```bash
sudo apt-get update
sudo apt-get install python3-rpi.gpio
```

Then, create a Python script (e.g., `led_buzzer.py`) with the following code:

```python
import RPi.GPIO as GPIO
import time

# Use BCM GPIO numbering
GPIO.setmode(GPIO.BCM)

# Define the GPIO pin (e.g., 18)
LED_PIN = 18

# Set up the GPIO pin as an output
GPIO.setup(LED_PIN, GPIO.OUT)

try:
    while True:
        # Turn the LED/Buzzer on
        GPIO.output(LED_PIN, GPIO.HIGH)
        time.sleep(1)  # Wait for 1 second

        # Turn the LED/Buzzer off
        GPIO.output(LED_PIN, GPIO.LOW)
        time.sleep(1)  # Wait for 1 second (total cycle = 2 seconds)
except KeyboardInterrupt:
    # Cleanup GPIO settings before exiting
    GPIO.cleanup()
```

*Explanation:*  
- We use the **RPi.GPIO** library to control the GPIO pins.
- The code sets up GPIO 18 as an output pin.
- The program then continuously turns the LED (or buzzer) on for 1 second and off for 1 second.
- Press **Ctrl+C** to stop the script, which will trigger the cleanup of GPIO settings.

---



# Experiment 2: Interface Push Button/Digital Sensor with Arduino/Raspberry Pi

## Objective
To interface a push button or digital sensor (BR/LDR) with Arduino/Raspberry Pi and write a program to turn ON an LED when the push button is pressed or when the sensor detects something.

## Steps
1. Connect the push button or sensor to the Arduino/Raspberry Pi.
2. Write a program to read the input from the button/sensor.
3. Control the LED based on the input.
4. Upload the program and test the functionality.

## Code Example
```cpp
const int buttonPin = 2;  // Push button pin
const int ledPin = 13;    // LED pin

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  if (digitalRead(buttonPin) {
    digitalWrite(ledPin, HIGH); // Turn LED ON
  } else {
    digitalWrite(ledPin, LOW);  // Turn LED OFF
  }
}
```

---


# Experiment 3: Interface DHT11 Sensor with Arduino/Raspberry Pi

## Objective
To interface the DHT11 sensor with Arduino/Raspberry Pi and write a program to print temperature and humidity readings.

## Steps
1. Connect the DHT11 sensor to the Arduino/Raspberry Pi.
2. Install the DHT library for Arduino.
3. Write a program to read temperature and humidity data.
4. Print the readings to the serial monitor.

## Code Example
```cpp
#include <DHT.h>

#define DHTPIN 2      // DHT11 data pin
#define DHTTYPE DHT11 // DHT11 sensor type

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperature: ");
  Serial.print(t);
  Serial.println(" *C");
  delay(2000);
}
```

---


# Experiment 4: Interface OLED with Arduino/Raspberry Pi

## Objective
To interface an OLED display with Arduino/Raspberry Pi and write a program to print temperature and humidity readings.

## Steps
1. Connect the OLED display to the Arduino/Raspberry Pi.
2. Install the necessary libraries for the OLED display.
3. Write a program to display temperature and humidity data on the OLED.
4. Upload the program and observe the readings on the display.

## Code Example
```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <DHT.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
#define DHTPIN 2
#define DHTTYPE DHT11

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);
  dht.begin();
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  display.clearDisplay();
  display.setCursor(0, 0);
  display.print("Humidity: ");
  display.print(h);
  display.print(" %");
  display.setCursor(0, 20);
  display.print("Temp: ");
  display.print(t);
  display.print(" *C");
  display.display();
  delay(2000);
}
```

---


# Experiment 5: Interface Motor with Relay using Arduino/Raspberry Pi

## Objective
To interface a motor using a relay with Arduino/Raspberry Pi and write a program to turn ON the motor when a push button is pressed.

## Steps
1. Connect the motor, relay, and push button to the Arduino/Raspberry Pi.
2. Write a program to control the motor using the relay.
3. Upload the program and test the functionality.

## Code Example
```cpp
const int buttonPin = 2;  // Push button pin
const int relayPin = 8;   // Relay pin

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(relayPin, OUTPUT);
}

void loop() {
  if (digitalRead(buttonPin)) {
    digitalWrite(relayPin, HIGH); // Turn motor ON
  } else {
    digitalWrite(relayPin, LOW);  // Turn motor OFF
  }
}
```

---



# Experiment 6: Interface Soil Moisture Sensor with Arduino/Raspberry Pi

## Objective
To interface a soil moisture sensor with Arduino/Raspberry Pi and write a program to read moisture levels.

## Steps
1. Connect the soil moisture sensor to the Arduino/Raspberry Pi.
2. Write a program to read the sensor data.
3. Print the moisture levels to the serial monitor.

## Code Example
```cpp
const int moisturePin = A0; // Soil moisture sensor pin

void setup() {
  Serial.begin(9600);
}

void loop() {
  int moistureValue = analogRead(moisturePin);
  Serial.print("Moisture Level: ");
  Serial.println(moistureValue);
  delay(1000);
}

```


---


# Experiment 7: Interface LDR/Photo Sensor with Arduino/Raspberry Pi

## Objective
To interface an LDR/Photo Sensor with Arduino/Raspberry Pi and write a program to read light intensity.

## Steps
1. Connect the LDR/Photo Sensor to the Arduino/Raspberry Pi.
2. Write a program to read the sensor data.
3. Print the light intensity to the serial monitor.

## Code Example
```cpp
const int ldrPin = A0; // LDR pin

void setup() {
  Serial.begin(9600);
}

void loop() {
  int ldrValue = analogRead(ldrPin);
  Serial.print("Light Intensity: ");
  Serial.println(ldrValue);
  delay(1000);
}
```

---



# Experiment 8: Interface Ultrasonic Sensor with Arduino/Raspberry Pi

## Objective
To interface an ultrasonic sensor with Arduino/Raspberry Pi and write a program to measure distance.

## Steps
1. Connect the ultrasonic sensor to the Arduino/Raspberry Pi.
2. Write a program to calculate the distance using the sensor.
3. Print the distance to the serial monitor.

## Code Example
```cpp
const int trigPin = 9;
const int echoPin = 10;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duration = pulseIn(echoPin, HIGH);
  int distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  delay(1000);
}
```

---


# Experiment 9: Upload Data to ThingSpeak Cloud using Arduino/Raspberry Pi

## Objective
To write a program on Arduino/Raspberry Pi to upload temperature and humidity data to ThingSpeak cloud.

## Steps
1. Create a ThingSpeak account and set up a channel.
2. Connect the DHT11 sensor to the Arduino/Raspberry Pi.
3. Write a program to upload data to ThingSpeak.
4. Monitor the data on the ThingSpeak dashboard.

## Code Example
```cpp
#include <DHT.h>
#include <ESP8266WiFi.h>
#include <ThingSpeak.h>

#define DHTPIN 2
#define DHTTYPE DHT11

const char* ssid = "YourSSID";
const char* password = "YourPassword";
unsigned long myChannelNumber = 123456; // Replace with your channel number
const char* myWriteAPIKey = "YourWriteAPIKey";

WiFiClient client;
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  ThingSpeak.begin(client);
  dht.begin();
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  ThingSpeak.setField(1, t);
  ThingSpeak.setField(2, h);
  ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);

  delay(20000); // Update every 20 seconds
}
```
---



# Experiment 10: Retrieve Data from ThingSpeak Cloud using Arduino/Raspberry Pi

## Objective
To write a program on Arduino/Raspberry Pi to retrieve temperature and humidity data from ThingSpeak cloud.

## Steps
1. Set up a ThingSpeak channel and ensure data is being uploaded.
2. Write a program to retrieve data from ThingSpeak.
3. Display the retrieved data on the serial monitor.

## Code Example
```cpp
#include <ESP8266WiFi.h>
#include <ThingSpeak.h>

const char* ssid = "YourSSID";
const char* password = "YourPassword";
unsigned long myChannelNumber = 123456; // Replace with your channel number
const char* myReadAPIKey = "YourReadAPIKey";

WiFiClient client;

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  ThingSpeak.begin(client);
}

void loop() {
  float temperature = ThingSpeak.readFloatField(myChannelNumber, 1, myReadAPIKey);
  float humidity = ThingSpeak.readFloatField(myChannelNumber, 2, myReadAPIKey);

  Serial.print("Temperature: ");
  Serial.println(temperature);
  Serial.print("Humidity: ");
  Serial.println(humidity);

  delay(20000); // Retrieve data every 20 seconds
}
```
