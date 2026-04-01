# Touch-module
This code is used for using the TTP223 touch module with an Arduino UNO R4 MINIMA. By Aryan Builds.
# TTP223 Touch Sensor — Arduino R4 Minima

A beginner-friendly exploration of the TTP223 capacitive touch module using the Arduino UNO R4 Minima. Touch the pad, trigger an action — as simple as that!

---

## Hardware Required

| Component | Quantity |
|---|---|
| Arduino UNO R4 Minima | 1 |
| TTP223 Touch Sensor Module | 1 |
| LED (any colour) | 1 |
| 220Ω resistor | 1 |
| Jumper wires | 5 |

---

## Wiring

### TTP223 Touch Sensor

| TTP223 Pin | Arduino R4 Minima Pin |
|---|---|
| VCC | 5V or 3.3V |
| GND | GND |
| I/O | Digital Pin 7 |

> The TTP223 works on both 3.3V and 5V — either power pin on the R4 Minima is fine.

### External LED

| Component | Connection |
|---|---|
| LED long leg (anode) | Digital Pin 6 via 220Ω resistor |
| LED short leg (cathode) | GND |

---

## How It Works

The TTP223 is a capacitive touch sensor. It detects a change in capacitance when a finger comes close to or touches the metal pad on the module.

- **Output HIGH** → finger is touching the pad
- **Output LOW** → not touched

It can also sense through thin materials like paper or plastic, making it great for hidden touch buttons!

The module has two solder pads on the back (A and B) that can change the behaviour to toggle mode or active-low mode, but the default mode works perfectly for most projects.

---

## Sketches

### 1. Single Flash Per Tap — External LED

Each tap gives a single short flash on the external LED. Uses edge detection so holding your finger down only counts as one tap.

```cpp
const int TOUCH_PIN = 7;
const int LED_PIN   = 6;  // external LED with 220Ω resistor to GND

int lastState = LOW;

void setup() {
  pinMode(TOUCH_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int currentState = digitalRead(TOUCH_PIN);

  if (currentState == HIGH && lastState == LOW) {
    // Single flash on each tap
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
    Serial.println("Touched!");
  }

  lastState = currentState;
  delay(50); // debounce
}
```

Open the **Serial Monitor** at 9600 baud to see "Touched!" printed on every tap.

### 2. Count Touches

Counts each individual tap using edge detection — holding your finger down only counts as one touch.

```cpp
const int TOUCH_PIN = 7;
const int LED_PIN   = 6;

int lastState  = LOW;
int touchCount = 0;

void setup() {
  pinMode(TOUCH_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int currentState = digitalRead(TOUCH_PIN);

  if (currentState == HIGH && lastState == LOW) {
    touchCount++;
    // Flash LED once to confirm the tap
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
    Serial.print("Touches: ");
    Serial.println(touchCount);
  }

  lastState = currentState;
  delay(50); // debounce
}
```

Open the **Serial Monitor** at 9600 baud to see the count.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| LED does not flash | Check the LED is the right way around — long leg to pin 6, short leg to GND |
| LED always on | Check the I/O wire is connected to pin 7 and the TTP223 has power |
| Nothing happens when touched | Check that I/O is connected to the correct digital pin and that VCC/GND are connected |
| Multiple flashes per tap | Increase the `delay()` debounce value (try 80–100ms) |
| Board not uploading | Double-tap the reset button on the R4 Minima to enter bootloader mode, then upload |

---

## Ideas to Try Next

- Toggle the LED on and off with each tap instead of holding
- Combine with a dot matrix display to show the touch count
- Use as a hidden button through a cardboard or plastic surface
- Play a tone with `tone()` on each touch
- Add a buzzer alongside the LED for audio + visual feedback

---

## License

MIT License — free to use, modify, and share.
