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

