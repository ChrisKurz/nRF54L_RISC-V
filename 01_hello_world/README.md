# Running _hello world_ on RISC-V

Let's start with a very simple project. We will use the _hello world_ example from Zephyr and run it on both the ARM Cortex-M33 and the RISC-V 
(on the nRF54L15, this is the FLPR). Since this existing sample also mentions the board target in the debug output, we have an easy way to see 
which CPU executed the code. 

In this hands-on session, I will try to build an understanding of how the individual projects are generally handled — i.e. the ARM Cortex-M33 project and the RISC-V project. 
We will therefore manually download the individual project images to the development kit.  Later, in another hands-on session, we will look at how both projects can be generated and downloaded together using SYSBUILD. 

> __Note:__ This example has been tested with nRF Connect SDK v3.2.1.


## Step-by-Step Description

1) We prepare the directory and file structure. The following directories and files are required:

       hello_world/ 
       │
       ├── app_arm/        <-- Code for ARM Cortex-M33 
       │    ├── CMakeLists.txt 
       │    ├── prj.conf 
       │    │    └── src/main.c
       │
       ├── app_riscv/      <-- Code for RISC-V Core 
       │    ├── CMakeLists.txt 
       │    ├── prj.conf 
       │    │    └── src/main.c

> __NOTE:__ You can copy Zephyr's _hello world_ project (zephyr/samples/hello_world) into folder __app_arm__ and __app_riscv__. 


### Let's first build the RISC-V project

2) Create a project for RISC-V by clicking __+ Open an existing application__ in Visual Studio Code under nRF Connect. Select the __app_riscv__ directory there.
3) Now the build configuration must be added to the project. We use the following settings for this:
   > - __Board target:__ nrf54l15dk/nrf54l15/cpuflpr

> __Note:__
> There are two board targets for the RISC-V:
> - nRF54L15DK/nrf54l15/cpuflpr <br>
>   This is the recommended board target for the RISC-V. Although the code is placed in non-volatile memory, processing is done in SRAM. The ARM Cortex-M33 must therefore first copy the code to the SRAM before the RISC-V can start.
> - nrf54l15dk/nrf54l15/cpuflpr/xip <br>
>   With this board target, the RISC-V code is executed from non-volatile memory. Copying to SRAM is not necessary here.

4) Then press the "Generate and Build" button.
5) We have now generated the Intel Hex file for the RISC-V. We can find it in the following build directory:

    app_riscv/build/app_riscv/zephyr/zephyr.hex

> __Note:__
> The Intel Hex file _merged.hex_ is located in the __app_riscv/build/__ directory. It contains the RISC-V code and a VPR launcher code snippet for the ARM Cortex-M33. If we program the _merged.hex_ file on an nRF54L15DK, we see that the ARM Cortex-M33 starts executing the code, copies the RISC-V code to the SRAM, and then starts the RISC-V. As expected, the RISC-V then sends a message via the virtual COM interface. The ARM Cortex-M33 would only output its Zephyr boot banner in its terminal.

> __Note:__ This Intel hex file is also stored in this GitHub repository in the [__Intel-Hex files__](Intel-Hex_files) directory under the name __app_riscv_zephyr.hex__.

### Let's build the ARM Cortex-M33 project

6) Create a project for ARM Cortex-M33 by clicking __+ Open an existing application__ in Visual Studio Code under nRF Connect. Select the __app_arm__ directory there.
7) Now the build configuration must be added to the project. We use here the following settings for this:
   > - __Board target:__  nrf54l15dk/nrf54l15/cpuapp
   > - __Snippets:__ nordic_flpr

> __Note:__
> By selecting the snippet _nordic-flpr_, the required VPR launcher is integrated into the project. When selecting the snippet, it is important to choose the appropriate solution that was also used in the RISC-V project. In other words, choose between RISC-V code execution in SRAM (_nordic-flpr_) or in non-volatile memory (_nordic-flpr-xip_).

8) Then press the "Generate and Build" button.
9) We have now generated the Intel Hex file for the ARM Cortex-M33. We can find it in the following build directory:

    app_arm/build/app_arm/zephyr/zephyr.hex

> __Note:__ This Intel hex file is also stored in this GitHub repository in the [__Intel-Hex files__](Intel-Hex_files) directory under the name __app_arm_zephyr.hex__.

## Testing

### Check when VPR_Launcher is executed

As described above, we used the __nordic-flpr__ snippet to include the vpr_launcher code into the ARM Cortex-M33 project. Here, we will check when this code is executed. 

10) The __nRF Connect__ Visual Studio Code extension allows us to view the initialization levels in the __Details__ section under __Core overview__. This shows which threads are started during Zephyr RTOS start-up. Various levels (EARLY, PRE_KERNEL_1, PRE_KERNEL_2, POST_KERNEL, or APPLICATION) are listed here. First, we select the __app_arm__ build in __app_arm__ project. __Under Core overview__, we open __PRE_KERNEL_2__. Now we click on ____device_dts_ord_132__. This opens the file with the corresponding thread that is executed here. In our example, the VPR_launcher code opens.

   ![image](images/init_level.jpg)

### Download both Intel Hex files to the nRF54L15DK dev kit

11) Use the __Programmer__ tool from nRF Connect from Desktop and add both zephyr.hex files in the programmer. Then press __Erase & write__.

### Check response with Serial Terminal

12) Open twice the __Serial Terminal__ and connect both to the nrf54l15. However, select different COM ports.
13) You may have to press the RESET button on your dev kit. You should see in both Terminals the "Hello World!" message followed by the board target name. 
