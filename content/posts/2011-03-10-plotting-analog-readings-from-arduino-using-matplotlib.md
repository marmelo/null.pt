---
title: "Plotting analog readings from Arduino using Matplotlib"
date: 2011-03-10T23:45:00+01:00
draft: false
---

Uses an LDR (Light Dependent Resistor) to sample readings through the serial port every half second. A Python script reads from the serial port and plots the light intensity using Matplotlib.

<!--more-->

Optionally, just for the kicks, there are two LEDs:

- Green - Lit when the light intensity is higher than 50%
- Red - Otherwise

### Components

- LDR
- 2.2kΩ Resistor (red-red-red)
- Python (with [Matplotlib](http://matplotlib.sourceforge.net/) and [Serial](http://pyserial.sourceforge.net/))
- Red LED (optional)
- Green LED (optional)
- Two 220Ω Resistors (optional, red-red-brown)
- **WARNING**: The probability of the resistors being wrong is\... extremely high!

### Schematics

![](/files/Arduino-plot-04.jpg)
![](/files/Arduino-plot-03.jpg)
![](/files/Arduino-plot-02.jpg)
![](/files/Arduino-plot-01.jpg)

### Arduino Code

```
const int BAUD_RATE = 9600;
 
const int RED = 9;
const int GREEN = 8;
const int LDR = 0;
 
void setup()   {
  pinMode(RED, OUTPUT);
  pinMode(GREEN, OUTPUT);
  Serial.begin(BAUD_RATE);
}
 
void loop()
{
  int light = analogRead(LDR);
 
  if (light > 500) {
    digitalWrite(RED, LOW);
    digitalWrite(GREEN, HIGH);
  } else {
    digitalWrite(RED, HIGH);
    digitalWrite(GREEN, LOW);
  }
 
  Serial.println(light);
  delay(500);
}
```

### Python Code

```
from serial import Serial
from pylab import *
 
# serial connection to arduino
arduino = Serial('/dev/ttyUSB0', 9600)
 
# readings
y = []
 
# empty plot (interactive mode)
ion()
plot(y, 'b')
draw()
 
while True:
  try:
    # read from arduino
    reading = int(arduino.readline())
    # replot
    y.append(reading)
    plot(y, 'b')
    draw()
  except ValueError:
    pass
```

### Final Result

![](/files/Arduino-plot.png)
