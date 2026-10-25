# Arduino Anti-Brute-Force Smart Lock System

An Arduino Uno–based electronic lock prototype that combines four-digit password authentication, keypad-pattern-based honeytrap detection, failed-attempt lockout, buzzer alerts, and automatic door relocking.

The project demonstrates how an embedded system can combine physical access control with simple intrusion-detection concepts. It is an educational prototype, not a certified security product.

## Features

### Password-based authentication

- Accepts a four-digit password through a 4×3 matrix keypad.
- Masks entered digits on the 16×2 LCD using `*` characters.
- Unlocks the door only when the entered password passes normal authentication.
- Relocks the door automatically after the configured unlock countdown.

### Pattern-based honeytrap detection

The honeytrap is based on a **spatial pattern on the keypad**, not on a separate hardcoded decoy password.

For example, consider a standard 4×3 keypad layout:

```text
1  2  3
4  5  6
7  8  9
*  0  #
```

If the valid password is `4562`, the input `7895` traces the same relative positions shifted down by one row:

- `4 → 7`: same column, one row down
- `5 → 8`: same column, one row down
- `6 → 9`: same column, one row down
- `2 → 5`: same column, one row down

When the system recognises this configured pattern as a honeytrap attempt, it:

1. Displays `ACCESS GRANTED` as deceptive feedback on the LCD.
2. **Keeps the physical door locked**; the honeytrap response must not issue an unlock command to the servo.
3. Sounds the buzzer for several beeps, then stops the alert.

The displayed message is deliberately misleading. It does **not** mean that authentication succeeded or that the door opened.

### Anti-brute-force lockout

- Tracks ordinary incorrect password attempts.
- Activates a lockout after **two failed attempts**.
- Keeps password entry locked out for **30 seconds**.
- Uses the LCD to communicate the lockout state and the buzzer to provide audible warnings where implemented.

The honeytrap response and ordinary failed-password lockout are different behaviors: the honeytrap produces deceptive LCD feedback and a short alert, while repeated ordinary incorrect attempts trigger the timed lockout.

### Password change

The system supports changing the password after the user verifies the current password. This helps prevent someone from changing the configured password without first passing the existing authentication check.

### LCD status messages

The 16×2 LCD provides password-entry prompts, masked input, authentication feedback, countdown information, and security or lockout messages. The exact wording depends on the messages defined in the Arduino sketch.

## Hardware components

| Component | Quantity | Purpose |
|---|---:|---|
| Arduino Uno | 1 | Main controller |
| 4×3 matrix keypad | 1 | Password entry |
| 16×2 LCD | 1 | Prompts and status messages |
| Servo motor | 1 | Demonstrates locking and unlocking |
| Buzzer | 1 | Audible security alert |
| Breadboard | 1 | Prototyping connections |
| Jumper wires | As required | Electrical connections |
| USB cable | 1 | Programming and USB power for the Arduino |

## Wiring

Use the following pin assignments to match the project configuration.

### 4×3 matrix keypad

| Keypad connection | Arduino Uno pin |
|---|---:|
| R1 | D1 |
| R2 | D2 |
| R3 | D3 |
| R4 | D4 |
| C1 | D5 |
| C2 | D6 |
| C3 | D7 |

### Other components

| Component connection | Arduino Uno pin |
|---|---:|
| Buzzer signal | D8 |
| Servo signal | D9 |
| LCD RS | A0 |
| LCD E | A1 |
| LCD D4 | A2 |
| LCD D5 | A3 |
| LCD D6 | A4 |
| LCD D7 | A5 |

Connect the components' power and ground pins according to their specifications and the wiring diagram. Check the pin labels on your actual modules before powering the circuit.

> **Important pin note:** Arduino Uno pin D1 is also the hardware UART TX pin. Using it for keypad row R1 can interfere with serial communication and may complicate uploading or debugging. If you encounter these issues, move R1 to an unused digital pin and update the keypad pin definition in the sketch to match the physical wiring.

> **Servo power note:** A servo can draw more current than the Arduino's onboard 5 V supply can reliably provide. If the Arduino resets, the LCD flickers, or the servo behaves erratically, use a suitable regulated supply for the servo and connect its ground to Arduino ground. Follow the servo's voltage and current requirements.

Refer to [`wiring_diagram.png`](wiring_diagram.png) for the project wiring illustration, if that file is included in the repository.

## How it works

### Normal successful entry

1. The user enters four digits on the keypad.
2. The LCD masks the digits as they are entered.
3. The sketch checks the entered password.
4. If the password is correct, the servo moves to the configured unlocked position.
5. The system displays the access status and countdown, then commands the servo back to the locked position when the countdown ends.

### Ordinary incorrect entry

1. The user enters an incorrect password that does not match the honeytrap pattern.
2. The failed-attempt counter increases.
3. After two failed attempts, the system activates a 30-second lockout.
4. During lockout, the system restricts further password entry until the timeout has elapsed.

### Honeytrap-pattern entry

1. The user enters digits that match the configured keypad pattern associated with the valid password, but with different numbers—for example, `7895` for a valid password of `4562` on the standard keypad layout shown above.
2. The LCD displays `ACCESS GRANTED` as deceptive feedback.
3. The servo must remain in the locked position; no physical access is granted.
4. The buzzer sounds for several beeps and then stops.

### Password change

1. The user starts the password-change flow.
2. The current password must be verified.
3. The user follows the on-screen prompts to set a replacement password.

The exact button sequence and any additional validation rules are determined by the sketch.

## Software requirements

- [Arduino IDE](https://www.arduino.cc/en/software)
- Arduino Uno board support in the IDE
- `Keypad` library
- `LiquidCrystal` library (included with the standard Arduino AVR platform)
- `Servo` library (included with the standard Arduino AVR platform)

## Project structure

```text
arduino-anti-bruteforce-smart-lock/
├── Arduino_Code/
│   └── AntiBruteForceLock.ino
├── smart_lock_arch.jpg
├── smart_lock_honeytrap.jpg
├── smart_lock_pwd_change.jpg
├── wiring_diagram.png
└── README.md
```

The image files are documentation assets. Their presence depends on which files have been committed to the repository.

## Setup and upload

1. Connect the components using the wiring tables and diagram.
2. Install the Arduino IDE and the `Keypad` library.
3. Open `Arduino_Code/AntiBruteForceLock.ino`.
4. In the Arduino IDE, select **Arduino Uno** under **Tools → Board** and select the correct port under **Tools → Port**.
5. Confirm that the keypad pin definitions in the sketch match the physical wiring.
6. Verify the sketch, then upload it to the Arduino Uno.
7. Test the normal-password, incorrect-password, honeytrap-pattern, lockout, password-change, and automatic-relocking flows before connecting the mechanism to a real door.

## Suggested validation checklist

Use these checks to verify that the physical behavior matches the documentation:

- [ ] Password characters are masked on the LCD.
- [ ] The correct password unlocks the servo-controlled mechanism.
- [ ] The mechanism automatically returns to the locked position after the configured countdown.
- [ ] An ordinary incorrect password increments the failed-attempt counter.
- [ ] The second ordinary failed attempt activates a 30-second lockout.
- [ ] The example honeytrap pattern (valid password `4562`, pattern input `7895`) is recognised when using the standard keypad layout.
- [ ] The honeytrap response displays `ACCESS GRANTED`, sounds a short alert, and **does not unlock the door**.
- [ ] Password changes require verification of the current password.
- [ ] Normal input works again after the lockout timeout expires.

Do not treat these checks as passed until they have been tested on the actual circuit and sketch.

## Security concepts demonstrated

- Password-based authentication
- Physical access control
- Failed-attempt tracking and timed lockout
- Keypad-pattern-based suspicious-input detection
- Deceptive feedback as a honeytrap-style mechanism
- Audible intrusion alerts
- Embedded hardware/software integration

## Limitations and safety notes

- This is a learning prototype; it has not been represented as a certified or production-grade lock.
- The password is only four digits, so the design should not be used as the sole security control for a real property.
- A misleading LCD message is not, by itself, a security control. The servo must stay locked during a honeytrap response, and this behavior must be verified by testing.
- The pattern rule shown here assumes the standard physical layout of a 4×3 keypad. A different keypad layout or a different pattern-matching implementation may change which inputs are classified as honeytrap attempts.
- Physical protection also depends on the servo linkage, power supply, enclosure, and the mechanical strength of the lock assembly.
- The behavior described here should be kept consistent with the actual contents of `AntiBruteForceLock.ino`. Do not claim behavior that has not been implemented and tested.

## Future improvements

Possible extensions include RFID or fingerprint authentication, mobile or IoT monitoring, remote notifications, and event logging. These are potential improvements, not features claimed as currently implemented.

## Author

**Kavyansh Goenka**  
Computer Engineering Student | Embedded Systems & Security Enthusiast
