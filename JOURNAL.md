<h1>8-Pin Grant Design Journal: PomoBerry (Pomodoro Timer)</h1>

<p><strong>Total Time Logged:</strong> 4.5 / 8 Hours</p>

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

<h2>Session 3: Finishing the Schematic &amp; Passing ERC</h2>
<ul>
  <li><strong>Date:</strong> November 20, 2026</li>
  <li><strong>Time spent:</strong> 1.5 hours (Cumulative: 4.5 hrs)</li>
  <li><strong>What I did:</strong>
    <ul>
      <li>Wired the three phase LEDs (D1, D2, D3) to pins 4, 5 and 7 of the ATtiny412, each with a 300 &Omega; resistor to GND.</li>
      <li>
        <strong>Running ERC:</strong> I ran KiCad's electrical rules check before starting the layout.
        <ul>
          <li><strong>First run:</strong> 3 errors. One was an unconnected pin on the slide switch, and the other two were KiCad saying nothing was driving the VCC and GND nets.</li>
          <li><strong>Fixes:</strong> I put a no-connect flag on SW1 pin 3 (the OFF side, which I don't use) and added a PWR_FLAG on VCC and one on GND so KiCad knows the battery is the power source.</li>
          <li><strong>Second run:</strong> 0 errors, 0 warnings. All 8 pins of the ATtiny are accounted for.</li>
        </ul>
      </li>
      <li>Next: assign footprints and start the PCB layout.</li>
    </ul>
  </li>
</ul>

<h2>Final Schematic</h2>
<img width="887" height="717" alt="Screenshot 2026-11-20 124645" src="https://github.com/user-attachments/assets/6a3cea97-44f9-4c0f-be82-5f859ad327ab" />

<h2>ERC Test</h2>
<img width="884" height="877" alt="Screenshot 2026-11-20 124555" src="https://github.com/user-attachments/assets/0e149295-06e4-483c-90ad-760c120eb71c" />

<h2>Session 4: Footprint Assignment &amp; PCB Sync</h2>
<ul>
  <li><strong>Date:</strong> November 20, 2026</li>
  <li><strong>Time spent:</strong> 1 hour (Cumulative: 5.5 hrs)</li>
  <li><strong>What I did:</strong>
    <ul>
      <li>
        <strong>Assigned footprints for all 14 components:</strong>
        <ul>
          <li><code>U1 (ATtiny412)</code>: <code>Package_SO:SOIC-8_3.9x4.9mm_P1.27mm</code> (standard SOIC-8).</li>
          <li><code>C1, R1&ndash;R4, D1&ndash;D3</code>: <code>0805_2012Metric</code> SMD packages. Picked 0805 because it's compact but still easy to hand-solder without a microscope.</li>
          <li><code>SW2 (Button)</code>: <code>Button_Switch_THT:SW_PUSH_6mm</code> (standard through-hole tactile button).</li>
          <li><code>SW1 (Power Switch)</code>: <code>Button_Switch_THT:SW_Slide_SPDT_Straight_CK_OS102011MS2Q</code> (vertical slide switch).</li>
          <li><code>BZ1 (Buzzer)</code>: <code>Buzzer_Beeper:Buzzer_12x9.5RM7.6</code> (standard round piezo).</li>
          <li><code>BT1 (Battery)</code>: <code>Battery:BatteryHolder_Keystone_3002_1x2032</code> (CR2032 coin cell clip).</li>
          <li><code>J1 (UPDI Header)</code>: <code>Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical</code>.</li>
        </ul>
      </li>
      <li>
        <strong>PCB Netlist Import:</strong> Ran "Update PCB from Schematic" to pull all footprints and ratsnest connections into the PCB editor.
        <strong>Result: 0 errors!</strong> Everything synced cleanly on the first try.
      </li>
      <li>Next: Component placement, flipping the battery to the bottom layer, and drawing the board outline.</li>
    </ul>
  </li>
</ul>

<img width="1917" height="937" alt="Screenshot 2026-11-20 145532" src="https://github.com/user-attachments/assets/1a1df19d-1854-4dea-9674-cc4c5304e214" />

<h2>PCB Update </h2>

<h3>DRC Test</h3>

<h5>First time i got 0 error and 0 warring in DRC test i am so <b>happy woohooo!!!!!</b></h5>

<img width="797" height="722" alt="Screenshot 2026-11-20 144835" src="https://github.com/user-attachments/assets/93255565-7301-4e32-b0ef-c41a992f283c" />

<h2>Session 5: Placing the Parts &amp; 3D View</h2>
<ul>
  <li><strong>Date:</strong> November 20, 2026</li>
  <li><strong>Time spent:</strong> 1 hour (Cumulative: 6.5 hrs)</li>
  <li><strong>What I did:</strong>
    <ul>
      <li>Moved all the parts onto the board and worked out where each one goes.</li>
      <li>Put C1 right up against the ATtiny's power pins so it can do its job.</li>
      <li>Lined the 3 LEDs up in a column on the right, with each 300 &Omega; resistor next to its LED. This should keep the traces short and straight instead of crossing each other.</li>
      <li>Buzzer is bottom left. The button is in the middle so a thumb can reach it easily. The power switch is on the right.</li>
      <li>Squeezed the buzzer's 100 &Omega; resistor (R1) in between the buzzer and the button.</li>
      <li>Hit Alt+3 to check the 3D view. Looked clean to me: nothing overlaps, and the tall parts (buzzer and switch) don't block anything.</li>
      <li>Next: route the copper traces.</li>
    </ul>
  </li>
</ul>

<h2>PCB Layout without tracing</h2>
<img width="917" height="791" alt="Screenshot 2026-11-20 184118" src="https://github.com/user-attachments/assets/201740a1-98a5-4596-90ac-85986d7cd125" />

<h2>3D View without tracing</h2>
<img width="693" height="607" alt="Screenshot 2026-11-20 184050" src="https://github.com/user-attachments/assets/da2a163f-b675-4db9-92b2-a84844a2b91e" />

