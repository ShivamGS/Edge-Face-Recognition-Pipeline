# 🛰️ Edge-Based Face Recognition System (AWS IoT + Lambda + SQS)

This repository presents a **hybrid edge-cloud face recognition pipeline** using **AWS IoT Greengrass**, **Lambda**, and **SQS**. It simulates IoT edge devices performing **on-device face detection** with cloud-triggered recognition. Built with **low-latency processing** in mind, the system enables real-time inference using a distributed, event-driven architecture.

Designed as a prototype for **smart surveillance and IoT use cases**, the project demonstrates key concepts in **edge computing**, **serverless ML**, and **asynchronous cloud messaging**.

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

```text
[IoT Client Device (EC2)] 
       │
       ▼
 Publishes MQTT Message
(topic: clients/<id>-IoTThing)

[Greengrass Core (EC2)]
  ├── Face Detection (MTCNN)
  └── Sends detected faces to ➤ [SQS Request Queue]
                                      │
                                      ▼
                     [Lambda Function: Face Recognition (FaceNet)]
                                      │
                                      ▼
                        [SQS Response Queue → IoT Client]


## 📂 Project Components

- **Edge Device (Greengrass Core)**: Deployed MTCNN-based face detection as a custom Greengrass component.
- **Client Simulator (EC2)**: Published base64-encoded image frames via MQTT to simulate IoT camera input.
- **Cloud Backend (Lambda)**: Performed identity recognition using FaceNet, triggered asynchronously via SQS.
- **Messaging Layer**: Used SQS for decoupling edge detection from cloud classification and result delivery.

## 🚀 System Workflow

1. **Client device** sends image frames via MQTT to AWS IoT Core.
2. **Greengrass Core** detects faces using MTCNN and pushes valid detections to an SQS request queue.
3. **AWS Lambda** is triggered by SQS, performs FaceNet-based recognition, and places results on an SQS response queue.
4. **Client device** receives the classification output from the response queue.

