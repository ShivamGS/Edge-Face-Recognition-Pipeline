# 🛰️ Edge-Based Face Recognition System (AWS IoT + Lambda + SQS)

This repository presents a **hybrid edge-cloud face recognition pipeline** using **AWS IoT Greengrass**, **Lambda**, and **SQS**. It simulates IoT edge devices performing **on-device face detection** with cloud-triggered recognition. Built with **low-latency processing** in mind, the system enables real-time inference using a distributed, event-driven architecture.

Designed as a prototype for **smart surveillance and IoT use cases**, the project demonstrates key concepts in **edge computing**, **serverless ML**, and **asynchronous cloud messaging**.


> 🔐 **Note:** This repository contains only documentation and system architecture.  
> The source code is withheld in accordance with academic integrity policies.
---

## 🔧 Project Overview

| Component                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| 🧊 IoT Core (EC2 + Greengrass) | Runs Greengrass Core and performs MTCNN-based face detection at the edge     |
| 📡 IoT Client Device (EC2)      | Simulated device publishing image frames over **MQTT**                      |
| 🧠 Lambda: Face Recognition     | Triggered by **SQS**, performs identity classification using **FaceNet**     |
| 📨 SQS (Request/Response)      | Manages message flow between edge and cloud, enabling decoupled execution   |
| ☁️ S3                          | (Optional) Cloud storage for intermediate results and logs                   |

---

## 🏗 Architecture

```
[Client Device (EC2)]
       │
       ├── Publishes video frames via MQTT
       ▼
[Core Device (Greengrass EC2)]
       │
       ├── Performs face detection (MTCNN)
       ▼
[Request Queue (SQS)]
       │
       ├── Triggers Lambda for recognition
       ▼
[Face Recognition (Lambda with FaceNet)]
       │
       ├── Sends name result
       ▼
[Response Queue (SQS)]
       │
       └── Returns result to Client Device
```
---

## 📂 Project Components

- **Edge Device (Greengrass Core)**: Deployed MTCNN-based face detection as a custom Greengrass component.
- **Client Simulator (EC2)**: Published base64-encoded image frames via MQTT to simulate IoT camera input.
- **Cloud Backend (Lambda)**: Performed identity recognition using FaceNet, triggered asynchronously via SQS.
- **Messaging Layer**: Used SQS for decoupling edge detection from cloud classification and result delivery.

---

## 🚀 System Workflow

1. **Client device** sends image frames via MQTT to AWS IoT Core.
2. **Greengrass Core** detects faces using MTCNN and pushes valid detections to an SQS request queue.
3. **AWS Lambda** is triggered by SQS, performs FaceNet-based recognition, and places results on an SQS response queue.
4. **Client device** receives the classification output from the response queue.

---
## 📌 Technical Highlights

🧠 MTCNN-based face detection at the edge – Lightweight, real-time detection on IoT clients using Greengrass components.

⚡ Cloud-hosted Lambda function with FaceNet – Identity recognition offloaded to the cloud via event-driven architecture.

🛰️ MQTT-based messaging via AWS IoT Core – Reliable, low-latency device-to-cloud communication using standard IoT protocols.

🔁 SQS for queue-based orchestration – Decouples edge and cloud components with asynchronous message handling.

📈 <1s end-to-end latency – Achieved consistently across 100 sample inputs under test conditions.

---
## 🔒 Security Measures

🔐 IAM Roles with Least Privilege – Scoped access for Lambda, Greengrass, and SQS operations.

📜 TLS Certificate Authentication – Secure device-level communication through AWS IoT Core.

🧪 Input Validation – Enforced rules for file types, queue/topic names, and payload structure.

🌍 Region-Scoped Resources – All services confined to a single AWS region to optimize latency and cost.

---

## 📝 Summary

- This project demonstrates an event-driven, modular architecture for edge-based facial recognition using AWS services. It reflects strong practical experience in:

- Edge inference deployment using AWS IoT Greengrass

- Serverless ML integration with AWS Lambda and FaceNet

- MQTT-based IoT messaging

- Asynchronous task management with SQS

- Cloud-IoT pipeline design for real-world, low-latency systems


