This task’s aim is to connect three LEDs to Arduino GPIO digital pins serially and create and a 
repeating LED pattern in the code. This configuration was implemented first with large delays, 
then progressively was reduced to observe how the update rate changes. One LED pin’s 
waveform was measured using an oscilloscope, determining frequency, period, duty cycle, and 
timing variation at different speeds. 
Hardware configuration 
 Arduino Uno 
 3 LEDs 
 3 resistors 
 breadboard and jumper wires 
 oscilloscope probe connection 
Component Arduino Pin 
LED 1 
Description 
12 
LED 2 
Via series resistor 
8 
LED 3 
Via series resistor 
4 
Pin 12 
Via series resistor 
Oscilloscope 
probe 
Measured output 
Oscilloscope 
ground 
GND 
Common ground 
Each LED was connected to a digital output pin through a current-limiting resistor. The 
oscilloscope probe was connected to pin 12, with the ground clip connected to Arduino GND, so 
the voltage waveform of one LED output could be observed. 
Code Implementation 
The program generates a sequential LED pattern. First, pin 12 is driven HIGH, turning on LED 1 
for 500 ms. Then pin 12 is driven LOW and after that pin 8 is driven HIGH, turning on LED 2 
for 500 ms. Next, pin 8 is driven LOW and pin 4 is driven HIGH, turning on LED 3 for 500 ms. 
Finally, pin 4 is driven LOW and the loop repeats. In this example, because only one LED is kept 
on during each delay interval, the waveform measured on pin 12 is HIGH for one delay interval 
and LOW while the other two LEDs are active. That is why, one full cycle for pin 12 includes 
500 ms HIGH and 1000 ms LOW giving a total period of about 1500 ms. 
In the code, delays were manipulated progressively to observe update rate.

Very fast update rate, pattern no longer is visually readable, but waveform is still measurable. 
Calculations for period, frequency, and duty cycle 
For 500 ms delay: 
HIGH time = 500 ms 
LOW time = 500 ms + 500 ms = 1000 ms 
�
� =500+1000 =1500 𝑚𝑠 =1.5 𝑠 
�
� =1
𝑇
= 1
1.5
=0.67 𝐻𝑧 
�
�𝑢𝑡𝑦 𝐶𝑦𝑐𝑙𝑒 = 𝑇(ℎ𝑖𝑔ℎ)
𝑇
For each of the delays: 
Delay 
×100 = 500
1500
×100 =33.3% 
Ideal Period Measured Period 
Difference ΔT Measured Frequency 
500 ms 
1500 ms 
1502.2 ms 
2.2 ms 
0.67 Hz 
Measured Duty Cycle 
100 ms 
33% 
300 ms 
300.47 ms 
0.47 ms 
3.33 Hz 
10 ms 
33% 
30 ms 
30.9 ms 
0.09 ms 
33.3 Hz 
1 ms 
33% 
3 ms 
3.04 ms 
0.04 ms 
333.3 Hz 
We see that as the delay value is reduced, the period becomes shorter and the frequency 
increases. 
Timing Variation and Jitter 
33% 
Timing variation was estimated by comparing the measured waveform period with the ideal 
period expected from the programmed delays. For the 500 ms case, the ideal period on pin 12 is 
1500 ms, while the measured value differed slightly, giving an estimated timing error/jitter of 
about 2.5 ms. This timing variation occurs because additional execution time is required for each 
digitalWrite() call, loop repetition, and Arduino function overhead. This timing variation 
ultimately results in jitter. 
Fastest Reliable Update Rate 
As the delay was reduced, the waveform frequency increased and the period decreased, as 
expected. At high speeds, the LED sequence became difficult or impossible to distinguish 
visually. The fastest reliable update rate was the highest speed at which the waveform remained 
stable in the oscilloscope, and the LED pattern still repeated correctly, but we just could not 
observe it.The limit occurs because the Arduino executes the pattern entirely in software. Each 
loop iteration requires multiple digitalWrite() calls, loop control overhead, and delay handling. 
At very small delay values, these overheads become comparable to the intended timing itself, so 
they limit the maximum viable update rate. 
Discussion 
For all tested versions, the duty cycle of the measured pin remained close to 33% because each 
LED was active for one equal time slot in a three-step sequence. Reducing the delay shortened 
the total period and increased the output frequency. At slow speeds, the pattern was easy to 
observe visually. At fast speeds, the waveform remained correct on the oscilloscope (not 
including jitter), but the LEDs no longer appeared to blink distinctly.The measured values were 
not exactly equal to the ideal calculations because the software execution time adds overhead. 
This becomes more important as the programmed delays become smaller. 
