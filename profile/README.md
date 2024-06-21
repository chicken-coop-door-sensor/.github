## Introduction to the Chicken Coop Door Sensor System

The FarmLite Chicken Coop Door system is a widely recognized and reliable solution for automatic chicken coop door management, providing an efficient and hands-free approach to ensuring the safety and convenience of managing a chicken coop. However, while the FarmLite system excels in automating the opening and closing of the coop door, there is a need for an additional layer of passive monitoring to further enhance the safety and security of the chickens. This project aims to supplement, not replace, the FarmLite system by introducing a passive sensor-based monitoring facility that operates independently of the door's mechanical functions.

This supplementary system will utilize an ESP32 controller and Amazon Web Services (AWS) to monitor the status of the chicken coop door, providing real-time notifications and visual indicators to ensure the door is functioning correctly. By integrating an Infrared Slotted Optical Optocoupler sensor and an LED indicator, this project aims to deliver timely alerts via text messages under failure conditions, such as the door failing to close at sunset or open at sunrise. Additionally, the LED indicator will provide a clear, always-on visual status of the door, switching between red and green to denote open and closed states respectively.

Importantly, this project respects the integrity of the FarmLite system by maintaining a completely passive approach. It does not interfere with the door mechanism or the chickens themselves, ensuring that the original functionality of the FarmLite system remains unchanged. Instead, it provides an added layer of assurance and monitoring, leveraging the power of IoT (Internet of Things) technology and cloud-based services to deliver enhanced security and peace of mind for chicken coop management.
### Purpose and Scope

The primary goal of this project is to create a passive sensor system that accurately detects and reports the status of the FarmLite Chicken Coop Door. By using a combination of local indicators and remote notifications, the system ensures that users are promptly informed of any issues related to the door’s operation. The system is designed to be non-intrusive, requiring no modifications to the existing door mechanism or the chickens themselves. 

### Key Features

1. **Real-Time Status Monitoring**: The system continuously monitors the door's open or closed status using an infrared slotted optical optocoupler sensor connected to an ESP32 microcontroller.
2. **Visual Indicators**: A 10mm LED bulb installed in the coop window provides a clear visual status of the door. The LED is green when the door is closed and red when the door is open. If the LED is off, it indicates a malfunction in the system.
3. **Automated Text Alerts**: AWS Lambda functions handle decision-making processes, sending text alerts to the user in case of any failures. Alerts are sent if the door remains open past sunset or if it fails to open by sunrise.
4. **Cloud-Based Processing**: The ESP32 controller sends status updates to AWS, where Lambda functions process the data, compare it with local sunrise and sunset times, and trigger alerts as necessary.
5. **Sunrise and Sunset Calculations**: The system uses the US Naval Observatory Clock API to retrieve accurate daily sunrise and sunset times based on the geographical coordinates of Shelbyville, TN.

### Environmental and Software Assumptions

- The chicken coop door mechanism remains unmodified.
- The sensor operates passively, without influencing the door’s movement.
- No modifications are made to the chickens, such as installing chips or leg bracelets.
- The system operates in the US Central Time zone, specifically for Shelbyville, TN (Latitude 35.4834, Longitude -86.4603).
- The ESP32 microcontroller reports only the door status.
- AWS handles the bulk of the logic and text messaging services.

### System Components

- **ESP32 Microcontroller**: Acts as the controller, reading sensor data, sending status messages, and controlling the LED indicator.
- **Infrared Slotted Optical Optocoupler**: Detects the door's open or closed status.
- **10mm LED Bulb**: Provides a visual indication of the door's status.
- **Amazon Web Services (AWS)**: Manages data processing, decision making, and alerting through Lambda functions and DynamoDB.

### Status Notification Logic

The ESP32 sends status updates every 45 minutes to ensure continuous monitoring and act as a keep-alive signal. AWS Lambda functions process these updates and compare them with the calculated sunrise and sunset times. Alerts are sent if the door’s status is not as expected. The LED in the coop window provides a constant visual status update, ensuring immediate local awareness of the door’s condition.

### Conclusion

The FarmLite Automatic Chicken Coop Door Sensor System is a comprehensive solution for monitoring the status of a chicken coop door. By combining robust hardware components with advanced cloud-based processing, it ensures the safety and security of the chickens through timely notifications and clear visual indicators. This system provides peace of mind to users, knowing that their chickens are protected without the need for constant manual checks.
