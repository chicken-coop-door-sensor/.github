# Introduction to the Chicken Coop Door Sensor System

Ensuring the safety and security of chickens in a coop requires constant vigilance, particularly when it comes to the opening and closing of the coop door. An open door at night can expose chickens to predators, while a closed door during the day can restrict their movement. To address these concerns, we have developed a passive sensor system for monitoring the status of the FarmLite Chicken Coop Door. This system leverages modern technology to provide real-time notifications and visual indicators about the door's status without modifying the door mechanism or the chickens themselves.

## Objectives

The primary objective of the Chicken Coop Door Sensor system is to enhance the security of the chicken coop through automated monitoring. The system aims to:
1. Provide real-time status updates on the coop door.
2. Send notifications in the event of a failure to open or close the door at the appropriate times.
3. Maintain a simple and non-intrusive design that does not alter the existing door mechanism or require any modifications to the chickens.

## Environmental and Software Assumptions

To ensure the system operates effectively, the following assumptions are made:
- The chicken coop door's existing open/close mechanism will remain unchanged.
- The sensor system will be passive, meaning it will not actively control the door but only monitor its status.
- No modifications to the chickens, such as chips or leg bracelets, will be made.
- All times will be based on the `America/Chicago` (US Central Time) timezone.
- Latitude 35.4834 and Longitude -86.4603 (Shelbyville, TN) will be used for sunrise and sunset calculations.

## System Components

The system comprises the following key components:
- **ESP32 Controller**: A microcontroller running Micropython, responsible for reading the sensor, sending status messages, and controlling the local LED indicator.
- **Infrared Slotted Optical Optocoupler**: A sensor used to detect the open or closed status of the door.
- **10mm LED Bulb**: An indicator light installed in the coop window to visually display the door status.
- **Amazon Web Services (AWS) Account**: Utilized for handling the logic and text messaging through Lambda functions and other AWS services.

## Status Notification Logic

The system communicates the status of the chicken coop door through two main channels:
- **Text Alerts**: Text messages are sent under specific failure conditions, such as if the door fails to close at sunset or fails to open at sunrise.
- **LED Indicator**: An LED light in the coop window provides a visual status indicator. The LED is always illuminated, showing red when the door is open and green when it is closed. If the light is off, it indicates a malfunction in the system.

## Controller’s Role

The ESP32 controller has three primary responsibilities:
1. **Reading the Sensor**: Continuously monitors the status of the coop door.
2. **Sending Status Messages**: Reports the door status as either "open" or "closed" approximately every 45 minutes via an HTTP REST API message.
3. **Setting the Local LED**: Controls the LED indicator to reflect the current door status.

## AWS Role

AWS handles the majority of the system's processing tasks:
- **Sensing Lambda**: This function processes incoming status messages and determines if alerts need to be sent based on the time of day and the door status.
- **Twilight Lambda**: Queries the US Naval Observatory Clock API daily to retrieve and store sunrise and sunset times in a DynamoDB table.
- **Monitoring Lambda**: Monitors the execution of the other Lambda functions to ensure they are running error-free and sends alerts if any issues are detected.

## Conclusion

The Chicken Coop Door Sensor system is a robust and reliable solution for monitoring the status of a FarmLite Chicken Coop Door. By providing real-time notifications and visual indicators, it enhances the security and safety of the chicken coop, ensuring peace of mind for chicken owners. This system exemplifies the integration of modern technology with traditional farming practices, offering an innovative approach to livestock management.
