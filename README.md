# RISC-V of the nRF54L Series

There are several ways to work with the nRF54L Series RISC-V. These are:
- __Run a Zephyr project__: Similar to an ARM Cortex-M33 project, the Zephyr RTOS is used here. The use of Zephyr drivers is possible in this case. [Here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/nrf54l/vpr_flpr.html) you find further information.
- __Soft Peripherals__: Soft peripherals are emulated peripheral functions that are emulated using RISC-V hardware. The RISC-V binary is provided by Nordic. This is usually a library that is integrated directly into the ARM Cortex-M33 and already contains the binary for the RISC-V. A list of existing Soft Peripherals can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrfxlib/softperipheral/README.html#soft-peripherals).
- __Bare-metal solution__: A bare-metal project for RISC-V can be created using a framework. This means that the RISC-V code runs without the Zephyr kernel (e.g. scheduler). Further info about the framework can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/coprocessors/hpf.html#hpf-index). Note that the _high performance framework_ is currently in experimental state.

Further information on development with coprosessor (RISC-V) can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/app_dev/device_guides/coprocessors/index.html).

This repository covers the use of RISC-V based on a Zephyr project. 


## Table of Content

- [Let's do a first test: running _hello world_ on RISC-V](01_hello_world/README.md)

- Using SYSBUILD to handle ARM Cortex project and RISC-V project in the same VS Code project. Please take a look at _nRF Connect SDK Intermediate_ course of Nordic's Developer Academy. The appropriate exercise can be found [here](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-8-sysbuild/topic/exercise-2-adding-custom-image/).

   > __Note:__ This Developer Academy course disables the Nordic-specific Partition Manager, and the memory partitions are defined via the device tree file. Currently, this only works if the ARM Cortex application is placed in the secure domain (i.e. if _nRF54L15DK/nRF54L15/cpuapp_ is selected as the board target).

- Passing data between ARM Cortex-M33 and RISC-V: The two cores communicate with each other via a shared memory and the ability to notify the other core of new data via an interrupt. Zephyr contains software libraries that support this concept. Although OpenAmp and RPMsg are available, they are too complex for easy implementation with the nRF54L. The [ICMsg](https://docs.nordicsemi.com/bundle/ncs-latest/page/zephyr/services/ipc/ipc_service/backends/ipc_service_icmsg.html#ipc-service-backend-icmsg) software library is more commonly used here. Examples of the ICMsg Library can be found [here](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/samples/ipc/ipc_service/README.html).
