# STM32H755 External Oscillator Setup — HSE & LSE

## 1. Hardware Reference

| **Oscillator** | **Crystal** | **MCU Pins** | **Frequency** |
|---|---|---|---|
| HSE | Y1 — ECS-250-8-36-B2-CKM-TR | PH0 (OSC_IN) / PH1 (OSC_OUT) | 25.000 MHz |
| LSE | X1 — NX3215SA 32.768K | PC14 (OSC32_IN) / PC15 (OSC32_OUT) | 32.768 kHz |

**Wiring:**

- Y1 pin 1 → PH0, Y1 pin 2 → PH1, Y1 GND pin → GND, one load capacitor per signal pin to GND.
- X1 left pin → PC14, X1 right pin → PC15, one load capacitor per pin to GND.
- No external feedback resistor required — both are internal to the H755.

---

## 2. HSE Configuration (CubeMX / STM32CubeIDE)

1. Open **Pinout & Configuration** → **System Core** → **RCC**.
2. Set **High Speed Clock (HSE)** to **Crystal/Ceramic Resonator**.
3. Go to the **Clock Configuration** tab.
4. Set **Input frequency** to match Y1: **25 MHz**.
5. Select **HSE** (or the PLL fed from HSE) as the **System Clock Mux** source.
6. Configure PLL1 multiplier/divider values to reach your target CPU/bus frequencies from the 25 MHz input.
7. Verify no red/orange error boxes appear in the clock tree diagram before generating code.

### Manual HAL Equivalent (if not using CubeMX)
```c++
    RCC_OscInitTypeDef RCC_OscInitStruct = {0};
    RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
    RCC_OscInitStruct.HSEState = RCC_HSE_ON; // Crystal mode, not RCC_HSE_BYPASS

    // Configure PLL fields as needed.

    if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
    {
        Error_Handler(); // HSE may have failed to start.
    }
```
---

## 3. LSE Configuration (CubeMX / STM32CubeIDE)

1. In the same **RCC** panel, set **Low Speed Clock (LSE)** to **Crystal/Ceramic Resonator**.
2. Go to **System Core** → **RTC**, enable the RTC peripheral.
3. In **Clock Configuration**, confirm **RTC Clock Mux** is set to **LSE**.
4. No frequency entry is needed — LSE is fixed at 32.768 kHz.

### Manual HAL Equivalent

```c++
    RCC_OscInitTypeDef RCC_OscInitStruct = {0};
    RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_LSE;
    RCC_OscInitStruct.LSEState = RCC_LSE_ON; // Crystal mode, not RCC_LSE_BYPASS

    if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
    {
        Error_Handler();
    }

    RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};
    PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_RTC;
    PeriphClkInit.RTCClockSelection = RCC_RTCCLKSOURCE_LSE;

    if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
    {
        Error_Handler();
    }
```
---

## 4. Fallback Notes (if X1/Y1 are ever left unpopulated)

- **HSE unpopulated:** Do **not** select HSE as a clock source. Use `RCC_OSCILLATORTYPE_HSI` instead and derive PLL1 from HSI. Leaving CubeMX set to "Crystal/Ceramic Resonator" with no physical crystal will cause `HAL_RCC_OscConfig()` to time out and hit `Error_Handler()`.
- **LSE unpopulated:** Set **RTC Clock Mux** to **LSI** instead of LSE, and use `RCC_RTCCLKSOURCE_LSI` in the peripheral clock configuration. The same timeout risk applies otherwise.

---

## 5. Bring-Up Verification Checklist

- Measure Y1/PH0 with an oscilloscope or frequency counter — confirm approximately **25 MHz** once HSE is enabled in firmware.
- Measure X1/PC14 — confirm approximately **32.768 kHz** once LSE is enabled.
- Confirm `HAL_RCC_OscConfig()` returns `HAL_OK` for both. Add a breakpoint, LED, or UART print in `Error_Handler()` to catch startup failures.
- Read back `RCC->CR` (`HSERDY` bit) and `RCC->BDCR` (`LSERDY` bit) in the debugger to confirm both oscillators report ready.
- If the RTC is used, verify timekeeping accuracy over a known interval (e.g., one hour) against a reference clock to confirm LSE is driving the RTC rather than an LSI fallback.