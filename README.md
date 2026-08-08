# GridGuard - Smart Condition Monitoring and Early Warning System for Rural Utility Poles
**Tagline:** Detect Early. Respond Faster. Protect Infrastructure.

## 1. Overview

GridGuard is a smart infrastructure monitoring system designed to provide early warnings about abnormal conditions affecting rural electrical utility poles.
Rural utility poles are exposed to several structural, electrical, and environmental risks such as excessive tilting, soil erosion, storms, overheating, smoke, fire hazards, and rainfall.
GridGuard monitors these conditions using multiple sensors connected to an ESP32. Instead of considering every sensor as a direct measurement of pole health, the system evaluates three major aspects of the infrastructure:
- Structural condition of the pole
- Electrical and thermal condition around the electrical equipment
- Environmental conditions surrounding the infrastructure

The collected information is processed by the ESP32 to determine the current risk condition. When an abnormal or critical condition is detected, the system provides local alerts and can associate the fault with the GPS location of the affected pole for faster maintenance response.

## 2. Problem Statement

Rural electrical distribution infrastructure is spread across large geographical areas, making frequent manual inspection difficult.
Utility poles can be affected by:
- Soil erosion
- Strong winds and storms
- Physical impact
- Pole inclination
- Overheating of electrical equipment
- Smoke or fire hazards
- Heavy rainfall
- Other environmental conditions

In many situations, faults may only become visible after they have already caused power interruptions, equipment damage, or safety hazards.

### Problem

There is a need for a low-cost and scalable monitoring system in rural areas that can continuously observe important infrastructure conditions and provide early warnings before they develop into major failures.

## 3. Proposed Solution

GridGuard is designed as a multi-parameter condition monitoring system.
The system divides monitoring into three major layers.

### Structural Monitoring

The MPU6050 tilt sensor monitors the inclination of the pole and detects abnormal changes in its orientation.

### Electrical/Thermal Monitoring

Temperature and smoke/gas sensors monitor abnormal conditions around the electrical equipment associated with the pole.

### Environmental Monitoring

The rain sensor provides environmental information that can be used as an additional risk parameter.
The ESP32 collects and processes these parameters and generates a condition or risk status.
GPS is used to identify the geographical location of the monitored pole so that the location can be included in a fault alert.

# 4. Working Principle

The working of GridGuard can be explained in the following steps:

1. The system is powered using an appropriate power supply. During prototype testing, the ESP32 can be powered through USB.
2. The MPU6050 continuously measures the orientation and inclination of the pole.
3. The temperature sensor monitors the ambient temperature around the electrical equipment.
4. The MQ-2 sensor is used in the prototype to detect smoke or gas-related abnormal conditions.
5. The rain sensor detects the presence of rainfall or water.
6. The GPS module provides the geographical coordinates of the monitored pole.
7. All sensor readings are sent to the ESP32.
8. The ESP32 processes the sensor data and compares the readings with predefined thresholds or conditions.
9. Each parameter is evaluated according to its purpose:
   - Tilt is used for structural condition monitoring.
   - Temperature and smoke/gas are used for electrical and thermal condition monitoring.
   - Rain is used as an environmental risk parameter.
10. The system classifies the current condition into different levels such as:
    - Safe
    - Warning
    - Critical
11. The current condition and sensor values can be displayed on the OLED display.
12. LEDs provide a simple visual indication of the condition:
    - Green: Safe
    - Yellow: Warning
    - Red: Critical
13. The buzzer is activated when a critical condition is detected.
14. When a significant fault is detected, the system obtains the GPS coordinates of the affected pole.
15. The fault information can be transmitted to the concerned maintenance authority through an appropriate communication network.
16. The alert can contain:
    - Pole ID
    - Fault type
    - Severity
    - GPS coordinates
    - Detection time
17. The maintenance team can use the information to identify the affected pole and prioritize inspection or maintenance.

# 5. System Architecture

```text
                    SENSOR LAYER
                         |
        +----------------+----------------+
        |                |                |
      MPU6050         DHT22             MQ-2
       Tilt         Temperature       Smoke/Gas
        |                |                |
        +----------------+----------------+
                         |
                   Rain Sensor
                         |
                         v
                      ESP32
                         |
              Data Processing
                         |
              Condition Assessment
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
        OLED        LED / Buzzer     GPS Module
          |              |              |
          |              |              v
          |              |       Location Information
          |              |              |
          +--------------+--------------+
                         |
                         v
                  Fault Information
                         |
                         v
              Maintenance Notification
                         |
                         v
                    Dashboard


# 6. Hardware Components

| Component | Purpose |
|---|---|
| ESP32 | Main microcontroller and data processing |
| MPU6050 | Pole tilt and inclination monitoring |
| DHT22 | Ambient temperature monitoring |
| MQ-2 | Smoke/gas indication |
| Rain Sensor | Rain detection and environmental monitoring |
| GPS Module | Location identification |
| OLED Display | Real-time system information |
| LEDs | Visual warning indication |
| Buzzer | Audible warning |
| Power Supply | System power |


# 7. Monitoring Layers

## 7.1 Structural Condition

The MPU6050 is the primary sensor for monitoring the physical orientation of the pole.
It can identify:
- Abnormal inclination
- Sudden changes in orientation
- Possible soil or foundation instability
- Physical disturbance or impact
The system compares the measured orientation with the calibrated normal position.

## 7.2 Electrical and Thermal Condition

The temperature and smoke/gas sensors are not considered direct measurements of structural pole health.
Instead, they monitor abnormal conditions around the electrical equipment associated with the pole.

### Temperature

The DHT22 is used in the prototype to measure ambient temperature.
A significant increase in temperature can indicate a potentially abnormal thermal condition requiring inspection.

### Smoke/Gas

The MQ-2 is used as a prototype smoke/gas indicator.
Smoke detection can be used as an indication of a possible abnormal condition around electrical equipment.
For a real deployment, industrial-grade and appropriately certified fire/smoke detection technology would be required.

## 7.3 Environmental Condition

The rain sensor provides information about the surrounding environmental condition.
Rain does not directly indicate pole failure.
Instead, it can provide additional context for risk assessment.

For example:

```text
Normal Tilt + Rain
= Environmental Risk

Abnormal Tilt + Rain
= Higher Infrastructure Risk
```

# 8. Risk Assessment

GridGuard evaluates each monitoring parameter according to its role.

| Monitoring Layer | Parameter | Purpose |
|---|---|---|
| Structural | Pole Tilt | Detect possible instability |
| Electrical/Thermal | Temperature | Detect abnormal heating |
| Electrical/Thermal | Smoke/Gas | Indicate possible abnormal condition |
| Environmental | Rain | Provide environmental context |
| Location | GPS | Identify affected infrastructure |

The overall condition can be classified as:

```text
SAFE
  |
  v
WARNING
  |
  v
CRITICAL
```

The system can also display the individual condition of each monitoring layer instead of presenting only one numerical score.

Example:

```text
GRIDGUARD

Structural : SAFE
Electrical : WARNING
Environment: SAFE

Overall Risk: MEDIUM
```

---

# 9. OLED Display

The OLED display provides real-time information about the monitored infrastructure.

Example:

```text
GRIDGUARD

Pole ID: GP-102

Tilt: 3 deg
Temp: 32 C
Smoke: Normal

Status: SAFE
```

During an abnormal condition:

```text
GRIDGUARD ALERT

Pole: GP-102

Fault:
HIGH TEMPERATURE

Risk: CRITICAL

GPS:
10.1234 N
76.5678 E
```

---

# 10. Alert System

GridGuard provides local warning mechanisms.

### LED Indication

| LED | Condition |
|---|---|
| Green | Safe |
| Yellow | Warning |
| Red | Critical |

### Buzzer

The buzzer can be activated during one of the critical conditions, that is pole tilt 

# 11. GPS-Based Location Tracking

The GPS module provides the geographical coordinates of the monitored pole.
When a significant fault is detected, the location can be associated with the fault information.

Example:

```text
GRIDGUARD ALERT

Pole ID: GP-102

Fault:
Pole Tilt Warning

Severity:
HIGH

Location:
10.1234 N
76.5678 E

Inspection Required
```

GPS provides the location information. A separate communication system is required to transmit this information to a remote authority or monitoring platform.

# 12. Authority Notification

For remote notification, GridGuard can be integrated with a suitable communication technology such as:
- Wi-Fi
- Other IoT communication networks

This allows maintenance personnel to identify and prioritize the affected infrastructure.

# 13. Prototype Demonstration

## Scenario 1: Pole Tilt

### Action

The miniature pole is tilted manually.

### System Response

- MPU6050 detects the change in inclination.
- ESP32 processes the tilt value.
- The system identifies the condition as a warning or critical condition depending on the threshold.
- OLED displays the tilt information.
- Yellow or red LED is activated.
- Buzzer can be activated for critical tilt.
- GPS coordinates can be associated with the alert.

## Scenario 2: Smoke Detection

### Action

Safe smoke is introduced near the MQ-2 sensor.

### System Response

- MQ-2 detects the smoke/gas condition.
- ESP32 processes the sensor value.
- The electrical risk status is updated.
- Red LED can be activated for a critical condition.
- Buzzer is activated.
- Fault information can be associated with the GPS location.

## Scenario 3: Temperature Rise

### Action

The temperature around the sensor is increased during the demonstration.

### System Response

- DHT22 measures the temperature.
- ESP32 compares the reading with the defined threshold.
- Thermal condition is updated.
- Warning or critical status is displayed.
- LED and buzzer can be activated depending on the severity.


## Scenario 4: Rain Detection

### Action

Water droplets are introduced to the rain sensor.

### System Response

- Rain sensor detects water.
- ESP32 identifies the environmental condition.
- Environmental risk status is updated.
- OLED displays the rain condition.
- The information can be combined with other parameters for risk assessment.

# 14. Prototype Model

The prototype represents a miniature rural power distribution environment.

The model can include:
- Utility pole
- GridGuard monitoring enclosure
- OLED display
- LED indicators
- Buzzer

The monitoring electronics can be placed inside a small enclosure attached to the miniature pole to demonstrate how the system could be integrated into actual infrastructure.

# 15. Safety Considerations

The prototype does not directly connect the sensors or ESP32 to live high-voltage electrical conductors.
The prototype is intended to demonstrate the monitoring concept using low-voltage electronics.
For actual field deployment, the system would require:
- Electrical isolation
- Proper insulation
- Industrial-grade sensors
- Surge protection
- Fuses and protection circuitry
- Weather-resistant enclosure
- Protected cable routing
- Proper connectors and cable glands
- Reliable power supply
- Battery backup where required
- Sensor calibration
- Communication reliability testing
- Compliance with relevant electrical and safety standards

For real electrical equipment monitoring, a non-contact temperature sensing method such as an appropriate infrared sensor can be considered instead of directly connecting a low-voltage temperature sensor to live electrical equipment.


# 16. Applications

GridGuard can be adapted for monitoring:

- Rural electricity distribution poles
- Remote utility infrastructure
- Storm-prone areas
- Flood-prone areas
- Landslide-prone regions
- High-risk utility locations

The same concept can potentially be extended to:

- Streetlight poles
- Telecom infrastructure
- Other remote utility structures

# 17. Advantages

- Continuous infrastructure monitoring
- Early detection of abnormal conditions
- Low-cost prototype architecture
- GPS-based fault location
- Local warning through LED and buzzer
- Real-time information through OLED
- Multi-parameter condition monitoring
- Potential for remote monitoring
- Scalable to multiple utility poles
- Supports predictive maintenance concepts

# 18. Future Scope

Future versions of GridGuard can include:

- GSM/4G-based remote alerts
- LoRa-based long-range communication
- Solar-powered operation
- Battery backup
- Industrial-grade sensors
- Non-contact infrared temperature monitoring
- Weatherproof IP-rated enclosure
- Cloud-based monitoring dashboard
- Multiple-pole monitoring
- Centralized control room
- Fault history and maintenance records
- Data analytics
- Machine-learning-based risk prediction
- Predictive maintenance
- Integration with utility maintenance platforms

# 19. Project Structure

```text
GridGuard/
│
├── README.md
│
├── src/
│   └── gridguard.ino
│
├── System_Design/
│   ├── use_case_diagram.png
│   ├── state_flow_diagram.png
│   └── hardware_block_diagram.png
│
├── hardware/
│   ├── circuit/
│   └── components.md
│
├── documentation/
│   └── idea_discussion_document.pdf
│
└── images/
    └── prototype.jpg
```


# 20. Project Team

## Team Name

QuadBusters

## Team Members

- Abhay Arakkal
- Aldrin Baiju
- Anna Roshan
- Ansha Mehrin MN 

## Institution

Model Engineering College, Thrikkakara
Ernakulam
Kerala, India

# 21. Hackathon

**Hackathon:** BURN A BOARD

**Project:** GridGuard

**Theme:** INNOVATION FOR RURAL INDIA, ELECTRONIC SOLUTIONS FOR RURAL DEVOLOPMENT


# 26. Disclaimer

GridGuard is currently a prototype developed to demonstrate the concept of low-cost infrastructure condition monitoring.
The prototype sensors are not certified for direct deployment on live electrical distribution infrastructure.
A production system would require appropriate industrial sensors, electrical isolation, environmental protection, calibration, testing, certification, and approval from the relevant authorities.
