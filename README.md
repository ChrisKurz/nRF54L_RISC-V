# RISC-V of the nRF54L Series

There are several ways to work with the nRF54L Series RISC-V. These are:
- __Run a Zephyr project__: Similar to an ARM Cortex-M33 project, the Zephyr RTOS is used here. The use of Zephyr drivers is possible in this case. [Here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/nrf54l/vpr_flpr.html) you find further information.
- __Soft Peripherals__: Soft peripherals are emulated peripheral functions that are emulated using RISC-V hardware. The RISC-V binary is provided by Nordic. This is usually a library that is integrated directly into the ARM Cortex-M33 and already contains the binary for the RISC-V. A list of existing Soft Peripherals can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrfxlib/softperipheral/README.html#soft-peripherals).
- __Bare-metal solution__: A bare-metal project for RISC-V can be created using a framework. This means that the RISC-V code runs without the Zephyr kernel (e.g. scheduler). Further info about the framework can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/coprocessors/hpf.html#hpf-index). Note that the _high performance framework_ is currently in experimental state.

Further information on development with coprosessor (RISC-V) can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/coprocessors/index.html).

This repository covers the use of RISC-V based on a Zephyr project. 


## Table of Content

- [Let's do a first test: running hellow world on RISC-V]()
