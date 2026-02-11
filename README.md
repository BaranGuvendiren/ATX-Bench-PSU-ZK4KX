# ATX-Bench-PSU
High-Power ATX Bench PSU

## Project Overview
I needed a bench power supply to use for my personal projects. Instead of buying one, I decided to make one myself by repurposing an old PSU.

## Key Specifications
- Main Output: 1.2-36V regulated output which can deliver up to 11A, within 144W (+12V input supply limit).
- Low-noise output: Filtered branch of the main output with a 1A fuse, used for sensitive applications.
- 3.3/5V rails: Logic value DC rails with 1A fuse. Can be used with other outputs simultaneously.
- USB-C Port (5V stand-by): Can be connected to low-current devices, has a 1A fuse.
- CC Display Mode (Internal load): A dedicated switch activates an internal dummy load, forcing the regulator into CC mode for current calibration.
- Display Voltage Selection: Displayed voltage can be changed to show either Main or Low-noise output using the relevant switch.

## Design Evolution
I started this project as a simple integration of the ZK-4KX module with an ATX PSU. But during the design phase, I decided to change the architecture for a few reasons:
- **Power Gap:** The ZK-4KX module was designed to operate at 35W for constant usage, meaning it hardly used 20% of the 12V rail. Even though I could use the idle power by adding a direct 12V rail output, high currents with regulated voltage would be still impossible. So I removed the direct 12V rail and changed ZK-4KX with high-power buck-boost module.
- **Upgraded Filtering:** My initial plan was a two-staged LC+RC filter. The problem was, using a large inductive directly at the regulator output seemed risky. As the transient delay caused by the inductor, and the energy it stored, could affect the regulator's control loop stability. Also, the resistor in the RC circuit and the ESR of the inductor would cause an unstable voltage at sudden current changes. Because of those reasons, I decided to implement a pi-filter (C-FB-C) using a ferrite bead.
- **Custom Power Carrier Board:** Using multiple modules created a need for a carrier board. So I designed a custom board using KiCad. In addition to power distribution, I added P-MOSFET reverse polarity protection and Back-EMF protection for inductive loads. I also designed an internal load test to display current limit.
- **Improved Cooling:** Since the changes greatly increased the dissipated heat, I added a 7x7cm cooling fan on front panel's top.

## Important Implementation Details
- Although I designed the PCB to deliver up to 12A, I recommend a 8A limit for constant use.
- According to the label on my ATX PSU, the 12V rail can deliver up to 16A (192W). However, since the PSU is an unreliable old OEM unit with no official datasheet, I de-rated the capacity by 25% to improve long-term reliability.
- Limitations of the main output depends on which external buck-boost module is being used. Though, for safety reasons, the regulated voltage must not exceed 50V (voltage limit of the post-regulation electrolytic capacitors).
