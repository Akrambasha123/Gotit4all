# 🚀 GotIt — Enterprise HR & Biometric Automation Platform

[![Website](https://img.shields.io/badge/Website-gotit4all.com-blue?style=for-the-badge&logo=google-chrome)](https://gotit4all.com/)
[![System Architecture](https://img.shields.io/badge/Architecture-Microservices-8A2BE2?style=for-the-badge)](#)
[![Tech Stack](https://img.shields.io/badge/Core-Laravel_|_Golang_|_Redis_|_WebSockets-FF2D20?style=for-the-badge&logo=laravel)](#)

**GotIt HR** is a high-throughput, enterprise-grade Human Resources and Biometric Attendance platform. Designed for immense scalability, it replaces fragmented HR systems with a unified, real-time data pipeline that handles everything from hardware-level biometric data extraction to automated payroll execution.

This repository serves as a technical showcase of the product's architecture, capabilities, and the complex engineering powering the platform.

---

## 🏗️ System Architecture & Server Infrastructure

GotIt is built on a highly optimized, decoupled microservices architecture designed to handle thousands of concurrent hardware connections and massive daily data pipelines.

### 1. The Golang ADMS Server (Hardware Layer)
To achieve zero-latency biometric processing, the standard HTTP layer was bypassed for hardware communication. 
*   **Custom Go Server:** I engineered a dedicated, highly concurrent **Go (Golang)** server that acts as the Automatic Data Master Server (ADMS). 
*   **Persistent Connections:** It maintains persistent TCP/IP connections with hundreds of physical biometric devices (Face/Fingerprint scanners) deployed across different geographical locations.
*   **High-Throughput Fetching:** The Go server listens for raw hexadecimal punch data in real-time, instantly decodes it, and pushes the payload into a high-speed Redis queue, ensuring the hardware is never bottlenecked by database locks.

### 2. The Laravel Core Engine (Business Logic Layer)
The primary backend is powered by **Laravel (PHP)**, operating as the brain of the HR ecosystem.
*   **Asynchronous Queue Processing:** Laravel daemon workers continuously consume the Redis queues populated by the Go server. They process raw punches, match them against complex shift rosters, and calculate late marks, half-days, or overtime asynchronously.
*   **Unified API Gateway:** Serves secure REST APIs for the frontend dashboards, mobile applications, and third-party integrations using strict JWT authentication.

### 3. Real-Time WebSocket Pipeline (Presentation Layer)
*   Instead of polling the database, the system uses WebSockets. When the Go server pushes a punch and Laravel validates it, an event is instantly broadcasted via WebSockets. HR Managers see employees clock in on their live dashboard the exact millisecond their finger touches the scanner.

---

## ✨ Pin-to-Pin Feature Breakdown

### 📍 The Unified Attendance Engine
GotIt doesn't just record time; it normalizes data from 5 completely different input streams into one cohesive timeline:
*   **Biometric (Face/Fingerprint):** Processed via the Go ADMS server.
*   **Geo-Location Check-in:** GPS coordinate extraction with strict radius geo-fencing and anti-spoofing (mock-location detection) algorithms.
*   **Dynamic QR Code:** Time-sensitive, encrypted QR codes that regenerate every few seconds to prevent proxy attendance.
*   **RFID/Smart Card:** Legacy access control integration.

### 💰 Automated Payroll & Compliance Engine
*   **Dynamic Rule Engine:** Custom formulas for LWP (Leave Without Pay), overtime multipliers, and tiered late-deductions.
*   **One-Click Processing:** Generates payroll for thousands of employees instantly by cross-referencing the normalized attendance timeline with approved leave balances.
*   **Automated Tax & Compliance:** Auto-calculates regional taxes, provident funds, and generates compliant, digital salary slips ready for download.

### 🔐 Enterprise Security & RBAC
*   **Granular Role-Based Access Control:** Strict permission matrices defining exactly what Admins, HR Managers, Branch Supervisors, and standard Employees can view or mutate.
*   **Data Encryption:** All biometric templates and sensitive employee PII (Personally Identifiable Information) are encrypted at rest. Network traffic between the Go server and biometric hardware is secured.
*   **Audit Trails:** Every configuration change, payroll generation, or manual attendance override is immutably logged with the IP, timestamp, and user ID.

### 📊 Employee Self-Service & Analytics
*   **Live HR Dashboards:** Visualized data on daily workforce strength, department-wise absenteeism, and real-time punch feeds.
*   **Self-Service Portal:** Employees can securely log in to request leaves (triggering hierarchical approval workflows), view their attendance history, and download payslips.

---

## 💡 Engineering Impact

By decoupling the biometric hardware communication (Golang) from the heavy business logic (Laravel), the **GotIt** platform achieved:
*   **100% Data Accuracy:** Zero dropped packets or lost punches during peak shift-change hours.
*   **Massive Scalability:** The Go server can scale horizontally to support thousands of simultaneous hardware devices using minimal CPU/Memory overhead.
*   **Instantaneous Feedback:** The WebSockets integration reduced dashboard data latency from minutes (traditional polling) to milliseconds.

---

### 📬 Get in Touch
Interested in learning more about the architecture or trying out the product? 
- **Website:** [gotit4all.com](https://gotit4all.com)
- **Contact:** info@gotit4all.com
- **Phone:** +91 96069 75900
