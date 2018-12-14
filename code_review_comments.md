# Code Review Comments

## Comments From David Frey - 20181214
It seems that the comments below have not been addressed yet. I am will try to only list new issues
here. Please address the issues below as well.
* `ourfb_driver.driver.name` should be something like "waveshare\_fb\_spi" since it should be
  capable of supporting more than just the 2.13 Inch version.
* `ourfb_driver.id_table` is where the part differentiating data should be.  It should be something like this:
```
enum waveshare_devices {
	DEV_WS_213,
	DEV_WS_27,
};

struct waveshare_eink_device_properties {
	unsigned int width;
	unsigned int height;
	unsigned int bpp;
};

static struct waveshare_eink_device_properties devices[] = {
	[DEV_WS_213] = { .width=128, .height=250, .bpp=1 },
	[DEV_WS_27]  = { .width=176, .height=264, .bpp=1 },
};


static struct spi_device_id waveshare_eink_tbl[] = {
	{ "waveshare_213", (kernel_ulong_t)&devices[DEV_WS_213] },	
	{ "waveshare_27",  (kernel_ulong_t)&devices[DEV_WS_27] },	
	{ },	
};
```
This eliminates the need for all of the module parameters in `fbws.c`.  I think we should be careful
about claiming support for devices that we haven't tested.  So unless the devies has been tested or
at least a thorough comparison of the datasheets has been performed, we should not claim support.
It you have a good idea of what devices are likely to be supported, then a comment noting that would
be helpful.

There seems to be some confusion about init vs. probe. The `ourfb_init` function seems to be an init
function, but it is also set as the `.probe` member of `ourfb_driver`. I think this is the wrong
approach. Instead, there should be a function `int waveshare\_fb\_probe(struct spi_device *)` which
is a proper probe function and the init function should be removed. The `module_spi_driver()` macro
should be used to generate the relevant `__init` and `__exit` code.

... To be continued


## Comments From David Frey - 20181205
* `fbws.c/h` should be named to something more descriptive.  Maybe `fb_waveshare_213.c/h`
* Format code according to the [Linux kernel coding
  style](https://www.kernel.org/doc/html/v4.19/process/coding-style.html). This will result in many
  changes including, but not limited to:
  * remove trailing whitespace at end of lines
  * insert spaces after commas between params
  * insert spaces around binary operators
  * line length not greater than 80 columns
* Remove all commented out code. Debugging messages should be left in, but should use `dev_dbg()`
* What is the meaning of "ourfb"?  I think "ws213fb" is a better prefix.
* Why is `rpifb.c` named the way that it is?  Wouldn't `mangOH_red_ws213.c` be a better name?
* It seems that `rpifb.h` isn't included by either `fbws.c` or `rpifb.c`. If the file isn't
  required, delete it.
* `struct our_function` in fbws.h isn't used.  Delete it.
* `struct ourfb_function` in fbws.h isn't used.  Delete it.
* `enum ourfb_cmd` in fbws.h isn't used.  Delete it.
* fbws.h declares `struct ourfb_par`. What is the meaning of "par"? Is there a better name that
  could be chosen?
* fbws.h should only contain definitions that are required in multiple C files. If it's only needed
  in fbws.c, then just put it in fbws.c. I think the only thing that rpifb.c actually uses from
  fbws.h is `struct ourfb_platform_data`.


  
