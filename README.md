# 🔐 Arduino Anti-Brute-Force Smart Lock System

A secure smart lock system built using **Arduino Uno**, **Keypad**, **Servo Motor**, **LCD Display**, and **Buzzer** that protects against unauthorized access through password authentication, brute-force attack prevention, and a unique honeytrap security mechanism.

This project demonstrates how embedded systems can be used to implement real-world access control and security concepts using low-cost hardware components.

---

## 🚀 Features

### 🔑 Password-Based Authentication
- Secure 4-digit password entry using a matrix keypad.
- Password masking using `*` characters on the LCD display.
- Access granted only after correct password verification.

### 🛡️ Anti-Brute-Force Protection
- Tracks incorrect password attempts.
- Limits the number of failed login attempts.
- Automatically locks the system after repeated failures.
- Prevents continuous password guessing attacks.

### 🍯 Honeytrap Detection System
- Includes a hidden decoy password.
- Appears to grant access but secretly triggers a security alert.
- Inspired by cybersecurity concepts such as honeypots and honeytokens.

### 🚨 Intrusion Alert Mechanism
- Activates buzzer alarm during suspicious activity.
- Security alert mode triggered after excessive failed attempts.
- Audible warning system for unauthorized access attempts.

### 🔄 Password Management
- Allows users to change the password securely.
- Requires verification of the current password before modification.

### 🚪 Automatic Door Control
- Servo motor unlocks the door after successful authentication.
- Automatic relocking after a countdown period.

### 📟 LCD User Interface
- Displays system status and user prompts.
- Shows authentication results and security notifications.

---

## 🧰 Components Used

| Component | Quantity |
|------------|----------|
| Arduino Uno | 1 |
| 4x3 Matrix Keypad | 1 |
| 16x2 LCD Display | 1 |
| Servo Motor | 1 |
| Buzzer | 1 |
| Breadboard | 1 |
| Jumper Wires | Multiple |
| USB Cable | 1 |

---

## ⚙️ Working Principle

### Normal Operation

1. User enters a 4-digit password.
2. System verifies the entered password.
3. If correct:
   - Door unlocks.
   - Access granted message is displayed.
   - Countdown begins.
   - Door automatically relocks.

### Failed Authentication

1. User enters an incorrect password.
2. Failed attempt counter increases.
3. Warning message is displayed.
4. After multiple failed attempts:
   - Alarm is activated.
   - System enters lockout mode.

### Honeytrap Mode

1. User enters the predefined honeytrap password.
2. System displays:
   ```
   ACCESS GRANTED
   ```
3. Security alert is secretly triggered.
4. Alarm activates to indicate suspicious activity.

---

## 📂 Project Structure

```text
arduino-anti-bruteforce-smart-lock/
│
├── Arduino_Code/
│   └── AntiBruteForceLock.ino
│
├
│── smart_lock_arch.jpg
│   ├── smart_lock_honeytrap.jpg
│   ├── smart_lock_pwd_change.jpg
│  
│── wiring_diagram.png
│
├── README.md

---

## 📸 Project Demonstration

### Password Entry

```text
ENTER PASSWORD
****
```

### Successful Authentication

```text
ACCESS GRANTED
```

### Door Relocking

```text
RELOCK IN
5
```

### Security Alert

```text
SECURITY ALERT
```

### System Lockout

```text
SYSTEM LOCKED
WAIT 30 SECS
```

---

## 🔌 Hardware Connections

### Keypad

| Keypad Pin | Arduino Pin |
|------------|------------|
| R1 | 1 |
| R2 | 2 |
| R3 | 3 |
| R4 | 4 |
| C1 | 5 |
| C2 | 6 |
| C3 | 7 |

### Buzzer

| Component | Arduino Pin |
|------------|------------|
| Buzzer | 8 |

### Servo Motor

| Component | Arduino Pin |
|------------|------------|
| Servo Signal | 9 |

### LCD Display

| LCD Pin | Arduino Pin |
|----------|------------|
| RS | A0 |
| E | A1 |
| D4 | A2 |
| D5 | A3 |
| D6 | A4 |
| D7 | A5 |

---

## 💻 Software Requirements

- Arduino IDE
- Keypad Library
- LiquidCrystal Library
- Servo Library

---

## ▶️ Installation

1. Clone the repository.

```bash
git clone https://github.com/yourusername/arduino-anti-bruteforce-smart-lock.git
```

2. Open:

```text
AntiBruteForceLock.ino
```

in Arduino IDE.

3. Install required libraries if needed.

4. Connect the hardware components.

5. Upload the code to Arduino Uno.

6. Power the system and test.

---

## 🧠 Security Concepts Demonstrated

- Password Authentication
- Access Control Systems
- Brute-Force Attack Prevention
- Honeytrap Security Mechanisms
- Intrusion Detection
- Embedded Security Design
- Physical Security Systems

---

## 🔮 Future Improvements

- RFID Integration
- Fingerprint Authentication
- Mobile App Control
- GSM Alert Notifications
- OTP-Based Authentication
- Wi-Fi Monitoring Dashboard
- Event Logging System
- IoT Integration using ESP32

---

## 🎯 Learning Outcomes

This project helped in understanding:

- Embedded Systems Programming
- Arduino Development
- Hardware-Software Integration
- Access Control Mechanisms
- Security System Design
- Sensor and Actuator Interfacing
- Real-World Cybersecurity Concepts

---

## 👨‍💻 Author

**Kavyansh Goenka**

Computer Science Student | Embedded Systems & Security Enthusiast

---

## ⭐ Support

If you found this project useful:

- Star the repository ⭐
- Fork the project 🍴
- Share it with others 🚀

---
