## Introduction

This document outlines the design and implementation of a passive sensor system for monitoring the status of a chicken coop door using an ESP32 microcontroller and Amazon Web Services (AWS). The system aims to provide real-time notifications about the door's open or closed status through text messages and visual indicators, ensuring the safety and security of the chickens without modifying the door mechanism or the chickens themselves.

### Ground Rules and Assumptions
- The chicken door open/close mechanism will remain unmodified.
- The sensor will be passive, not actively participating in the door's operation.
- No modifications to the chickens, such as chips or leg bracelets, will be made.

### Communication Methods
The sensor system will communicate the chicken door status in two primary ways:
1. **Text Alerts**: Sent under specific failure conditions, such as door closure failure at sunset or door open failure at sunrise.
2. **LED Indicator**: An LED in the coop window will illuminate red when the door is open and green when it is closed, providing a clear visual status at all times.

### Components
- **Controller**: An ESP32 microcontroller running micropython
- **Sensor**: Infrared Slotted Optical Optocoupler
- **Indicator**: 10mm LED bulb
- **AWS Account**: For handling logic and text messaging

### Status Notification Logic
The system employs a straightforward decision-making process managed by AWS:
- If the door is open more than 5 minutes after sunset, an alert is sent.
- If the door is closed more than 5 minutes after sunrise, an alert is sent.
- The LED indicator in the window provides a continuous visual status.

### Software Design
- **Controller**: The ESP32 reads the sensor and sends status messages every 7 minutes to AWS, also controlling the LED color.
- **AWS Processing**: AWS API Gateway and Lambda functions handle the logic and alerting:
  - **Sensing Lambda**: Interprets the door status and triggers alerts based on the time of day.
  - **Monitoring Lambda**: Ensures the Sensing Lambda is operational and sends alerts if it fails.

### Security and Costs
All communications will be encrypted, and sensitive information will be protected using git-crypt. The estimated monthly cost for AWS services is around $20, with all development work being volunteered.

### Potential Challenges
The project may face challenges such as text messaging limitations imposed by AWS and logistics of remote software changes. However, these are manageable with prior experience and documentation.

This project is open-source, with full documentation and code available for community contribution and long-term support.

