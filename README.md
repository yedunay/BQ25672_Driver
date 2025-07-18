# BQ25672 Embedded Driver

**Author:** Yunus Emre DUNAY
**Language:** C (C99)
**Target MCU:** STM32G0 / STM32 HAL (I2C Peripheral)

---

## 📖 Project Description

This repository provides a **fully modular, readable and maintainable driver** for the **Texas Instruments BQ25672** battery charger IC.
The BQ25672 is a highly integrated, I2C-programmable, 2-cell charger IC supporting fast charging with JEITA profile, OTG functionality, MPPT, comprehensive safety features and robust ADC diagnostics.

This driver has been specifically written from scratch in **C** for projects requiring **robust and clean hardware abstraction** when interacting with the BQ25672 through STM32 HAL I2C libraries.
The driver is designed with maintainability, readability, and future extension in mind.

---

## 🚀 Motivation

Existing reference drivers provided by manufacturers often lack structure, readability, and flexibility for embedded applications requiring modular, layered architectures.
This project aims to provide:

✅ **Readable and maintainable C source code**
✅ **Bitfield-level register abstraction** via header masks and shifts
✅ **Struct-based configuration management** for user-friendly API interaction
✅ **Comprehensive JEITA, NTC, safety, timers, watchdog, MPPT management**
✅ **Separation of concerns (No application logic included)**

---

## 📂 Project Structure

```shell
📁 bq25672_driver/
 ┣ 📄 bq25672.h      # Complete header: Register maps, enums, structs, function prototypes
 ┣ 📄 bq25672.c      # Implementation: Getter / Setter / Apply / Read / Reset / Init
```

---

## ⚙️ Key Features

* Complete coverage of **all configuration registers**
* Clean abstraction layer via **`bq25672_config_t`** struct
* JEITA (NTC) configurable via enums and structs
* USB-OTG regulation support (voltage/current)
* Watchdog / Timer / Precharge / Termination / Safety limits
* MPPT (Maximum Power Point Tracking) with full options
* ADC readings support (current, voltage, temperature)
* Status & flag management (charger states, faults)
* Hardware reset, software reset, full read-back support

---

## 📥 API Overview

### 1️⃣ **System Configuration**

Read/write HIZ, ICO, Termination, PFM, LDO, ACDRV, PWM freq via:

```c
bq25672_get_system_config();
bq25672_set_system_config();
```

### 2️⃣ **Charge Configuration**

Voltage, current limits, termination thresholds:

```c
bq25672_get_charge_config();
bq25672_set_charge_config();
```

### 3️⃣ **USB-OTG Configuration**

```c
bq25672_get_usb_config();
bq25672_set_usb_config();
```

### 4️⃣ **Watchdog / Timers / MPPT / NTC / JEITA / Interrupts**

```c
bq25672_get_watchdog_config();
bq25672_set_watchdog_config();
bq25672_get_timer_config();
bq25672_set_timer_config();
...
```

### 5️⃣ **Apply / Read / Reset / Init**

Apply entire configuration to IC, or read back to config struct:

```c
bq25672_apply_config();
bq25672_read_config();
bq25672_soft_reset();
bq25672_hard_reset();
bq25672_init();
```

---

## 🔧 Usage Example (Basic Flow)

```c
bq25672_config_t config = BQ25672_DEFAULT_CONFIG;

config.charge.charge_voltage_max_mv = 8400;
config.charge.charge_current_max_ma = 2000;
config.system.enable_termination = true;
config.system.enable_hiz = false;

if (bq25672_apply_config(&hi2c1, &config) != BQ25672_OK) {
    // Handle error
}
```

---

## 💡 Why Struct-Based?

* Centralizes all parameters in a **single human-readable structure**
* Eliminates the need to manually track register maps during runtime
* Guarantees consistency between software representation and IC status
* Improves maintainability and future scalability

---

## 🔌 I2C Requirements

* BQ25672 communicates over I2C, **7-bit address: 0x6B**
* Driver depends on `HAL_I2C_Mem_Read` / `HAL_I2C_Mem_Write`

Example:

```c
I2C_HandleTypeDef hi2c1;
bq25672_apply_config(&hi2c1, &config);
```

---

## 🧪 Verification

The driver was validated on hardware with STM32G030F6 MCU communicating to BQ25672 over I2C. ADC readings, fault flags, status checks and all configuration parameters were verified through oscilloscope & logic analyzer measurements.

---

## 📊 Dependencies

| Component | Description           |
| --------- | --------------------- |
| STM32 HAL | I2C Peripheral Access |
| C99       | Language Standard     |

---

## ❗ Known Limitations

* Designed for STM32 HAL I2C, not portable as-is to other I2C frameworks (bare-metal rewrite required)
* No application-layer logic is provided (state machines, UI, etc. out of scope)

---

## ✍️ Future Work

* Advanced ADC data collection helpers
* Optional interrupt-driven status handling
* DMA I2C transfer options

---

## 📞 Contact

For any technical questions or contributions:
📧 **[yunusemre.dunay@gmail.com](mailto:yunusemre.dunay@gmail.com)**
