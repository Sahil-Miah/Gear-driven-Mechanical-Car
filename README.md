# Gear-driven-Mechanical-Car

Front-wheel-drive gear car that travels to a wall, reverses via a DPDT switch circuit, and mechanically returns to its exact starting position — no microcontroller. Features a custom laser-cut gear train and a 1:47 mechanical stopping mechanism.

![CAD Assembly](./assets/cad_render.jpg)

## Overview

This project explores the integration of mechanical power transmission (custom gear train), electromechanical control (DPDT motor-reversing circuit), and precision positioning (mechanical stop mechanism) into a single working vehicle. The brief required the vehicle to autonomously reach a wall, reverse, and return to its exact starting position — with no microcontroller or programmable logic involved. All control is achieved through switch-based circuitry.

**[▶ Watch the demo video](./assets/demo.mp4)**

## How It Works

### Mechanical Drive
A single 6V DC motor drives the rear axle through a custom gear train, laser-cut from 5mm acrylic:
- 1x large gear
- 1x medium two-gear piece (compound gear)
- 2x small gears
- 2x mini gears

The gear train steps down motor speed to increase torque at the wheels, allowing a small motor to reliably move the chassis. Drive is transmitted to all four wheels via steel axle rods (19cm, 12cm, and 7cm lengths), with rubber bands fitted as improvised drive tracks across the wheel sets.

### Reversing Circuit
Direction reversal is handled entirely by a **DPDT (Double Pole Double Throw) latching switch**, without any microcontroller:

![DPDT wiring diagram](./assets/dpdt_circuit.png)

- Common terminals (B & E) stay permanently connected to the power supply whenever the circuit is live.
- The motor is wired across the two remaining terminal pairs (A–D and C–F).
- In one switch position, current flows one way through the motor (forward rotation). Flipping the switch reverses polarity across the motor terminals, reversing rotation.
- A **microswitch** mounted at the front of the chassis acts as the physical trigger: on contact with the wall, it actuates the DPDT switch, reversing the motor without any manual input.
- Power to the whole circuit is gated by a simple SPST on/off switch, with a fuse holder and fuse fitted for overcurrent protection.

### Return-to-Start Mechanism
A laser-cut acrylic "prong" acts as a mechanical stop, physically halting the vehicle at its original position on the return leg — a purely mechanical solution rather than a second sensor or timed circuit.

## Design Process

The gear train and motor selection followed a standard mechanical design method (per MMU's Design Handbook for the 6E4Z0004 Design Project):

1. Calculate the minimum required vehicle velocity from the target distance and time constraint, with a safety margin applied.
2. Assume a wheel radius and derive the required wheel rotational speed from that target velocity.
3. Calculate the gear ratio needed to match motor output speed to the required wheel speed: 

   `GR = speed of driver / speed of driven = teeth on driven / teeth on driver`
4. Iterate on wheel size and gear ratio until a physically achievable gear combination (whole numbers of teeth only) is reached.

This process governed the choice of a multi-stage gear train (rather than a single gear pair) to achieve a practical, manufacturable ratio while keeping torque at the wheels sufficient to move the chassis reliably.

## Design Calculations

**Drivetrain (front-wheel drive):**
The vehicle is front-wheel drive. The motor's 40-tooth gear meshes with a 20-tooth gear on the front axle:

`GR = teeth driven / teeth driver = 20/40 = 0.5` → front axle turns at **2× motor speed**

- Motor no-load speed: 35 RPM
- Front axle speed: 35 × 2 = **70 RPM**
- Wheel diameter: 70mm (0.07m)
- Vehicle speed: v = π × D × n / 60 = π × 0.07 × 70 / 60 ≈ **0.257 m/s** (~25.7 cm/s)

**Return-to-start stopping mechanism:**
The rear axle is undriven — it free-spins as a direct result of the car rolling forward, and since front and rear wheels share the same 70mm diameter, the rear axle also rotates at 70 RPM. This passive rotation drives a mechanical reduction train that acts as the vehicle's "distance counter," removing the need for any electronic sensor:

- Stage 1: 12T (rear axle, driver) → 60T (compound gear, driven) = 12/60 = **1:5 reduction** → 70 ÷ 5 = 14 RPM
- Stage 2: 10T (same shaft as the 60T, driver) → 94T pronged gear (driven) = 10/94 = **1:9.4 reduction** → 14 × (10/94) ≈ **1.49 RPM**
- **Combined reduction: (12/60) × (10/94) = 1:47** — the pronged gear turns once for every 47 revolutions of the rear axle/wheels

The gear train is deliberately sized so the pronged gear completes only **0.9 of a revolution** over the vehicle's outbound journey to the wall — intentionally undershooting a full turn. On the return leg, the DPDT switch reverses the motor and, by extension, reverses rotation through the entire drivetrain including the stopping mechanism gears. The 94T pronged gear therefore unwinds the same 0.9 revolution it wound up on the way out, arriving back at its exact starting angular position at the same moment the vehicle arrives back at its exact starting physical position — at which point the prong strikes the microswitch and cuts power to the circuit. Because the prong is calibrated to less than a full revolution, it can only ever contact the switch once, at that single defined position — the mechanism functions as a purely mechanical, self-resetting odometer with no electronics involved.

**Safety:** A fuse and fuse holder are fitted in-line with the power circuit to protect against overcurrent in the event of a motor stall or short circuit — a deliberate design inclusion rather than an oversight, given the vehicle relies on a hard mechanical stop (the microswitch trigger) rather than a soft/electronic cutoff.

## Bill of Materials

Fully custom-manufactured components (gears, axle holders, chassis plank, stopping prong) were laser-cut from 5mm acrylic. All fasteners, motors, and electrical components were sourced from standard lab stock.

| Component | Qty | Notes | Cost |
|---|---|---|---|
| 6V DC Motor | 1 | Primary drive | £9.71 |
| Large gear | 1 | 5mm acrylic, laser-cut | £0.25 |
| Medium two-gear piece | 1 | 5mm acrylic, compound gear | £0.37 |
| Small gears | 2 | 5mm acrylic | £0.18 |
| Mini gears | 2 | 5mm acrylic | £0.09 |
| Acrylic chassis plank (26x10cm) | 1 | 5mm acrylic | £1.84 |
| Axle holders | 4 | 5mm acrylic | £0.30 |
| Stopping mechanism prong | 1 | 5mm acrylic | £0.06 |
| Wheels | 4 | | £0.90 |
| Wheel bearings | 4 | | £11.00 |
| Steel rod (19cm) | 2 | Axles | £0.75 |
| Steel rod (12cm) | 1 | Axle | £0.24 |
| Steel rod (7cm) | 1 | Axle | £0.14 |
| Steel hook | 1 | | £1.30 |
| DPDT switch (PCB-mount) | 1 | Reversing circuit | £2.36 |
| SPST on/off switch | 1 | | £0.32 |
| Microswitch | 1 | Wall-contact trigger | £0.35 |
| Fuse holder w/ fuse | 1 | Overcurrent protection | £0.05 |
| Battery holder | 1 | | £0.25 |
| Battery holder clip | 1 | | £0.25 |
| Batteries | 4 | | £3.32 |
| Wires | 12 | | £1.20 |
| Wall brackets | 2 | | £0.54 |
| Plastic wall piece | 1 | Target/obstacle | £0.14 |
| Rubber bands | 4 | Drive tracks | £0.10 |
| **Total** | | | **£36.01** |

## Repository Contents

- `CAD for mechanical car.f3z` — full Fusion 360 assembly (native, includes design history)
- `CAD For Mechanical Car.step` — universal format, viewable without Fusion
- `Bill of Materials.xlsx` — full itemized cost breakdown
- `assets/demo.mov` — video of the vehicle completing a full run
- `assets/dpdt_circuit.png` — DPDT switch wiring reference
- `assets/cad_render.jpg` — rendered assembly screenshot

## What I'd Improve

- Model standard fasteners (M2/M3/M4 hardware) in CAD using library components rather than leaving holes unmodeled, for a more complete assembly
- Add a proper Fusion motion simulation of the gear train and reversing sequence
- Replace the rubber-band drive tracks with purpose-made belting for more consistent traction
- Instrument the return position with a simple sensor to quantify positioning accuracy over repeated runs

## Skills Demonstrated

Mechanical power transmission (gear train design and manufacture) · Electromechanical control circuit design (DPDT motor reversal) · CAD assembly modelling (Fusion 360) · Design-to-manufacture workflow (laser-cut acrylic parts) · Cost-conscious component sourcing and BOM management

