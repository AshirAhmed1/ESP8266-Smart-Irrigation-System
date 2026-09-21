# 🌱 ESP8266 Smart Irrigation System

An IoT-based smart irrigation system built with the **NodeMCU ESP8266** that monitors soil moisture in real time and controls a water pump based on plant watering needs.

The system combines a **soil moisture sensor**, **relay-controlled water pump**, **16x2 LCD display**, and the **Blynk IoT platform** to provide both local monitoring and remote control over WiFi.

## 🎥 Demo

[▶ Watch Project Demo](https://youtu.be/yMhydp0Kmmc)

---

## ✨ Features

* 🌱 Real-time soil moisture monitoring
* 💧 Water pump control through a relay module
* 🔄 Automatic and manual irrigation control
* 📱 Remote monitoring and pump control through Blynk
* 📟 Live moisture and pump status on a 16x2 LCD
* 📶 WiFi connectivity using the ESP8266
* ☁️ Real-time communication with the Blynk IoT platform
* 📊 Soil moisture displayed as a percentage
* 🪴 Designed for home gardens, indoor plants, and small-scale irrigation

---

## 🛠️ Hardware

| Component                  | Purpose                                    |
| -------------------------- | ------------------------------------------ |
| NodeMCU ESP8266            | Main microcontroller and WiFi connectivity |
| Soil Moisture Sensor       | Measures soil moisture levels              |
| 1-Channel Relay Module     | Controls the water pump                    |
| Mini Water Pump            | Supplies water to the plant                |
| 16x2 LCD Display           | Displays moisture and pump information     |
| I2C Module                 | Provides simplified LCD communication      |
| Breadboard                 | Circuit prototyping                        |
| Jumper Wires               | Electrical connections                     |
| Water Tubing               | Directs water from the pump                |
| External Pump Power Supply | Powers the water pump                      |

---

## 💻 Technologies

* **C / C++**
* **Arduino IDE**
* **ESP8266**
* **Blynk IoT**
* **WiFi**
* **I2C**
* **Embedded Systems**
* **IoT**

---

## ⚙️ How It Works

The system continuously reads the soil moisture level through the sensor connected to the ESP8266.

The raw analog sensor reading is converted into a percentage that represents the approximate moisture level of the soil.

The current moisture value is then:

1. Displayed locally on the LCD
2. Sent to the Blynk dashboard over WiFi
3. Used by the irrigation system to determine watering requirements

The water pump is connected through a relay module so that the low-power ESP8266 can safely control the separate pump circuit.

Users can also interact with the system remotely through Blynk to monitor moisture levels and control the water pump.

---

## 🔌 System Architecture

```text
                   ┌─────────────────┐
                   │   Soil Moisture │
                   │      Sensor     │
                   └────────┬────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   NodeMCU     │
                    │    ESP8266    │
                    └───────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
        ┌──────────┐   ┌─────────┐   ┌───────────┐
        │ 16x2 LCD │   │  Relay  │   │   WiFi    │
        │   + I2C  │   │ Module  │   │ / Blynk   │
        └──────────┘   └────┬────┘   └─────┬─────┘
                            │              │
                            ▼              ▼
                      ┌──────────┐    ┌──────────┐
                      │  Water   │    │  Mobile  │
                      │   Pump   │    │Dashboard │
                      └──────────┘    └──────────┘
```

---

## 🔧 Pin Configuration

The ESP8266 handles both sensor input and pump control.

| Component            | ESP8266 Pin            |
| -------------------- | ---------------------- |
| Soil Moisture Sensor | `A0`                   |
| Water Pump Relay     | `D3`                   |
| LCD                  | I2C                    |
| LCD SDA/SCL          | ESP8266 I2C connection |

The water pump is controlled indirectly through the relay rather than being powered directly by the ESP8266.

---

## 📱 Blynk Integration

The project uses the **Blynk IoT platform** to provide remote monitoring and control.

Two virtual datastreams are used:

| Virtual Pin | Function                 | Range     |
| ----------- | ------------------------ | --------- |
| `V0`        | Soil moisture percentage | `0 - 100` |
| `V1`        | Water pump control       | `0 - 1`   |

### Blynk Dashboard

The dashboard can contain:

* **Gauge** for current soil moisture
* **Button** for water pump control

The ESP8266 connects to Blynk Cloud using WiFi and continuously updates the moisture value.

---

## 📟 LCD Display

The 16x2 LCD provides immediate feedback without requiring a phone or computer.

Example:

```text
Moisture : 64%
Motor is OFF
```

When the pump is activated:

```text
Moisture : 31%
Motor is ON
```

---

## 🚀 Getting Started

### 1. Install Arduino IDE

Download and install the Arduino IDE.

Add support for the **ESP8266 / NodeMCU** board through the Arduino Board Manager.

---

### 2. Install Required Libraries

Install the following Arduino libraries:

```text
ESP8266WiFi
Blynk
LiquidCrystal_I2C
```

These libraries provide:

* ESP8266 WiFi connectivity
* Blynk communication
* I2C LCD control

---

### 3. Build the Circuit

Connect the:

* Soil moisture sensor to the ESP8266
* LCD and I2C module to the ESP8266
* Relay module to the ESP8266
* Water pump to the relay
* Pump to an appropriate external power source

The soil moisture sensor provides the ESP8266 with the current moisture reading, while the relay electrically switches the pump on or off.

---

### 4. Configure Blynk

Create a new device/template in the Blynk dashboard.

Create the following datastreams:

```text
V0 → Moisture Value
Minimum: 0
Maximum: 100
```

```text
V1 → Water Pump
Minimum: 0
Maximum: 1
```

Add:

* A **Gauge** connected to `V0`
* A **Button** connected to `V1`

---

### 5. Configure WiFi and Blynk Credentials

Add your own credentials to the program:

```cpp
char auth[] = "YOUR_BLYNK_AUTH_TOKEN";
char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";
```

> Do not commit real WiFi passwords or Blynk authentication tokens to a public GitHub repository.

---

### 6. Upload the Program

Connect the NodeMCU to your computer through USB.

In Arduino IDE:

1. Select the appropriate **NodeMCU ESP8266 board**
2. Select the correct **COM/serial port**
3. Compile the program
4. Upload it to the ESP8266

Once connected to WiFi and Blynk, the system will begin transmitting soil moisture readings.

---

## 📊 Moisture Measurement

The ESP8266 reads the soil sensor through its analog input:

```cpp
int value = analogRead(sensor);
```

The reading is converted into a percentage:

```cpp
value = map(value, 0, 1024, 0, 100);
value = (value - 100) * -1;
```

The resulting value is displayed on the LCD and transmitted to Blynk:

```cpp
Blynk.virtualWrite(V0, value);
```

---

## 💧 Pump Control

The pump is controlled using a relay connected to the ESP8266.

```cpp
#define waterPump D3
```

The relay allows the microcontroller to switch the higher-current pump circuit without directly powering the pump.

The Blynk control uses virtual pin `V1`:

```cpp
BLYNK_WRITE(V1) {
    Relay = param.asInt();

    if (Relay == 1) {
        digitalWrite(waterPump, LOW);
    } else {
        digitalWrite(waterPump, HIGH);
    }
}
```

This enables the water pump to be controlled remotely from the Blynk dashboard.

---

## 🌐 IoT Workflow

```text
Soil
  ↓
Moisture Sensor
  ↓
ESP8266
  ↓
Moisture Processing
  ↓
 ┌──────────────────────────────┐
 │                              │
 ▼                              ▼
LCD Display                 Blynk Cloud
                                ↓
                         Mobile / Web App
                                ↓
                         Pump Command
                                ↓
                             ESP8266
                                ↓
                              Relay
                                ↓
                          Water Pump
```

---

## 🎯 Project Purpose

This project demonstrates the integration of **embedded hardware, sensors, networking, cloud-based IoT services, and physical automation** into a single working system.

It explores concepts including:

* Sensor data acquisition
* Analog signal processing
* Microcontroller programming
* Relay-based actuator control
* WiFi communication
* Cloud-connected IoT systems
* Mobile device integration
* Real-time environmental monitoring

---

## 🔮 Possible Improvements

Future versions of the system could include:

* Configurable moisture thresholds
* Historical moisture graphs
* Automated watering schedules
* Multiple soil sensors
* Multiple irrigation zones
* Temperature and humidity monitoring
* Water-level monitoring
* Pump safety shutoff
* Push notifications when soil becomes dry
* Weather-aware watering logic

---

## 🎥 Project Demo

See the complete system operating here:

### [▶ Watch the ESP8266 Smart Irrigation System Demo](https://youtu.be/yMhydp0Kmmc)

---

## 📄 License

This project is intended for educational and demonstration purposes.
