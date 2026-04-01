# agrobot_master_wiring_diagram
## About this project

**@Callie** and **@fleabrew** are jointly working on a master wiring diagram for the system, including the motors, controllers, gripper subsystem, PC, PLC, and other odd components. This will be a document-in-progress for a while, but the first draft should be ready **Thursday**.

Rough draft should include the following:

- Joints (J0-J6 + gripper servo)
    - J0, J1 are driven by Copley drives
    - J2-J6 are driven by Maxon EPOS2 drives
    - For now, just include “phase wires” and “encoder wires” as abstract lines - don’t worry about how many there are and what they are yet. We can discuss in detail on Thursday with Kaedin.
    - Every controller is connected to CAN, 48V Power and 24V Logic.
    - The gripper is a special case; we are basically building the “controller” ourselves with an ESP32. For this rough draft, start by abstracting it as a device called the “Gripper Controller” which takes in 24V, 48V, and CAN just like the other controllers.
- CANBUS
    - CAN is a bus, meaning all devices on the network are connected in a line. A 120-ohm resistor is used to terminate the bus at either end.
- Power
    - We have 2 power rails: a 48V main power rail and a 24V Logic power rail.
    - The 48V power rail is connected to all the motor drives and is supplied by 2 (possibly 3) 1400W 48V supplies tied together in parallel.
    - The 48V power rail is E-stop switched, meaning there should be a safety relay in the diagram for cutting power off at the positive rail.
    - The integrity of the 48V supply is maintained by the Shunt regulator system, and optionally a large power capacitor. Include these in the diagram, but only as abstractions.
    - The 24V power is supplied by the (currently unused) second 24V DIN rail supply on the PLC rail. It provides logic power for all the controllers, and will likely also provide power for the gripper motors, for a total peak draw likely close to 3A.
- PLC
    - The PLC is the safety controller for the system. It has the job of monitoring the CANBUS line and digital inputs for stop signals.
    - The PLC is connected to the light tree by it’s digital output pins, to inform users about the current state of the machine.
    - The PLC is also connected to the Safety relay. One line is the Estop line, where the PLC can command a stop if it sees an issue reported by any CANBUS device. There is a second line going from the relay back to the PLC to report if the relay is currently in the safe state.
- E-stop and safety system
    - The safety relay is a “smart” relay - it can monitor multiple seperate input lines and cut the power if any of them are triggered.
    - Our system should have two triggers: an estop button and the estop output of the PLC.
