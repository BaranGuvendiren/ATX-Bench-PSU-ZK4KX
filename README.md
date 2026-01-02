# ATX-Bench-PSU-ZK4KX
High-Power ATX Bench PSU with Multi-Rail Outputs and ZK-4KX Buck-Boost

## Why this project?
I needed a bench power supply to use for my personal projects. Instead of buying one, I decided to make one myself by repurposing an old PSU. It became an enjoyable learning experience for me.

## Hardware Overview
* **Source:** 350W Switching Power Supply (ATX).
* **Regulator:** ZK-4KX Buck-Boost module for adjustable outputs (0.5V-30V).
* **DC Rails:** While the adjustable output is limited to 35W/4A, the direct 12V rail provides high current for more demanding tasks. Also, 3.3V/5V rails can be used to power digital circuits without limiting power of the module. 

## Features
- **Adjustable Output:** 0.5V - 30V / 4A (35W) (Momentary 50W).
- **Low-Noise Adjustable Output:** Filtered output for sensitive low-current applications (1A fuse).
- **High-Current 12V Rail:** Protected by 10A fuse.
- **Fixed Logic Rails:** 5V (5A fuse) and 3.3V (3A fuse).
- **Extra:** 5V standby USB port for microcontrollers.

**Reason of 10A fuse on 12V rail:**
<br />
$$P_{in(max)} = \frac{P_{out(peak)}}{\eta} = \frac{50W}{0.88} \approx 56.8W$$
<br />
$$I_{in(max)} = \frac{P_{in(max)}}{V_{in}} = \frac{56.8W}{12V} \approx 4.73A$$
<br />
With the module drawing ~4.73A at peak, the 10A fuse provides a safe limit while allowing module to work on the same rail.

## Output Filtering Design
I planned a dedicated filtering board including both LC and RC filters to reduce switching noise from the buck-boost converter and the ATX power supply.

I first pass the 12V rail powering the buck-boost module through an LC filter to improve voltage stability during load transients.

After the LC filter, I split the output into two paths:
- The **main adjustable output**, taken directly after the LC stage to minimize voltage drop while avoiding unnecessary losses. 
- A **secondary low-noise output**, which is further filtered using an RC filter. This output is intended for sensitive, low-current applications where noise reduction is prioritized over voltage drop.

I have performed detailed filter calculations and simulations, which will be added once finalized.

## Design & Implementation
Instead of a linear power supply, which dissipates excess voltage as heat to maintain stability, I decided on a switching-based design. While linear supplies provide stable output, their thermal inefficiency requires large heatsinks, making them impractical for my needs. So instead, I integrated a buck-boost module and designed a two-step (LC-RC) filter to minimize switching noise. To handle power limitations effectively, I also separated the most-used voltage rails into dedicated outputs (so that more power can be drawn from the PSU simultaneously).

## Front Panel & User Interface
I am designing a front panel in Fusion 360 to mount the circuitry and attach it to the front of the ATX PSU enclosure.

Final CAD files and renders will be added once the mechanical dimensions and component layout are fully finalized.

### Wiring & Safety
Since the ATX PSU can deliver high currents, I selected wire gauges based on AWG standards. Each output rail is protected by a dedicated fuse to prevent accidental short circuits from damaging the components.

*Note: This project is work-in-progress*
