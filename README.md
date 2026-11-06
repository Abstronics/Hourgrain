# Hourgrain

**A screen-free digital hourglass focus timer for students.**

Two 8x8 LED matrices drop pixel "sand" one grain at a time. Flip the device and an MPU6050 motion sensor reverses the flow. A buzzer sounds when time is up.

![Hourgrain demo](docs/demo.gif)

> Built for [Hackathon name] on Devpost. Devpost submission: [link]

---

## Why Hourgrain

A phone is the biggest distraction on a student's desk, and a phone timer pulls the student back to the phone. Hourgrain is a timer with no apps, no notifications and nothing to scroll. You flip it, you work, and the buzzer tells you when to stop.

## Features

- Sand animation on two 8x8 LED matrices arranged as a diamond, tip to tip, like a real hourglass
- 64 grains per session, one grain dropped at a fixed interval
- Flip detection with an MPU6050, so the sand reverses when the device is turned over
- Buzzer alert when the last grain lands
- Adjustable session length: change one value and the per-grain interval is recalculated

## How it works

1. The top matrix starts full (64 pixels lit).
2. Every interval, the top-most pixel of the top matrix is removed and a grain falls through the neck and lands in the bottom matrix.
3. The MPU6050 reads acceleration. The Z axis runs along the vertical of the device, so its sign tells the device which end is up.
4. When the device is flipped, the roles of the two matrices swap and the sand flows the other way.
5. When all 64 grains have moved, the buzzer sounds.

## Hardware

| Part | Qty | Notes |
|---|---|---|
| Arduino Nano | 1 | Main controller |
| 8x8 LED matrix with MAX7219 driver | 2 | Chained together |
| MPU6050 accelerometer/gyro module | 1 | Orientation sensing |
| Buzzer | 1 | [active or passive] |
| Power source | 1 | [USB / battery] |
| 3D-printed enclosure | 1 | 

## Wiring

**LED matrices (MAX7219, chained)**

| Matrix pin | Arduino Nano pin |
|---|---|
| DIN | 11 |
| CLK | 13 |
| CS | 10 |
| VCC | 5V |
| GND | GND |

**MPU6050 (I2C)**

| MPU6050 pin | Arduino Nano pin |
|---|---|
| SDA | A4 |
| SCL | A5 |
| VCC | [3.3V or 5V, depending on your module] |
| GND | GND |

## Software

**Libraries**

| Library | Version |
|---|---|
| LedControl | [version] |
| Adafruit MPU6050 | [version] |
| Adafruit Unified Sensor | [version] |

**Upload**

1. Install the libraries above in the Arduino IDE.
2. Open `hourgrain/hourgrain.ino`.
3. Select **Arduino Nano** and the correct processor and port.
4. Upload.

## Setting the session time

Each matrix has 8 x 8 = 64 pixels, so every session moves 64 grains.

```
time per grain = total session time / 64
```

In the code:

```
grainInterval_ms = (minutes * 60 * 1000) / 64
```

Change the `minutes` value [name of the variable in your sketch] and the interval is recalculated. Nothing else needs to change.

| Session | Total (s) | Time per grain |
|---|---|---|
| 5 min | 300 | 4.69 s |
| 10 min | 600 | 9.38 s |
| 25 min | 1,500 | 23.44 s |
| 45 min | 2,700 | 42.19 s |
| 60 min | 3,600 | 56.25 s |

The fall animation takes a fixed time, and the device waits out the rest of the interval before the next grain.

**Accuracy:** the Nano's clock is a ceramic resonator, not a precise crystal, so some drift is expected. [Add your measured error here, for example: a 25-minute session measured X min Y s on a phone stopwatch.]

## Orientation sensing

Measured MPU6050 readings (m/s^2):

| Position | Z | X | Y |
|---|---|---|---|
| Upright | about -11.5 | about +1.0 | about +0.2 |
| Flipped | about +8.0 | about -0.8 | about +0.1 |

The magnitudes are not a clean 9.8 on either side, so the flip threshold is based on the measured sign change of Z and not on theoretical gravity.

## Enclosure

The case is 3D printed. Files are in `docs/`.

- Material: [PLA / PETG]
- Color: [color]
- Parts: [number of STL files]
- Print settings: [layer height, infill, wall thickness]

## Challenges

- **Diamond layout:** rotating the matrices 45 degrees meant remapping pixel coordinates so grains fall tip to tip. The rotation values were calibrated by trial until the sand moved the right way.
- **Sand feel:** the fall had to look like sand and not a blinking dot, so the timing was tuned by eye.
- **Axis and thresholds:** finding which MPU6050 axis is vertical and setting a flip threshold from real readings.
- **Flip behavior:** the sand in the new top matrix should fall toward the neck without jumping.
- [Add your real problems here: wiring, power, case fit, buzzer noise.]

## What I learned

- Driving LED matrices with LedControl and handling matrix orientation
- Reading an accelerometer and turning raw values into a reliable flip signal
- Turning a real-world behavior into timing math and animation logic
- Designing hardware and code together, because case size and sensor placement affected the software

## Limitations

- Timing accuracy is limited by the Nano's clock.
- [Session length is set in code / by button: state how it works today.]
- [Battery and charging status.]

## Future work

- Preset session lengths selected with [button / tilt]
- Battery power and charging
- Final printed enclosure

## Author

Okunlola Abdulbasit, Mechatronics Engineering, Bells University of Technology, Nigeria.


## License

MIT. See `LICENSE`.
