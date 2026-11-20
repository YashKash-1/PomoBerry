<h1>8-Pin Grant Design Journal: PomoBerry (Pomodoro Timer)</h1>

<p><strong>Total Time Logged:</strong> 3 / 7 Hours</p>

<h2>Session 1: Project Setup &amp; Concept</h2>
<ul>
  <li><strong>Date:</strong> November 19, 2026</li>
  <li><strong>Time spent:</strong> 1 hour (Cumulative: 1.0 hr)</li>
  <li><strong>What I did:</strong>
    <ul>
      <li>Picked my idea: PomoBerry, a 25-minute Pomodoro timer badge on an ATtiny412.</li>
      <li>Sketched the pinout: one button for start/reset, one buzzer for the alarm, three LEDs for the phases (0-10, 10-20, 20-25 min), and a 3-pin UPDI header.</li>
      <li>Power is a 3V CR2032 coin cell.</li>
      <li>Made the GitHub repo and started this journal.</li>
    </ul>
  </li>
</ul>

<h2>Session 2: KiCad Schematic &amp; Wiring</h2>
<ul>
  <li><strong>Date:</strong> November 20, 2026</li>
  <li><strong>Time spent:</strong> 2 hours (Cumulative: 3.0 hrs)</li>
  <li><strong>What I did:</strong>
    <ul>
      <li>Set up the KiCad schematic and placed the ATtiny412.</li>
      <li>Added the CR2032 holder, a slide switch for power, and a 3-pin UPDI header (VCC, UPDI, GND) for programming.</li>
      <li><strong>Fixing C1:</strong> I got the capacitor wrong at first. I had put it next to the battery, before the switch, and the GND connection on it was messed up too. So it sat far from the chip, and the ATtiny had no decoupling right at its power pins. I deleted it and put a 100nF cap directly between pin 1 (VCC) and GND.</li>
      <li>Added the pushbutton on pin 2 (PA6) to GND. I'll use the chip's internal pull-up in code, so no extra resistor on the board.</li>
      <li><strong>Why 100 &Omega; on the buzzer:</strong> I put a 100 &Omega; resistor between the buzzer and GND. At 3V that caps the current at about 30mA. A CR2032 has fairly high internal resistance, so if the buzzer pulls too much at once the voltage can sag and reset the chip. The resistor limits that while the buzzer should still be loud enough.</li>
    </ul>
  </li>
</ul>

<img width="822" height="542" alt="image" src="https://github.com/user-attachments/assets/dfcfcaca-9226-4d1f-9954-09e94565c504" />
