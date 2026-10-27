## Debug / Programming

| MCU Pin | Net | Connects To | Notes |
|---|---|---|---|
| PA13 | `N$2` | SWD header pin 4 | SWDIO |
| PA14 | `N$1` | SWD header pin 2 | SWCLK |
| BOOT0 | `BOOT0` | Boot-select strap | Boot mode select |

## External Oscillators

These must be manually configured if installed. See [Oscillators](./Oscillators.md) documentation.

| MCU Pin | Net | Function |
|---|---|---|
| PC14/PC15 | `OSC32_IN/OSC32_OUT `| ECS-250-8-36-B2-CKM-TR 25MHz HSE |
| PH0/PH1 | `OSC_IN/OSC_OUT` | NX3215SA 32.768K 32.768 kHz LSE|

## Onboard SPI Flash (NVFLASH, M25P05-A)

| MCU Pin | Net | Function |
|---|---|---|
| PA5 | `FLASH_SPI_SCK` | SPI1 Clock |
| PA6 | `FLASH_SPI_MISO` | SPI1 MISO |
| PA7 | `FLASH_SPI_MOSI` | SPI1 MOSI |
| PE2 | `FLASH_GPIO_CS` | Chip select (active-low, flash's `!S` pin) |

## External SPI Header (SPI_OUT, SPI2)

| MCU Pin | Net | Function |
|---|---|---|
| PB13 | `SPI_SCK` | SPI2 Clock |
| PB14 | `SPI_MISO` | SPI2 MISO |
| PB15 | `SPI_MOSI` | SPI2 MOSI |
| PB12 | `SPI_CS` | SPI2 NSS (pulled up to 3.3V via R12) |

## I²C2 Sensor Bus

| MCU Pin | Net | Function |
|---|---|---|
| PF0 | `I2C2_SDA` | I2C2 Data |
| PF1 | `I2C2_SCL` | I2C2 Clock |
| PF15 | `I2C2_SDA_OUT` | I2C2 Data |
| PF14 | `I2C2_SCL_OUT` | I2C2 Clock |

| Device | Component | Notes |
|---|---|---|
| BMI323PRIMARY | BMI323 | IMU #1 |
| BMI323BACKUP | BMI323 | IMU #2 (redundant) |
| MAGNETOMETER | LIS2MDL | Magnetometer |
| BMP581PRIMARY | BMP581 | Barometer; SDI/SCK pins strapped for I²C mode, not SPI |

## microSD Card (J1, MSD-1-A)

| MCU Pin | Net | Function |
|---|---|---|
| PC8 | `MSD_DAT0` | SD Data 0 |
| PC9 | `MSD_DAT1` | SD Data 1 |
| PC10 | `MSD_DAT2` | SD Data 2 |
| PC11 | `MSD_CD_DAT3` | SD Data 3 |
| PC12 | `MSD_CLK` | SD Clock |
| PD2 | `MSD_CMD` | SD Command |
| PG2 | `MSD_CD` | Card detect |

## FDCAN1 (CAN FD Bus)

| MCU Pin | Net | Function |
|---|---|---|
| PD0 | `FDCAN1_RX` | FDCAN_TS transceiver R pin |
| PD1 | `FDCAN1_TX` | FDCAN_TS transceiver D pin |

| Component | Part Number | Function |
|---|---|---|
| FDCAN_TS | SN65HVD230 | CAN transceiver; drives CANH/CANL to the 3-pin FDCAN1 header |

## Pyro Firing Circuit

| MCU Pin | Net | Function |
|---|---|---|
| PE9 | `PYRO_GPIO_MAIN` | Arm; gates Q1 → T1 to switch the +12V rail. Has R22 pull-down. |
| PE7 | `PYRO_GPIO_1` | Fire channel 1; gates Q2 and has a pull-down resistor. |
| PE8 | `PYRO_GPIO_2` | Fire channel 2; gates its MOSFET and has a pull-down resistor. |
| PC2_C | `PYRO_ADC_1` | Continuity/voltage sense, channel 1 (via 33k). |
| PC3_C | `PYRO_ADC_2` | Continuity/voltage sense, channel 2 (via 33k). |

## USB Power Switch Monitor (U2, TPS2553)

| MCU Pin | Net | Function |
|---|---|---|
| PE3 | `VOUT_SHORT` | U2's `!FAULT` output; active-low input for detecting an overcurrent/short on the switched 3.3V output. |

## Unconnected Pins and Clock Sources

| MCU Pin | Connection Status | Notes |
|---|---|---|
| NRST | Unconnected | No reset button or capacitor connected. |

## MCU Pin Summary

| Port | Pins Used | Purpose |
|---|---|---|
| PA | PA5–PA7 | Flash SPI1 |
| PA | PA13–PA14 | Debug (SWDIO/SWCLK) |
| PB | PB12–PB15 | External SPI2 (NSS/SCK/MISO/MOSI) |
| PC | PC2_C–PC3_C | Pyro continuity sensing (ADC) |
| PC | PC8–PC12 | microSD (DAT0–3/CLK) |
| PD | PD0–PD1 | FDCAN1 (RX/TX) |
| PD | PD2 | microSD CMD |
| PE | PE2 | Flash chip select |
| PE | PE3 | USB power switch fault |
| PE | PE7–PE9 | Pyro fire/arm |
| PF | PF0–PF1 | I²C2 (SDA/SCL) |
| PG | PG2 | microSD card detect |