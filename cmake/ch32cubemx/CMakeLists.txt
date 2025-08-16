cmake_minimum_required(VERSION 3.22)

project(ch32cubemx)
add_library(ch32cubemx INTERFACE)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

target_compile_definitions(ch32cubemx INTERFACE 
#	USE_HAL_DRIVER 
#	STM32F103x6 
#	STM32_THREAD_SAFE_STRATEGY=2
   $<$<CONFIG:Debug>:DEBUG>
#    __GNUC__
    __riscv_xlen=32
    CH32X033F8P6
    DAP_WITH_CDC=1
    MSOS_DESC=2
    USE_MDK_RESET_WORKAROUND=1
)

target_include_directories(ch32cubemx INTERFACE
        ../../Debug
        ../../Core
        ../../Peripheral/inc
        ../../RTOS/include
        ../../RTOS/portable
        ../../RTOS/portable/Common
        ../../RTOS/portable/GCC/RISC-V
        ../../RTOS/portable/GCC/RISC-V/chip_specific_extensions/RV32I_PFIC_no_extensions
        ../../RTOS/portable/MemMang
        ../../DAP
        ../../Middleware
        ../../Tasks
        ../../User
        ../../Vendor
)

target_sources(ch32cubemx INTERFACE
    ../../User/main.c
    ../../User/DAP_custom.c

    ../../DAP/SW_DP.c
    ../../DAP/DAP.c
    ../../DAP/DAP_vendor.c

    ../../Core/core_riscv.c
    ../../Vendor/ch32x035_it.c
    ../../Vendor/system_ch32x035.c
	../../Vendor/ch32x035_usbfs_device.c

    ../../Peripheral/src/ch32x035_adc.c
    ../../Peripheral/src/ch32x035_awu.c
    ../../Peripheral/src/ch32x035_dbgmcu.c
    ../../Peripheral/src/ch32x035_dma.c
    ../../Peripheral/src/ch32x035_exti.c
    ../../Peripheral/src/ch32x035_flash.c
    ../../Peripheral/src/ch32x035_gpio.c
    ../../Peripheral/src/ch32x035_i2c.c
    ../../Peripheral/src/ch32x035_iwdg.c
    ../../Peripheral/src/ch32x035_misc.c
    ../../Peripheral/src/ch32x035_opa.c
    ../../Peripheral/src/ch32x035_pwr.c
    ../../Peripheral/src/ch32x035_rcc.c
    ../../Peripheral/src/ch32x035_spi.c
    ../../Peripheral/src/ch32x035_tim.c
    ../../Peripheral/src/ch32x035_usart.c
    ../../Peripheral/src/ch32x035_wwdg.c

    ../../RTOS/croutine.c
    ../../RTOS/event_groups.c
    ../../RTOS/list.c
    ../../RTOS/queue.c
    ../../RTOS/stream_buffer.c
    ../../RTOS/tasks.c
    ../../RTOS/timers.c
    ../../RTOS/portable/MemMang/heap_4.c
    ../../RTOS/portable/GCC/RISC-V/port.c
    ../../RTOS/portable/GCC/RISC-V/portASM.S
    ../../RTOS/portable/Common/mpu_wrappers.c

    ../../Middleware/usb_desc.c
    ../../Middleware/usbqueue.c
    ../../Middleware/IRQ_Wrapper.S

    ../../Debug/debug.c

    ../../Tasks/taskDAP.c
    ../../Tasks/taskLED.c
    ../../Tasks/taskSER.c

    ../../Startup/startup_ch32x035_highcode_machine.S
)

target_link_directories(ch32cubemx INTERFACE
)

target_link_libraries(ch32cubemx INTERFACE
)

# Validate that ch32cubemx code is compatible with C standard
if(CMAKE_C_STANDARD LESS 11)
    message(ERROR "Generated code requires C11 or higher")
endif()


