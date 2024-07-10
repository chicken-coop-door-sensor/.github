# Chicken Coop Door Sensor (C2DS) Project

## Introduction

Welcome to the Chicken Coop Door Sensor (C2DS) Project, an innovative solution designed to enhance the FarmLite Chicken Coop Door system. The FarmLite system is already an excellent automatic door opener, ensuring the safety and convenience of managing your chicken coop. Our project aims to supplement this system with a passive, external monitoring facility that provides real-time notifications about the door's status—whether open or closed—through various communication methods including text messages, email alerts, and visual indicators.

The goal of C2DS is to ensure the safety and security of your chickens without modifying the existing door mechanism or the chickens themselves. This system leverages ESP32-based appliances and Amazon Web Services (AWS) to deliver a reliable and efficient monitoring solution.

## Environmental Assumptions

- The chicken door open/close mechanism will remain unmodified.
- The sensor system will be entirely passive, meaning it will not actively participate in the opening or closing of the chicken door.
- There will be no modifications to the chickens (e.g., no chips or leg bracelets).
- All times are referenced in the `America/Chicago` (US Central Time) timezone.
- Latitude 35.4834 and Longitude -86.4603 (Shelbyville, TN) will be used for sunrise and sunset calculations.

## Software Assumptions

- An Espressif ESP32-based appliance located in the coop (referred to as the "Coop Controller") will report door status and illuminate an LED for visual status indication.
- Another ESP32 appliance (referred to as the "Coop Snooper") will be used remotely to indicate door status, featuring an on-board LED for status indication.
- Both the Coop Controller and Coop Snooper will be AWS IoT Things.
- Both appliances will run on ESP-IDF/FreeRTOS software, written in C.
- Lambda functions on AWS will handle most of the decision-making processes.
- Communication between the Controller and Snooper will be facilitated via MQTT.
- Both appliances will support Over-The-Air (OTA) upgrades via AWS S3 buckets.

## System Limitations

- The system, once deployed, should not require any manual intervention.
- The hardware will be located several miles and an hour's drive away from the user’s residence.

## Communication Methods

The sensor will communicate the chicken coop door status through four main channels:
1. Text messages (pending finalization with AWS).
2. Email notifications.
3. Illuminating an LED in the coop window.
4. Illuminating an LED on the "Coop Snooper" appliance.

## Error Conditions

User notifications will be triggered under the following error conditions:
- Failure of the door to close at sunset.
- Failure of the door to open at sunrise.
- Missing keep-alive messages, indicating a Controller failure.
- Failure in retrieving sunrise/sunset times.
- Status disagreement between the two optocouplers, indicating a potential Controller connectivity problem.

## Hardware Components

- **Coop Controller**: An ESP32-WROOM-32U module, equipped with an external antenna and optocouplers for door status detection.
- **Coop Snooper**: An ESP32-C3-DevKitM-1 module, used for remote status indication with an RGB LED.

## Status Notification Logic

The Coop Controller can send three types of messages to AWS: `OPEN`, `CLOSED`, and `ERROR`. Based on these messages, AWS Lambda functions assess the current status and notify the user accordingly. The LED color codes are as follows:

| Coop Door | Time-of-Day | Assessed State Will Be                    | LED Color    |
|-----------|-------------|-------------------------------------------|--------------|
| OPEN      | Day         | CHICKEN_COOP_DOOR_OPEN_IN_DAYTIME_OK      | GREEN        |
| CLOSED    | Night       | CHICKEN_COOP_DOOR_CLOSED_AT_NIGHT_OK      | GREEN        |
| CLOSED    | Day         | CHICKEN_COOP_DOOR_CLOSED_IN_DAYTIME_ERROR | FLASHING_RED |
| OPEN      | Night       | CHICKEN_COOP_DOOR_OPEN_AT_NIGHT_ERROR     | FLASHING_RED |
| ERROR     | Day/Night   | CHICKEN_COOP_DOOR_SENSOR_FAILURE_ERROR    | FLASHING_RED |

## Component Roles and Responsibilities

### Coop Controller

The Coop Controller has three main responsibilities:
1. Reading the sensor.
2. Sending status messages to AWS.
3. Setting the local LED status.

It publishes status messages (`OPEN`, `CLOSED`, or `ERROR`) to the `coop/hardware/signal` MQTT topic and subscribes to the `coop/status` topic for status updates to control the LED.

### Coop Snooper

The Coop Snooper is a remote status reporter that subscribes to the MQTT topic conveying the current door status. It uses an RGB LED to display the status and does not need to be on the same Wi-Fi network as the Coop Controller.

### Amazon Web Services (AWS)

AWS is the primary platform for processing and decision-making. The services involved include:
- **IoT Core**: Defines IoT Things for both Coop Controller and Coop Snooper, and handles OTA updates.
- **Lambda Functions**: Responsible for state assessment, sunrise/sunset time retrieval, and OTA notifications.
- **DynamoDB**: Stores local sunrise/sunset times and the latest coop state.
- **S3 Buckets**: Holds OTA images for both Coop Controller and Coop Snooper.

## GitHub Repositories

The project consists of multiple repositories, each handling different aspects of the system:
- `C2DS-aws-lambda-coop-controller-handler`
- `C2DS-aws-lambda-query-state`
- `C2DS-aws-lambda-set-twilight-times`
- `C2DS-aws-lambda-upgrade-handler`
- `C2DS-esp32-coop-controller`
- `C2DS-esp32-coop-snooper`

This comprehensive system ensures the safety and security of your chickens by providing real-time status notifications and visual indicators, leveraging the power of IoT and cloud computing without requiring manual intervention or modifications to your existing setup.
