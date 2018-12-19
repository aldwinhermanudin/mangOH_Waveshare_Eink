# Code Review Comments

## Instructions
Remove items from the sections below as part of the commit that resolves the issue. Add new items in
the appropriate sections as they are identified.


## Questions
What is the purpose of the `#ifdef __LITTLE_ENDIAN` sections in the driver?


## Fixes Requested
* defines `WIDTH`, `HEIGHT` and `BPP` should be removed because these aren't known at compile time.
* I think `eink_device->max_speed_hz` is still being initialized incorrectly. It should be
  initialized with the max speed supported by the device, not the max speed supported by the SPI
  master.


## Enhancements Requested
* Use the `module_spi_driver` macro to eliminate `ws213fb_init` and `ws213fb_exit`
* The variables `ws213fb_fix` and `ws213fb_var` don't really help because the values need to be
  written dynamically and the structs are wholly contained within `struct fb_info`. It would be
  better to just initialize the relevant fields in the `probe()`.
* No file, variable or function should contain the term "213" unless it is specifically related to
  that exact model of e-ink display.
* In `mangOH_red_ws213.c`, `ourfb_data` should be renamed to `ws213_pdata`


## Challenges
How should we deal with the fact that this driver depends on features of the kernel that aren't
explicitly selectable using menuconfig? I doubt the system reference team would be happy about us
enabling config for a bunch of device drivers for devices that we don't intend to use so that the
side-effect of getting `sys_imageblit()` (for example) included into the kernel is achieved.
