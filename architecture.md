# System Architecture – Mo-Tamu

> This document describes the **high-level system architecture** of the Mo-Tamu application.  
> Implementation details are intentionally abstracted due to client confidentiality.

---

## 1. Architecture Overview

Mo-Tamu is designed as a **multi-tier system** that integrates:
- An **Android mobile application**
- A **Python-based backend service**
- A **cloud database**
- A **web-based monitoring dashboard**

The system leverages **face recognition technology** to enhance residential security through real-time guest identification and monitoring.

---

## 2. High-Level Architecture Diagram (Conceptual)

```
+----------------------+
| Android App          |
| (Kotlin)             |
|                      |
| - Camera / Gallery   |
| - UI Interaction     |
| - Guest Input        |
+----------+-----------+
           |
           | HTTP / JSON
           v
+----------------------+
| Backend API          |
| (Python Flask)       |
|                      |
| - Image Processing   |
| - Face Detection     |
| - Face Recognition   |
+----------+-----------+
           |
           v
+----------------------+
| Firebase             |
| Realtime Database    |
|                      |
| - Guest Data         |
| - Visit Logs         |
| - Status Records     |
+----------+-----------+
           |
           v
+----------------------+
| Web Dashboard        |
|                      |
| - Monitoring         |
| - Statistics         |
| - Emergency Alert    |
+----------------------+
```


## 3. Mobile Application Layer (Android)

**Platform:** Android  
**Language:** Kotlin  

### Responsibilities:
- Capture guest images using camera or gallery
- Collect guest information (identity, purpose, visit type)
- Send image and metadata to backend API
- Display recognition results and confidence scores
- Provide emergency alert trigger

### Key Characteristics:
- Focused on user interaction and data presentation
- Does not perform heavy image processing locally
- Acts as a client for backend recognition services

---

## 4. Backend Service Layer

**Language:** Python  
**Framework:** Flask  

### Responsibilities:
- Receive image data from Android application
- Preprocess images (grayscale conversion, resizing)
- Perform face detection using **Haar Cascade Classifier**
- Execute face recognition using **Support Vector Machine (SVM)**
- Return prediction results and confidence values
- Handle guest data synchronization with database

### Design Considerations:
- Stateless API design
- JSON-based request and response
- Modular separation between detection and recognition logic

---

## 5. Face Recognition Pipeline

### Detection Phase:
- Haar Cascade Classifier used for detecting facial regions
- Converts input image to grayscale
- Extracts face region for further processing

### Recognition Phase:
- Face image flattened into numerical feature vector
- Classification performed using SVM model
- Output includes:
  - Predicted identity label
  - Confidence score

> The system prioritizes **accuracy and consistency** over lightweight inference.

---

## 6. Data Layer

**Technology:** Firebase Realtime Database  

### Stored Data (High-Level):
- Guest identity information
- Visit timestamps (check-in / check-out)
- Visit status (active / completed)
- Image reference URLs
- Emergency event logs

### Characteristics:
- Real-time data synchronization
- Centralized storage for mobile and web clients
- Enables live monitoring and reporting

---

## 7. Web Monitoring Dashboard

### Responsibilities:
- Display real-time guest activity
- Show visit statistics (daily, weekly, monthly)
- Receive emergency alerts from mobile application
- Provide structured security reports

### Integration:
- Reads data directly from Firebase
- Acts as a monitoring and visualization layer

---

## 8. Security & Confidentiality Considerations

- No credentials or API keys are stored in the mobile application repository
- Sensitive configuration files are excluded
- Communication between layers uses structured data formats
- Source code and model files are restricted to authorized environments

---

## 9. Architectural Strengths

- Clear separation of concerns between layers
- Scalable backend design
- Real-time monitoring capability
- Flexible integration between mobile, backend, and web systems

---

## 10. Notes

This architecture reflects a **real-world client implementation** focused on:
- Security
- Reliability
- Maintainability

Detailed implementation and optimization strategies are excluded for confidentiality.
