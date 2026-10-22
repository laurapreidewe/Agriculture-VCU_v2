# Agricultural VCU V2

Vehicle control unit PCB for agricultural robots and machinery, built around the Nuvoton M467HJHAN microcontroller. V2 provides Ethernet, CAN, UART, ADC/DAC, PWM, and six relay channels.

The project was originally designed in **KiCad 8** and has been updated to **KiCad 10**. The current schematic and PCB files identify KiCad 10.0 as their generator.

**Known issue:** the relay circuit requires rework because pins **3 and 4** are swapped in the optocoupler footprint. Refer to the [PCB Test Report Rev. A](docs/references/pcb-test-report-rev-a.pdf) for rework details.

## Board overview

![Annotated Agricultural VCU V2 board showing the power input, buck module, regulator, DAC, ADC, CAN, PWM, UART, MCU, reset button, USB, relays, and Ethernet](docs/images/vcu-v2-board-overview.png)

The photograph shows the assembled board with relay rework wiring.

## Hardware

| Item | V2 design |
| --- | --- |
| MCU | Nuvoton M467HJHAN, Arm Cortex-M4, LQFP176 |
| Power | 12 V input, external 12 V to 5 V buck module, TPS79533 3.3 V regulator |
| Input protection | Fuse, power switch, and unidirectional TVS diode |
| Ethernet | RTL8201FI PHY, RMII connection to the MCU, magnetics, and RJ45 connector |
| CAN | Two interfaces using external MCP2551 transceiver modules |
| UART | UART0 and UART2 connectors |
| Analogue interfaces | Two ADC inputs with 5 V to 3.3 V scaling and two DAC output channels |
| PWM | Two outputs, BPWM0_CH0 and BPWM1_CH0, with BSS138 level shifting |
| Relays | Six 12 V SPDT channels with optocoupler-based drivers; rework required |
| Expansion and debug | Four 2×20 GPIO sockets and a Nu-Link SWD header |
| Storage | External SPI flash; the V1 SD-card module connector was removed |
| PCB | Four layers, 176.53 × 184.15 mm, 2 mm thick |
| Fabrication limits | 0.1 mm minimum track width/spacing and 0.2 mm minimum hole diameter |

Connector pinouts, switches, LEDs, and test points are listed in the [user manual](docs/references/user-manual.pdf).

## Project files and KiCad setup

Open [UGV Control Unit.kicad_pro](UGV%20Control%20Unit.kicad_pro) with KiCad 10 to work with the current project.

| File | Contents |
| --- | --- |
| [UGV Control Unit.kicad_sch](UGV%20Control%20Unit.kicad_sch) | Root schematic |
| [UGVPower.kicad_sch](UGVPower.kicad_sch) | Power supply |
| [UGVMCU.kicad_sch](UGVMCU.kicad_sch) | MCU, clocks, debug, USB, and flash |
| [UGVGPIO.kicad_sch](UGVGPIO.kicad_sch) | Relays and external interfaces |
| [UGVEthernet.kicad_sch](UGVEthernet.kicad_sch) | Ethernet circuitry |
| [UGV Control Unit.kicad_pcb](UGV%20Control%20Unit.kicad_pcb) | PCB layout |
| [bom/ibom.html](bom/ibom.html) | Interactive BOM and component placement |
| [UGVPCBGerber.zip](UGVPCBGerber.zip) | Existing Gerbers and manufacturing data |
| [UGV Control Unit-odb.zip](UGV%20Control%20Unit-odb.zip) | Existing ODB++ export |
| [Top placement](UGV%20Control%20Unit-top.pos) / [bottom placement](UGV%20Control%20Unit-bottom.pos) | Component position files |
| [3D model.zip](3D%20model.zip) | Complete board STEP model |

The design references custom symbol/footprint libraries, including `MCU_Nuvoton_M467`, `TPS79533DCQ`, and `DESD5V0U1BA_7`. Configure the required libraries when editing or updating components. Some PCB 3D-model paths still use `${KICAD8_3DMODEL_DIR}`; check those paths in KiCad 10 if individual component models are missing. The STEP archive contains the complete board model.

## Changes from V1 to V2

The [Version Control Report](docs/references/version-control-report.pdf) documents the hardware changes from V1 (February 2025) to V2 (January 2026). This hardware revision history is separate from the KiCad 8-to-10 software migration.

| Area | V1 | V2 |
| --- | --- | --- |
| MCU | STM32F446RE, LQFP64 | Nuvoton M467HJHAN, LQFP176 |
| Clocks | 8.192 MHz external crystal; external RTC clock unused | 12 MHz external crystal and external RTC clock |
| Power input | Four-pin connector for 12 V, 72 V, and grounds | Two-pin 12 V/GND connector; added TVS protection |
| MCU supply | VCAP routing mistake required a manual trace cut | LDO_CAP pins kept separate from the 3.3 V rail |
| Decoupling | 0402 capacitors | 0603 capacitors |
| GPIO expansion | One 2×13 and three 2×12 sockets, with 3.3 V and 5 V power pins | Four 2×20 sockets, with 3.3 V power pins |
| Debug | STM32 SWD and boot-jumper circuitry | M467 SWD on PF.0/PF.1 with external pull-ups; simplified boot circuitry |
| Ethernet | Unused | Added PHY, magnetics, and RJ45 interface |
| Relays | 12 relays, BJT-based drivers, D-sub/terminal-block connections | Six SPDT relays, optocoupler-based drivers, individual terminal blocks |
| Interfaces | ADC, DAC, CAN, one UART, and SD-card module connector | Retained ADC/DAC/CAN; two UARTs and two PWM outputs; removed SD-card module connector |
| Storage and USB | No external flash; USB shield directly grounded | Added SPI flash; USB shield connected through a ferrite bead |
| Layout | 180 × 180 mm; components on the top layer | 176.53 × 184.15 mm; some Ethernet components on the bottom layer, edge connectors, and teardrops at small vias |

## Reference documents

| Source | Purpose |
| --- | --- |
| [Presentation1.pptx](docs/references/Presentation1.pptx) | Original annotated board overview, rendered above as a PNG |
| [Agricultural VCU V2 User Manual](docs/references/user-manual.pdf) | Pin assignments, connectors, switches, LEDs, test points, and schematics |
| [Agricultural VCU V2 Testing Protocol](docs/references/testing-protocol.pdf) | Assembly and laboratory test procedures |
| [Agricultural VCU v2 PCB Test Report Rev. A](docs/references/pcb-test-report-rev-a.pdf) | PCB test record and relay rework details |
| [Agricultural VCU V2 Version Control Report](docs/references/version-control-report.pdf) | Changes from hardware V1 to V2 |
| [20260810_Rai.pptx](docs/references/20260810_Rai.pptx) | V2 evaluation list; original location: `season 2/projects/presentation/20260810_Rai.pptx` |
