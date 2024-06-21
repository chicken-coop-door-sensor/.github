## Introduction to the FarmLite Automatic Chicken Coop Door Sensor System

Ensuring the safety and security of chickens in a coop involves meticulous attention to the status of the coop door. An open door at night exposes chickens to potential predators, while a closed door during the day can disrupt their natural behavior. The FarmLite Automatic Chicken Coop Door Sensor System is designed to address these concerns by providing real-time monitoring and notifications of the door’s status. This system leverages an ESP32 microcontroller, infrared sensors, LEDs, and Amazon Web Services (AWS) to deliver a reliable and efficient solution without modifying the door mechanism or the chickens.

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
