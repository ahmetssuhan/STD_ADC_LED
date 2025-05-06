# STM32F4 ADC‑Controlled LED Demo

This example demonstrates how to read an analog voltage on **PA1** using the on‑chip 8‑bit ADC (ADC1) of the **STM32F407G‑DISC1** board and drive the four on‑board LEDs (**PD12–PD15**) according to the measured value.

---

## Hardware

| Item          | Details                                                        |
| ------------- | -------------------------------------------------------------- |
| MCU board     | STM32F407G‑DISC1 (STM32F4‑Discovery)                           |
| Analog source | 0–3.3 V potentiometer wiper connected to **PA1**               |
| LEDs          | On‑board: PD12 (Green), PD13 (Orange), PD14 (Red), PD15 (Blue) |

**Wiring**

```
Potentiometer → PA1 (ADC Channel 1)
              GND ↔ GND
              Vref ↔ 3V3
```

---

## Folder Structure

```
project/
├── Inc/            # Header files
├── Src/            # Source files (main.c, system_stm32f4xx.c …)
├── .cproject/.project  # Atollic TrueSTUDIO metadata
└── README.md       # This file
```

---

## Import, Build & Flash (Atollic TrueSTUDIO 9.3)

1. **Import the project**

   * *File ▸ Import ▸ Existing Projects into Workspace* → select the `project` folder.
2. **Connect the board** via the ST‑LINK USB port.
3. **Build** with *Project ▸ Build All* (or **Ctrl+B**).
4. **Flash & debug** with the *Debug* toolbar button or **F11**.

> Default settings assume the ST‑LINK GDB server bundled with TrueSTUDIO. If you changed ST‑LINK firmware or drivers, adjust the debugger configuration accordingly.

---

## Code Walk‑Through

| Function        | Purpose                                                                                                                                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GPIO_Config()` | Enables clocks for GPIOD and GPIOA, sets PD12–PD15 as push‑pull outputs and PA1 as analog input.                                                                                                                     |
| `ADC_Config()`  | Enables the ADC1 clock, configures independent mode, prescaler ÷4, 8‑bit resolution, then enables ADC1.                                                                                                              |
| `Read_ADC()`    | Selects **Channel 1**, starts a software conversion, waits for **EOC**, and returns the 8‑bit digital value (0–255).                                                                                                 |
| `main()`        | Initializes peripherals, enters an infinite loop, reads the ADC value, and lights LEDs according to these ranges:<br/>`< 50 → Green` • `50–99 → Orange` • `100–149 → Red` • `150–199 → Blue` • `200–255 → All LEDs`. |

### Timing Notes

* **Sample time** is set to 56 cycles, balancing speed and settling for moderate source impedances (<10 kΩ).
* At 8‑bit resolution the full‑scale reading corresponds to 3.3 V, so each LSB ≈ 13 mV.

---

## Customisation

* **Resolution** – change `ADC_Resolution_8b` to `12b`, `10b`, or `6b` and adjust the LED thresholds.
* **Input pin** – select another analog channel with `ADC_RegularChannelConfig()`.
* **LED logic** – replace the if‑else chain with a LUT or a `switch` for faster execution.

---

## Troubleshooting

| Symptom                            | Likely Cause & Fix                                                                             |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| All ADC reads return 0             | Check potentiometer wiring, verify 3.3 V reference, ensure PA1 isn’t left floating.            |
| ST‑LINK *“Target no device found”* | Wrong BOOT0/BOOT1 setting, faulty USB cable, out‑of‑date ST‑LINK firmware.                     |
| LEDs flicker randomly              | Source impedance too high ➜ lower potentiometer value or increase sample time to ≥ 144 cycles. |

---

## References

* **RM0090** – *STM32F4 Reference Manual*, §11 (ADC) and §8 (GPIO).
* **AN2834** – *How to get the best ADC accuracy*.
* STMicroelectronics application example projects for STM32F4‑Discovery.

---

## License

This example is released under the MIT License – see `LICENSE` for details.
